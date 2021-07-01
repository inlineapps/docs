# Webhook
inline can send webhook events to a specified http endpoint.

# Spec
webhook events are sent through Http POST, and in application/json:
```
{
  "timestamp": 1524181344796,
  "events": [
    {
      "type": "event-type",
      "data": {
        // depend on event type
      },
      "timestamp": 1524181344796
    },
    ...
  ]
}
```
## Security
inline uses `X-Hub-Signature` to ensure webhook security. Please refer to https://developer.github.com/webhooks/securing/ for verifying the data from webhook is trustable or not. Please note, the sha1 secret is created from payload sorted by key in asc order.

## Retry Rules
When inline POST a webhook event but gets a response of fail, it'll retry every 5 seconds. And it'll retry at most to the 60th seconds. If it still fail, inline will stop sending this event.

# Event List

## Reservation Related
event.type =
- reservation-created: A reservation is created
- reservation-canceled: A reservation is canceled
- reservation-time-changed: A reservation's time is changed
- reservation-seated: A reservation is seated
- reservation-table-changed: A reservation has table assigned to another one
- reservation-table-cleaned: A reservation is checked out

event.data =
```
{
  "id": "string",
  "tables": [
    { "name": "A1" }
  ],
  "reservationLink": "http://to/reservation",
  "branchId": "string",
  "companyId": "string",
  "customerNote": "string",
  "estimatedReadyTime": 0,
  "note": "string",
  "groupSize": 0,
  "numberOfKidChairs": 0,
  "numberOfKidSets": 0,
  "thirdPartyMember": {
    "memberId": "member-id",
    "type": "vip",
    "provider": "facebook",
    "name": "Ken",
    "phone": "+886988111222",
    "email": "ken@inlineapps.com",
    "address": "3F, No. 30-5號, Beiping East Road, Taipei, Taiwan",
    "gender": 0,
    "birthday": "1986-06-06",
    "language": "zh-tw",
    "note": "I am a note",
    "avatar": "https://example.com/avatar.jpg",
    "metadata": {},
    "updatedTime": 1514247345710,
    "createdTime": 1514247345710
  },
  "order":{
    "id": "a_pos_id",
    "taxId": "12345678",
    "amount": 100,
    "discount": true,
    "items": [
    {
      "name": "item1",
      "quantity": 1.2,
      "value": 100,
      "note": "some note"
    },{
      "name": "item2",
      "quantity": 2,
      "value": 100,
      "note": "some note"
    }
   ]
  },
  "customer": {
    "customerId": "my-id",
    "name": "inline customer",
    "phoneNumber": "+886988111222", //this will show if restaurants agree to share patron in formation
    "email": "rsv@inlineapps.com",
    "gender": "male",
    "language": "zh",
  },
  "contact": {	//this will only show when the reservation comes with a booking agent
    "customerId": "my-id",	
    "name": "inline customer",	
    "gender": "male",	
    "phoneNumber": "+886988111222",	
    "language": "zh"	
  },    
  "reservationTime": 0,
  "type": "waiting",
  "state": "waiting",
  "stateChangeTime": 0,
  "serialNumber": "string",
  "canceledBy": "customer",
  "createdTime": 0,
  "createdFrom": "string"
}
```

## Voucher Related
event.type =
- voucher-issued: A voucher is issued
- voucher-used: A voucher is used

event.data =
```
{
  "ticketId": "a138171",
  "serialNumber": 1,
  "cover": "https://somewhere/to/cover/image",
  "title": "Buy one get one",
  "content": "To celebrate 5-year birthday...",
  "terms": "Can not use this with other vouchers",
  "providerId": "1e911",
  "usedReservationId": "-19982111",
  "usedAt": "2017-07-21T17:32:28Z",
  "createdAt": "2017-07-21T17:32:28Z",
  "expirationType": "never",
  "availableAfter": "2017-07-21T17:32:28Z",
  "availableBefore": "2017-07-21T17:32:28Z",
  "expiredAfter": 14
}
```

## Food Order Related
event.type =
- order-created: An order is created
- order-accepted: An order is accepted by the merchant
- order-ready: An order is ready to be picked up
- order-cancelled: An order is canceled
- order-pickedup: An order is picked up

event.data =
```
{
  "id": "string", //Order ID
  "serial": "string", //Order serial number
  "companyId": "string",
  "branchId": "string",
  "order": {
    "total": 880, //Total amount of price
    "serviceFee": 0, //Service fee (optional)
    "tax": 0, //consumer tax (optional)
    "deliveryFee": 0, //Delivery fee
    "depositAmount": 0, //Deposit (optional)
    "currency": "TWD",
    "subtotal": 880,
    "items": [
      {
        "itemId": "5705", //inline item ID
        "orderItemId": "4328_5705_1", //Item ID of this order
        "modifierFor": null,
        "modifierId": null,
        "modifierName": null,
        "parentItemId": null,
        "quantity": 1,
        "promotionId": null,
        "promotionPrice": null,
        "price": 880,
        "note": "item note",
        "specialPrice": null,
        "prepayPrice": null,
        "name": { //品名
          "en": "Sample Meal Set",
          "zh-tw": "範例套餐"
          },
        "modifiers": [
          {
            "itemId": "5030",
            "orderItemId": "4328_5705_1_5030_0",
            "modifierFor": "4328_5705_1",
            "modifierId": "1",
            "modifierName": {
              "zh-tw": "ENTRÉE 前菜"
              },
            "parentItemId": 5705,
            "quantity": 1,
            "promotionId": null,
            "promotionPrice": null,
            "price": 0,
            "note": null,
            "specialPrice": null,
            "prepayPrice": null,
            "name": {
              "en": "Fried Rice with Crispy Lemongrass Fride Chicken",
              "zh-tw": "香茅炸雞招牌炒飯"
              }
          }
        ],
    "gifts": []
     }
    ],
  "state": "pickedup", 
  "delivery": {
    "address": {
      "line1": "string",
      "line2": "string",
      "lat": 25.0766335,
      "lng": 121.5270664
    },
  "note": "string", //Note from patrons to courier
  "contactMethod": "outside-door",
  "carrier": {
    "provider": "string",
    "lookupURL": "string",
    "driver": "Driver name",
    "phone": "string",
    "license": "string",
    "note": "string"
    }
  },
  "note": "string", //Note from patrons to this order
  "estimatedReadyTime": 1618489200000,
  "createdAt": 1618487374999, 
  "acceptedAt": 1618487444752,
  "readyAt": 1618487482658,
  "pickedupAt": 1618487580310,
  "updatedAt": 1618487580310,
  "payment": {
    "method": "string",
    "status": "paid"
    },
  "customer": {
    "customerId": "string",
    "gender": "FEMALE",
    "language": "en",
    "name": "Chieh Chen",
    "phoneNumber": "+886988111222",
    "email": "rsv@inlineapps.com"
    }
  }
}
```
