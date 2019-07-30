# POS Integration Guide

Currently inline can integrate with POS partners for the following scenarios:

1. inline tabling → POS
2. POS tabling → inline
3. inline ↔ POS changing table
4. POS clean table and check out → inline

# inline informs POS

inline uses webhook POST to inform POS about tabling, changing table, cleaning table. And POS can receive the events through webhook endpoint.

The events sent through webhook can be forwarded to different POS clients in different stores, based on companyId & branchId.

POS can do the following things, after receiving the events:

1. Show patron details and `tables` on POS UI.
2. Update the patron status based on `state`.

## Structure

![](https://d2mxuefqeaa7sj.cloudfront.net/s_739C3A445CE0DA65F2D9AF143A27AF7AABDD022DC5721FD2F5AF5C7EA74EE832_1521099875361_file.jpeg)

- [Webhook Spec](./webhook.md)

# POS -> inline

When restaurant staff is operating on POS, the following info can be synced with inline through APIs.

## Tabling

Tabling on POS, this info can be sent to inline through API.

### [POST /reservations/{companyId}/{branchId}](https://partner-api.inline.app/docs/#/reservations/createReservation)
    {
      "customerName": "John",
      "gender": 0, // 0 = Male, 1 = Female, 2 = None
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
          "name": "John",
          "phone": 886988111222,
          "email": "john@inlineapps.com",
          "address": "3F., No. 30-3, Beiping E. Rd., Zhongzheng Dist., Taipei City 100, Taiwan",
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


## Change Table

When changing table happens on POS, inform inline with this info through REST API.

### [PUT /reservations/{companyId}/{branchId}/{reservationId}/tables](https://partner-api.inline.app/docs/#/reservations/assignTables)
    {
      "tables": [
        { "name": "A1" },
        { "name": "A2" }
      ] // table names, inline will try to find the related tableName, and updates the table info to this reservation.
    }

## Clean Table
 
When cleaning table happens on POS, inform inline with this info through REST API.

### [PUT /reservations/{companyId}/{branchId}/{reservationId}/checkout](https://partner-api.inline.app/docs/#/reservations/checkout)
    {
      "customer": { // options
        "name": "customer name",
        "gender": "male",
        "email": "john@inline.tw",
        "phone": "+886988316186",
        "language": "zh-TW"
      },
      "thirdPartyMembers": { // options
        "skm": {
          "id": "member-id-1",
          "type": "vip",
          "provider": "facebook",
          "name": "john",
          "phone": 886988111222,
          "email": "john@inlineapps.com",
          "address": "3F., No. 30-3, Beiping E. Rd., Zhongzheng Dist., Taipei City 100, Taiwan",
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
        "taxId": "52384127",
        "amount": 1200,
        "note": "some notes",
        "url": "link to order",
        "items": [
          {
            "name": "coke",
            "quantity": 2,
            "value": 100,
            "note": "some custome note"
          }
        ]
      }
    }
