# Payment Service

On node, payment service is for upgrading the vault. Vault user use ELA to get more space for the vault. 
Node owner can get ELA by providing more space to the vault.

Assume a user want to upgrade his vault, how should he do ?

- Get [Payment JS SDK](https://github.com/elastos-trinity/Hive.Payment.JS.SDK/tree/master) first.
- Call place order API (`/api/v2/payment/order`) to get the target address and order proof.
- Use essential app to pay with ELA to get an order ID.
- Call settle order with id (`/api/v2/payment/order/<order_id>`) to fulfill the payment process.
