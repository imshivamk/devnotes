## REST API integration

### high level flow:

1. User clicks “Buy Premium” in frontend
2. Frontend calls your backend `/create-order`
3. Backend creates Razorpay order via Razorpay API
4. Backend returns `order_id` to frontend
5. Frontend opens Razorpay Checkout
6. User completes payment
7. Razorpay:
    - Returns success response to frontend
    - Sends a **webhook** to your backend (very important)
8. Backend verifies payment signature
9. Backend updates DB (user tier → pro)
10. Backend responds success

notes : 
Never should be called from frontend : 
	1. Razorpay secret key must never be exposed
	2. Anyone could fake a payment request
	3. You lose control over verification logic


### db schema

1. users

| id  | email | tier |
| --- | ----- | ---- |
2. subscriptions

| id  | user_id | plan_id | status | current_period_end | razorpay_subscription_id |
| --- | ------- | ------- | ------ | ------------------ | ------------------------ |


3. payments


| id  | user_id | razorpay_payment_id | amount | status | created_at |
| --- | ------- | ------------------- | ------ | ------ | ---------- |


### prod-level flow:


1. user clicks on the `buy pro membership` button

```ts
POST /api/payments/create-order
{
  planId: "pro_monthly"
}
```

2. backend creates razorpay order

```ts
const order = await razorpay.orders.create({
  amount: 50000, // in paise
  currency: "INR",
  receipt: "receipt_123"
});
```

the api call returns to the frontend : 

```json
{
  orderId: "order_9A33XWu170gUtm"
}
```

3. Frontend opens the Razorpay checkout interface :

```ts
const options = {
  key: "RAZORPAY_PUBLIC_KEY",
  order_id: orderId,
  handler: function (response) {
    // payment success callback
  }
};

new Razorpay(options).open();
```

on success , razorpay returns :

```json
{
  razorpay_payment_id,
  razorpay_order_id,
  razorpay_signature
}
```

4. Payment verification

frotnend calls api to verify the payment status :

```ts
POST /api/payments/verify

{
  razorpay_payment_id,
  razorpay_order_id,
  razorpay_signature
}
```

backend verifies the signature: 

```ts
const generated_signature = crypto
  .createHmac("sha256", RAZORPAY_SECRET)
  .update(order_id + "|" + payment_id)
  .digest("hex");

if (generated_signature === razorpay_signature) {
   // payment successful
   // db.update(users).set(plan="pro")
}
```

5. Webhooks  

razorpay sends a webhook like 
```ts
POST /api/webhooks/razorpay
```
with event
```ts
payment.captured
```
Backend then verifies webhook signature.

Because:

- User may close browser after payment
- Network may fail
- Frontend callback may not reach backend

> Webhook is authoritative. Frontend success is secondary.