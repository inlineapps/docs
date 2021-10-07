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
## 安全
inline 使用 `X-Hub-Signature` 機制來確保 webhook 安全性，您可以參考 https://developer.github.com/webhooks/securing/ 來驗證 webhook 的來源與內容是否可信。需注意的是，要用 payload 依照 key 的名稱遞增排序之後，再 sha1 hash。

## 重試機制
當 inline 在 webhook POST 失敗時，會每 5 秒重試一次，到第 60 秒時試最後一次。若依舊失敗，則此次 webhook event 傳送將會放棄。

# 事件列表

## 訂位相關
event.type =
- reservation-created: 訂位建立
- reservation-canceled: 訂位取消
- reservation-uncanceled: 訂位加回
- reservation-time-changed: 訂位時間變更
- reservation-pax-changed: 訂位人數變更
- reservation-seated: 訂位入座
- reservation-table-changed: 訂位換桌
- reservation-table-cleaned: 訂位清桌

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
    "email": "rsv@inlineapps.com",
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
  "customer": {
    "customerId": "my-id",
    "name": "inline customer",
    "phoneNumber": "+886988111222", //如果餐廳有開放分享顧客資訊才會顯示
    "email": "rsv@inlineapps.com",
    "gender": "male",
    "language": "zh",
  },
  "contact": {	//如果該筆訂位有代訂人才會顯示
    "customerId": "my-id",	
    "name": "inline customer",	
    "gender": "male",	
    "phoneNumber": "+886988111222",	
    "language": "zh"	
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

## 訂餐相關
event.type =
- order-created: 訂單建立
- order-accepted: 訂單接受
- order-ready: 訂單備餐完成
- order-cancelled: 訂單取消
- order-pickedup: 訂單取餐

event.data =
```
{
  "id": "string", //訂單編號
  "serial": "string", //訂單序號
  "companyId": "string",
  "branchId": "string",
  "order": {
    "total": 880, //總金額
    "serviceFee": 0, //服務費 (如果有)
    "tax": 0, //消費稅 (如果有)
    "deliveryFee": 0, //運費 (外送才有)
    "depositAmount": 0, //訂金 (如果有)
    "currency": "TWD", //幣別
    "subtotal": 880, //產品小計
    "items": [
      {
        "itemId": "5705", //inline 產品編號
        "orderItemId": "4328_5705_1", //訂單內產品編號
        "modifierFor": null,
        "modifierId": null,
        "modifierName": null,
        "parentItemId": null,
        "quantity": 1, //數量
        "promotionId": null, //滿額贈或滿額加價購活動 ID
        "promotionPrice": null, //滿額贈或滿額加價購活動折扣後金額
        "price": 880, //單價
        "note": "item note", //消費者所留產品備註
        "specialPrice": null, //促銷價格
        "prepayPrice": null, //產品訂金
        "name": { //品名
          "en": "Sample Meal Set",
          "zh-tw": "範例套餐"
          },
        "modifiers": [
          {
            "itemId": "5030",
            "orderItemId": "4328_5705_1_5030_0", //訂單內產品編號
            "modifierFor": "4328_5705_1", //子選項從屬的訂單內產品編號
            "modifierId": "1", //子選項類別 ID
            "modifierName": { //子選項類別名稱
              "zh-tw": "ENTRÉE 前菜"
              },
            "parentItemId": 5705, //從屬產品 inline 產品編號
            "quantity": 1, //數量
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
    "gifts": [] //行銷活動
     }
    ],
  "state": "pickedup", //訂單狀態，有 
  "delivery": { //運送地址
    "address": {
      "line1": "string",
      "line2": "string",
      "lat": 25.0766335,
      "lng": 121.5270664
    },
  "note": "string", //消費者給外送員的備註
  "contactMethod": "outside-door", //取餐方式
  "carrier": {
    "provider": "string", //外送單位
    "lookupURL": "string", //外送狀態追蹤網頁 (如果有)
    "driver": "Driver name", //外送員姓名
    "phone": "string", //外送員電話
    "license": "string", //外送員車牌
    "note": "string" //外送員留下的備註
    }
  },
  "note": "string", //消費者所留的訂單備註
  "estimatedReadyTime": 1618489200000, //預計備餐完成時間
  "createdAt": 1618487374999, //訂單建立時間
  "acceptedAt": 1618487444752, //接單時間
  "readyAt": 1618487482658, //實際備餐完成時間
  "pickedupAt": 1618487580310, //取餐時間
  "updatedAt": 1618487580310,
  "payment": {
    "method": "string", //付款方式
    "status": "paid" //付款狀態
    },
  "customer": {
    "customerId": "string",
    "gender": "FEMALE", //消費者性別
    "language": "en", //消費者語言
    "name": "Chieh Chen", //消費者姓名
    "phoneNumber": "+886988111222", //消費者電話
    "email": "rsv@inlineapps.com" //消費者email
    }
  }
}
```
