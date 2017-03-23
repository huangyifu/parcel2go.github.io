---
menu:
  main:
    identifier: order
    weight: 10
title: Orders
---

The flow for processing an order is to first [create an order](#Creating an Orders) which if successful will return an OrderId.  This orderid is then used to reference the order when payment using one of the available payment methods.

{{< note title="Note" >}}
All API calls require authentication with bearer tokens via the Authorization header. See [Authentication](/authentication)
{{< /note >}}

## Creating an Orders

See [Swagger Documentation](https://www.parcel2go.com/api/swagger/ui/index#!/Orders/Orders_Post) for an interactive demo.

A successful response will include the orderId allow with links to payment options.

* OrderLineHMmac - This reference is for getting labels or doing any action which would normally require user permissions.
* OrderlineIdMap - This is a map of OrderLineIds to the original request item ids which you assigned.
* OrderLineId - This is the parcel2go reference for this shipment and is an important reference used for contacting parcel2go about a shipment

### Successful Response

The following is a successful response from creating an order.

``` json
{
  "OrderId": "29285115",
  "Links": {
    "payment": "https://www.parcel2go.com/order/payment?id=29285115&hash=6f480df1253fd20796f418c273853a60",
    "PayWithPrePay": "https://www.parcel2go.com/api/orders/29285115/paywithprepay"
  },
  "TotalPrice": 13.43,
  "TotalVat": 2.24,
  "TotalPriceExVat": 11.19,
  "OrderlineIdMap": [
    {
      "OrderLineIdHmac": "40069998|iFtr1p3ZeyVgFkx1M0OwB8Q0e04F5NLGtcXWBjIhAQE=",
      "OrderLineId": "40069998",
      "ItemId": "06364749-1bfa-4152-a8d7-0f0a5d9f602c"
    }
  ]
}

```


## Paying for an Orders

See the Sandbox [Swagger Documentation](https://api.qa.parcelsolutions.net/api/swagger/ui/index#/PayWithPrepay) for an interactive demo.



### Prepay

In order to pay for an order with a customer prepay, a token must be provided which has the [payment scope](/authentication#scopes) and you must be authorised by the user (via implicit flow).  It's also possible to setup prepay so that a single account will be debited for clients which want to use their own account to pay for orders.

### Stored Cards

Coming Soon

### Redirect to parcel2go.com

It's possible to complete the payment on parcel2go.com by redirecting the user to the payment url returned from the /order call. This will complete the order as if it was done with parcel2go.com, all payment option will be available.