# POS Integration Guide
inline 提供以下幾個功能與 POS 串接：

1. inline 帶位 → POS 
2. POS 開桌 → inline
3. inline ↔ POS 換桌
4. inline ↔ POS 清桌
# inline 通知 POS

inline 利用 webhook POST 方式通知 POS 客人帶位、換桌、清桌，POS 提供 webhook endpoint 接收帶位事件。

webhook 可能需要處理事項：

1. 根據 companyId & branchId 發送資訊到不同分店的 POS
2. 將客人資訊與 `tables` 帶入 POS 顯示
3. 根據 `state` 更新客人狀態
## 架構
![](https://d2mxuefqeaa7sj.cloudfront.net/s_739C3A445CE0DA65F2D9AF143A27AF7AABDD022DC5721FD2F5AF5C7EA74EE832_1521099875361_file.jpeg)













## Webhook Format
    {
      "timestamp": 12345678,
      "events": [
        {
          "type": "reservation-seated",
          "timestamp": 3213124,
          "data": {
            ... // depend on event type
          }
        }
      ]
    }
## Event Type

**reservation-seated**
餐廳在 inline 操作客人入座時會傳送此事件

    {
      "id": "-KeXw0wjrlkSlasDdjAk", // id of reservation, length = 20
      "branchId": "-KeXw0ghwejqrlkYyxxB5yw", // 分店 id, length = 20
      "companyId": "-KXKjbMklwejqrlkAYIwbVnO", // 餐廳 id, length = 20
      "customer": {
        "customerId": "-KmWFqwjerlkSlHuAGZB9YJ", // length = 20
        "gender": "male", // "male" / "female" / "none"
        "language": "zh-tw",
        "name": "蔡",
      },
      "customerNote": "sms:我們現在要過去了再多加一人，共四人", // 客人留言
      "estimatedReadyTime": 0, // 候位預計等候時間 in Unix timestamp
      "note": "靠窗", // 餐廳備註
      "groupSize": 3,
      "numberOfKidChairs": 0,
      "numberOfKidSets": 0,
      "reservationTime": 1497846600000, // 訂位時間 in Unix timestamp (ms)
      "reservationType": "booking", // 類型，訂位 "booking", 候位 "waiting", 現場 "walk-in"
      "serialNumber": "041", // 序號
      "state": "created", // 客人狀態：建立 "created", 入座 "seated" , 取消 "cancelled"
      "canceledBy": "host", // host, customer
      "canceledReason": "no-show", // none, no-show
      "stateChangeTime": 1497846621710, // 最後更新時間 in Unix timestamp (ms)
      "createdTime": 1497354178371, // 建立時間 in Unix timestamp (ms)
      "createdFrom": "web", // web / host / facebook
      "tables": [
        { name: "A1" },
        { name: "A2" }
      ],
      "thirdPartyMember": {
        "id": "skm-member-id",
        "phone": null,
        "name": "Ken",
        "provider": "skm",
        "type": "尊榮級",
        "updatedTime": 1497354178371,
        "createdTime": 1497354178371,
        "gender": "male", // male, female, company
        "language": String | Null,
        "metadata": {
          "grade": "S" // for skm only
        } | Null // metadata pass from service
      }
    }

**reservation-switch-tables**
餐廳在 inline 操作客人換桌時會傳送此事件

    {
      "id": "reservation-id",
      "tables": [
        { name: "A1" },
        { name: "A2" }
      ]
    }

**reservation-clean-tables**
餐廳在 inline 操作客人清桌時會傳送此事件

    {
      "id": "reservation-id"
    }
# POS 同步資料回 inline

餐廳在 POS 進行操作時，可透過以下 API 將資訊同步回 inline

# 換桌

POS 發生換桌動作，可利用 REST API 將資訊更新回 inline

## PUT /reservations/{companyId}/{branchId}/{reservationId}/tables
    {
      "tables": ["A1", "A2"] // table names, inline 會試著找到匹配的桌子名稱並將客人桌號更新
    }
# 清桌

POS 清桌，可利用 REST API 將資訊更新回 inline

## PUT /reservations/{companyId}/{branchId}/{reservationId}/clean
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
        "items": [
          {
            "name": "可樂",
            "quantity": 2
          }
        ]
      }
    }
# 開桌

POS 開桌，可利用以下 API 將傳送開桌資訊給 inline

## POST /v2/reservations/{companyId}/{branchId}
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
      "type": "walk-in"
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

