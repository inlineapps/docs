# Webhook
inline 可透過 webhook 發送訂位、優惠券相關資訊更新到指定的 http endpoint

# 規格
webhook 一律以 Http POST, application/json 格式發送，格式如下
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

# 事件列表

## 訂位相關
event.type =
- reservation-created: 訂位建立
- reservation-canceled: 訂位取消
- reservation-time-changed: 訂位時間變更
- reservation-seated: 訂位入座

event.data =
```
{
  "id": "string",
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
    "id": "member-id",
    "type": "vip",
    "provider": "facebook",
    "name": "Ken",
    "phone": "+886988111222",
    "email": "ken@inlineapps.com",
    "address": "台北市中正區重慶南路一段122號",
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

## 優惠券相關
event.type =
- voucher-issued: 優惠券發放
- voucher-used: 優惠券使用

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
