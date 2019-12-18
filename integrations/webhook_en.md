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
inline uses `X-Hub-Signature` to ensure webhook security. Please refer to https://developer.github.com/webhooks/securing/ for verifying the data from webhook is trustable or not.

# Event List

## Reservation Related
event.type =
- reservation-created: A reservation is created
- reservation-canceled: A reservation is canceled
- reservation-time-changed: A reservation's time is changed
- reservation-seated: A reservation is seated

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
  "contact": {
    "customerId": "my-id",
    "name": "inline customer",
    "gender": "male",
    "language": "zh"
  },
  "customer": {
    "customerId": "my-id",
    "name": "inline customer",
    "gender": "male",
    "language": "zh",
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
