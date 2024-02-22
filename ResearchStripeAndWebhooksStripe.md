# Research Stripe and Webhooks Stripe

## Stripe
  
1. **API using in Adesso Project**
   
   POST `/v1/payment_intents` -> create new payment intent -> generate client_secert
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

## Webhooks Stripe

Webhooks Stripe là một tính năng quan trọng trong việc xử lý các sự kiện liên quan đến thanh toán trên nền tảng của Stripe. Webhooks cho phép Stripe gửi thông báo tức thời đến các ứng dụng của bạn khi có các sự kiện như thanh toán thành công, hủy đơn hàng, hoàn tiền, và nhiều hơn nữa.

Các bước để sử dụng Webhooks Stripe bao gồm:

1. **Đăng ký URL Webhook**: Đầu tiên, bạn cần đăng ký URL mà Stripe sẽ gửi các thông báo Webhook đến.
2. **Xử lý thông báo**: Khi Stripe gửi thông báo đến URL Webhook của bạn, bạn cần xử lý thông tin được gửi từ Stripe và thực hiện các hành động cần thiết, chẳng hạn như cập nhật trạng thái đơn hàng trong cơ sở dữ liệu của bạn.

## Kết luận

Trong document này, chúng ta đã nghiên cứu về Stripe và tính năng Webhooks Stripe. Hiểu biết sâu sắc về cả hai sẽ giúp bạn tối ưu hóa quá trình thanh toán trực tuyến và cung cấp trải nghiệm người dùng tốt nhất cho khách hàng của mình.
