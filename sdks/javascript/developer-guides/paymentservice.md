# Payment Service

Hive JS SDK provides the payment service which can be used to upgrade the vault. 
Payment service use smart contract to do real pay and then use the paid order id to upgrade the vault.

## Place Order

Before do real payment, place order by Hive JS SDK can get 
the target address (node owner's wallet address) and the order proof.

```
const subscription = new VaultSubscription(<app_context>, <provider_address>);
const order: Order = await subscription.placeOrder('Rookie');
```

## Pay Order

The order information got by place-order can be used to do real payment.
Here [Hive Payment JS SDK](https://github.com/elastos-trinity/Hive.Payment.JS.SDK/tree/master) is required.
All information to pay comes from order information.

```
const amount = order.getPaymentAmount().toString();
const to = order.getReceivingAddress(); // wallet address of the hive node owner.
const memo = order.getProof();
const orderId = await this.paymentContract.payOrder(amount, to, memo);
```

## Settle Order

As the real payment is over, the order id should be settled to upgrade the vault.
After settling the order, the payment process is over.

```
const receipt: Receipt = await subscription.settleOrder(orderId);
```

## Get Orders

User can get all orders information which already paid by himself.

```
const order: Order = await subscription.getOrder(1); // get order by order id.
const orders: Order[] = await subscription.getOrderList(); // get all orders.
```

## Get Receipts

The receipt is the proof of the order which user can keep.

```
const receipts: Receipt[] = await vaultSubscription.getReceipts(1); // get by order id
const receipts: Receipt[] = await vaultSubscription.getReceipts(); // get all receipts
```
