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
    ![image](https://github.com/tmthien/Report-Laravel/assets/93562815/8fae4c79-8c7f-4fca-ac70-1489b4eedc4f)

  - Create endpoint on BE's source
    ![image](https://github.com/tmthien/Report-Laravel/assets/93562815/cdb69de7-70fd-4242-afba-3e55c8b625fb)

  - Add endpoint to Dashboard Webhooks Stripe
    <img width="724" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/8b25e146-43a1-42cf-a041-eea122d9032c">

  - Select events to listen
    <img width="753" alt="image" src="https://github.com/tmthien/Report-Laravel/assets/93562815/5bf7e5df-b3c3-493d-8a71-739191e48200">

## Flow Payment of Adesso
  - User will register a new account and add a coupon received (for example, 20%, 50%, 100%, 20$, 100$, etc.).
  - Back-end (BE) will receive a request from the user to enter a coupon and calculate the amount.
  - BE will then call the API /v1/payment_intents and return the client_secret key to the front-end (FE) side.
  - FE will use the client_secret key to show the payment page with the calculated amount.
  - When User submits the payment, Webhooks Stripe will trigger an event to the BE side.
  - BE side will update the payment_status of the user when the status of the event is 'succeeded'.
  - Finally, User will be redirected to the Heart Score page.
