# POS Integration Guide
inline provides following operations for integration：

1. seat on inline → POS
1. open a table on POS → inline
1. inline ↔ POS switch table
1. inline ↔ POS clean table

# inline -> POS

inline use webhook http POST to notify POS about a patron seat, switch table, clean table. POS need setup a webhook endpoint to receive related events.

POS webhook needs to dispatch events to POS machine according to companyId & branchId

POS could perform following operations:

1. Open a table that matched `tables` and display customer info
1. Update the state or info of a table

## Architecture
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
The host take a reservation seat on inline

    {
      "id": "-KeXw0wjrlkSlasDdjAk", // id of reservation, length = 20
      "branchId": "-KeXw0ghwejqrlkYyxxB5yw", // length = 20
      "companyId": "-KXKjbMklwejqrlkAYIwbVnO", // length = 20
      "customer": {
        "customerId": "-KmWFqwjerlkSlHuAGZB9YJ", // length = 20
        "gender": "male", // "male" / "female" / "none"
        "language": "zh-tw",
        "name": "蔡",
      },
      "customerNote": "sms: Celerabting birthday",
      "estimatedReadyTime": 0, // estimated ready time for a waiting in Unix timestamp
      "note": "A good customer",
      "groupSize": 3,
      "numberOfKids": 0,
      "numberOfKidChairs": 0,
      "reservationTime": 1497846600000, // A time reservation  in Unix timestamp (ms)
      "reservationType": "booking", // ["booking", "waiting", "walk-in"]
      "serialNumber": "041",
      "state": "created", // ["created", "seated", "cancelled"]
      "stateChangeTime": 1497846621710, // Unix timestamp (ms)
      "createdTime": 1497354178371, // Unix timestamp (ms)
      "tables": [ // POS can use table name to display reservation on tables
        { name: "A1" },
        { name: "A2" }
      ],
      "thirdPartyMember": {
        "id": "skm-member-id",
        "phone": null,
        "name": "Ken",
        "provider": "skm",
        "type": "Premium",
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

The host switch tables on inline

    {
      "id": "reservation-id",
      "tables": [
        { name: "A1" },
        { name: "A2" }
      ]
    }

**reservation-clean-tables**

The host clean a tables on inline

    {
      "id": "reservation-id"
    }

# POS -> inline

When operations is on POS, POS can use following API to sync info back to inline.

## Open a table

### POST /reservations/{companyId}/{branchId}
    {
      "customerName": "Ken",
      "gender": "male", ["male", "female", "none"]
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
          "address": "No. 12, Section 1, Zhongyang North Road, Beitou District",
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


## Switch tables

When change tables on POS, use this API to update the according reservations info.

### PUT /reservations/{companyId}/{branchId}/{reservationId}/tables

    {
      "tables": [ // table names, inline will tried to update the reservation to the tables that matched name
        { "name": "A1" },
        { "name": "A2" }
      ]
    }

## Checkout

When a table checkout on POS, use this API to notify inline.

## PUT /reservations/{companyId}/{branchId}/{reservationId}/checkout
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
          "address": "No. 12, Section 1, Zhongyang North Road, Beitou District",
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
        "items": [
          {
            "name": "Coke",
            "quantity": 2
          }
        ]
      }
    }
