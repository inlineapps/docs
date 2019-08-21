# POS Integration Guide
inline 提供以下幾個功能與 POS 串接：

1. inline 帶位 → POS
2. POS 開桌 → inline
3. inline ↔ POS 換桌
4. POS 清桌、結帳 → inline
# inline 通知 POS

inline 利用 webhook POST 方式通知 POS 客人帶位、換桌、清桌，POS 提供 webhook endpoint 接收帶位事件。

webhook 需要處理事項可根據 companyId & branchId 發送事件到不同分店的 POS

POS 收到事件後可做以下行為：
1. 將客人資訊與 `tables` 帶入 POS 顯示
1. 根據 `state` 更新客人狀態

## 架構
![](https://d2mxuefqeaa7sj.cloudfront.net/s_739C3A445CE0DA65F2D9AF143A27AF7AABDD022DC5721FD2F5AF5C7EA74EE832_1521099875361_file.jpeg)

- [Webhook Spec](./webhook.md)

# POS -> inline

餐廳在 POS 進行操作時，可透過以下 API 將資訊同步回 inline

## 開桌

POS 開桌，可利用以下 API 將傳送開桌資訊給 inline

### [POST /reservations/{companyId}/{branchId}](https://partner-api.inline.app/docs/#/reservations/createReservation)
    {
      "customerName": "Ken",
      "gender": 0, // 0 = 男, 1 = 女, 2 = 無
      "phone": 886988315555,
      "email": "an-email@gmail.com",
      "language": "zh-TW",
      "groupSize": 2,
      "numberOfKids": 1,
      "numberOfKidChairs": 3,
      "datetime": "2017-12-04T01:50:00Z",
      "note": "This is a note",
      "tables": [
        { "name": "A1" },
        { "name": "A2" }
      ],
      "preferredTableTags": [
        "string"
      ],
      "createdFrom": "pos",
      "shouldNotifyCustomer": false,
      "thirdPartyMembers": {
        "skm": {
          "id": "member-id-1",
          "type": "vip",
          "provider": "facebook",
          "name": "Ken",
          "phone": 886988111222,
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
        }
      },
      "referer": "foo.bar"
    }

**Response**

    {
      "reservationId": "id of reservation"
    }


## 換桌

POS 發生換桌動作，可利用 REST API 將資訊更新回 inline

### [PUT /reservations/{companyId}/{branchId}/{reservationId}/tables](https://partner-api.inline.app/docs/#/reservations/assignTables)
    {
      "tables": [
        { "name": "A1" },
        { "name": "A2" }
      ] // table names, inline 會試著找到匹配的桌子名稱並將客人桌號更新
    }
## 清桌

POS 清桌，可利用 REST API 將資訊更新回 inline

### [PUT /reservations/{companyId}/{branchId}/{reservationId}/checkout](https://partner-api.inline.app/docs/#/reservations/checkout)
    {
      "customer": { // options
        "name": "customer name",
        "gender": "male",
        "email": "ken@inline.tw",
        "phone": "+886988316186",
        "language": "zh-TW"
      },
      "thirdPartyMembers": { // options
        "skm": {
          "id": "member-id-1",
          "type": "vip",
          "provider": "facebook",
          "name": "Ken",
          "phone": 886988111222,
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
        }
      },
      "order": {
        "id": "order-id",
        "taxId": "52384127", // 統編
        "amount": 1200,
        "note": "some notes",
        "url": "link to order",
        "items": [
          {
            "name": "可樂",
            "quantity": 2,
            "value": 100,
            "note": "some custome note"
          }
        ]
      }
    }
