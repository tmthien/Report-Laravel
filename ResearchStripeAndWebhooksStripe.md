# Research Stripe and Webhooks Stripe

## Stripe
  Add `Publishable key` on FE's source
  Add `Secret key` on BE's source
  
**API using in Adesso Project**
   
   POST `/v1/payment_intents` -> create new payment intent
  ```
  $stripe = new StripeClient(config('payment.stripe.secret_key'));
  
  $intent = $stripe->paymentIntents->create([
    'amount' => $amount,
    'currency' => 'usd',
    'automatic_payment_methods' => ['enabled' => true],
  ]);
  ```

  - `amount` intended to be collected by this PaymentIntent
  - `currency` Three-letter ISO currency code, in lowercase. Must be a supported currency.
  - `automatic_payment_methods` When you enable this parameter, this PaymentIntent accepts payment methods that you enable in the Dashboard and that are compatible with this PaymentIntent’s other parameters.

  GET `v1/payment_intents/id` -> get payment intent with `$id = $intent->id`

```
  $stripe = new StripeClient(config('payment.stripe.secret_key'));

  $intent = $stripe->paymentIntents->retrieve($data['payment_intent']);
```
## Webhooks Stripe
  - Add endpoint_secret(Get in Webhooks Stripe)
    <img width="736" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/2ea932da-9a06-464e-92ed-a3bc4b0b6561">

  - Create a controller that can handle the events received from Webhooks Stripe.
   ```
    public function updatePaymentStatus(Request $request)
    {
        $payload = $request->getContent();
        $sigHeader = $request->header('Stripe-Signature');
        $endpointSecret = config('payment.stripe.webhook_secret');

        $event = null;

        try {
            $event = Webhook::constructEvent($payload, $sigHeader, $endpointSecret);
        } catch (\UnexpectedValueException $e) {
            return response()->json(['error' => 'Invalid payload'], 400);
        } catch (\Stripe\Exception\SignatureVerificationException $e) {
            return response()->json(['error' => 'Invalid signature'], 400);
        }

        switch ($event->type) {
            case 'payment_intent.succeeded':
                $intentId = $event->data->object->id;
                $paymentIntent = app(PaymentIntentRepository::class);

                $record = $paymentIntent->getByFields([
                    'intent_id' => $intentId
                ]);
                if($record) {
                    switch ($record->type) {
                        case 'login':
                            $userRepo = app(UserRepository::class);
        
                            $user = $userRepo->find($record->user_id);
        
                            if (!empty($user->id)) {
                                $user->payment_status = 1;
                                $user->save();
                            }
                            break;
                    }
                    return true;
                };
                break;
            default:
                return response()->json(['error' => 'Unhandled event type'], 400);
        }

        return response()->json(['success' => true]);
    }
   ```
  - Create endpoint on BE's source
    ![image](https://github.com/tmthien/Report-Laravel/assets/93562815/cdb69de7-70fd-4242-afba-3e55c8b625fb)

  - Add endpoint to Dashboard Webhooks Stripe
    <img width="724" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/8b25e146-43a1-42cf-a041-eea122d9032c">

  - Select events to listen
    <img width="753" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/5bf7e5df-b3c3-493d-8a71-739191e48200">

## Flow Payment of Adesso
  - User will register a new account and add a coupon received (for example, 20%, 50%, 100%, 20$, 100$, etc.).
    <img width="1143" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/d9e57ecc-9423-44d2-bb71-9e6327afd8eb">

  - Back-end (BE) will receive a request from the user to enter a coupon and calculate the amount.
    ```
    public function reduceAmount($type, $value, $amount)
    {
        switch ($type) {
            case 'percent':
                $amount = $amount * (100 - $value) / 100;
            case 'amount':
                $amount = $amount - $value * 100;
        }

        return floor($amount);
    }
    ```
    
  - BE will then call the API /v1/payment_intents and return the client_secret key to the front-end (FE) side.
    ```
    public function getStripeToken($amount)
    {
        $user = auth()->user();
        $couponId = $user->coupon_id;

        // get the correct amount
        if (!empty($couponId)) {
            $coupon = app(CouponRepository::class);

            $record = $coupon->getByFields([
                'id' => $couponId
            ]);

            if (!empty($record->id)) {
                $amount = $this->reduceAmount($record->type, $record->value, $amount);
            }
        }

        if($amount == 0 || $amount < 0) {
            $user->update([
                'payment_status' => 1,
            ]);

            return [
                'client_secret' => null,
            ];
        }

        $stripe = new StripeClient(config('payment.stripe.secret_key'));

        $intent = $stripe->paymentIntents->create(
            [
                'amount' => $amount,
                'currency' => 'usd',
                'automatic_payment_methods' => ['enabled' => true],
            ]
        );

        // todo store intent to database
        $payment = app(PaymentIntentRepository::class);
        $payment->insertRecord([
            'intent_id' => $intent->id,
            'user_id' => $user->id,
            'type' => 'login'
        ]);

        return [
            'client_secret' => $intent->client_secret
        ];
    }
    ```
  - FE will use the client_secret key to show the payment page with the calculated amount.
    <img width="1144" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/c5621d0d-2870-40cf-833e-4231310fdba1">

  - When User submits the payment, Webhooks Stripe will trigger an event to the BE side.
    <img width="861" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/60cbcbeb-0e47-4365-b101-71a46f458c1b">

  - BE side will update the payment_status of the user when the status of the event is 'succeeded'.
    ```
    switch ($event->type) {
            case 'payment_intent.succeeded':
                $intentId = $event->data->object->id;
                $paymentIntent = app(PaymentIntentRepository::class);

                $record = $paymentIntent->getByFields([
                    'intent_id' => $intentId
                ]);
                if($record) {
                    switch ($record->type) {
                        case 'login':
                            $userRepo = app(UserRepository::class);
        
                            $user = $userRepo->find($record->user_id);
        
                            if (!empty($user->id)) {
                                $user->payment_status = 1;
                                $user->save();
                            }
                            break;
                    }
                    return true;
                };
                break;
            default:
                return response()->json(['error' => 'Unhandled event type'], 400);
        }
    ```
  - Finally, User will be redirected to the Heart Score page.
  <img width="1638" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/a3264617-9d4f-45b1-8d89-c50ac39ca90f">
