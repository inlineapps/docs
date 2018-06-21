# inline web booking form

https://inline.app/booking/{companyId}/{branchId}?pre_filled_form={form object}

透過 input form 參數，可以預先填入 booking system 的 form

input form is a json object encoded in base64 with following format:


    {
      "language": "zh-TW",
      "customer": {
        "name": "Ken",
        "gender": 0, //性別，0 = 男, 1 = 女
        "phone": "+886988315555",
        "email": "an-email@gmail.com",
      },
      thirdPartyMember: {
        "id": "member-id",
        "type": "vip",
        "provider": "provider-id",
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
      }
    }

# 測試資料

companyId: `-KmdXp8GIqM0ypiX1lV3`

branchId: `-KmdXp8GIqM0ypiX1lV4`

Example:

https://inline.app/booking/-KmdXp8GIqM0ypiX1lV3/-KmdXp8GIqM0ypiX1lV4?pre_filled_form=eyAiY3VzdG9tZXIiOiB7ICJuYW1lIjogIktlbiIsICJnZW5kZXIiOiAwLCAicGhvbmUiOiAiODg2OTg4MzE1NTU1IiwgImVtYWlsIjogImFuLWVtYWlsQGdtYWlsLmNvbSIsICJsYW5ndWFnZSI6ICJ6aC1UVyIgfSB9Cg==
