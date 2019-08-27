# inline web booking form

https://inline.app/booking/{companyId}/{branchId}?pre_filled_form={form object}

透過 input form 參數，可以預先填入 booking system 的 form

input form is a json object encoded in base64 with following format:


    {
      "language": "zh-TW",
      "customer": {
        "name": "inline",
        "gender": 0, //性別，0 = 男, 1 = 女
        "phone": "+886988315555",
        "email": "an-email@gmail.com"
      },
      "thirdPartyMember": {
        "id": "member-id",
        "type": "vip",
        "provider": "web", //please contact inline to provide your provider-id
        "name": "inline",
        "phone": "+886988111222",
        "email": "ken@inlineapps.com",
        "address": "台北市中正區重慶南路一段122號",
        "gender": 0,
        "birthday": "1986-06-06",
        "language": "zh-tw",
        "note": "I am a note",
        "avatar": "https://example.com/avatar.jpg",
        "metadata": {}
      }
    }

# 測試資料

companyId: `-KmdXp8GIqM0ypiX1lV3:inline-live-2a466`

branchId: `-KmdXp8GIqM0ypiX1lV4`

Example:

https://inline.app/booking/-KmdXp8GIqM0ypiX1lV3:inline-live-2a466/-KmdXp8GIqM0ypiX1lV4?pre_filled_form=CiAgICB7CiAgICAgICJsYW5ndWFnZSI6ICJ6aC1UVyIsCiAgICAgICJjdXN0b21lciI6IHsKICAgICAgICAibmFtZSI6ICJpbmxpbmUiLAogICAgICAgICJnZW5kZXIiOiAwLCAvL+aAp+WIpe+8jDAgPSDnlLcsIDEgPSDlpbMKICAgICAgICAicGhvbmUiOiAiKzg4Njk4ODMxNTU1NSIsCiAgICAgICAgImVtYWlsIjogImFuLWVtYWlsQGdtYWlsLmNvbSIKICAgICAgfSwKICAgICAgInRoaXJkUGFydHlNZW1iZXIiOiB7CiAgICAgICAgImlkIjogIm1lbWJlci1pZCIsCiAgICAgICAgInR5cGUiOiAidmlwIiwKICAgICAgICAicHJvdmlkZXIiOiAid2ViIiwgLy9wbGVhc2UgY29udGFjdCBpbmxpbmUgdG8gcHJvdmlkZSB5b3VyIHByb3ZpZGVyLWlkCiAgICAgICAgIm5hbWUiOiAiaW5saW5lIiwKICAgICAgICAicGhvbmUiOiAiKzg4Njk4ODExMTIyMiIsCiAgICAgICAgImVtYWlsIjogImtlbkBpbmxpbmVhcHBzLmNvbSIsCiAgICAgICAgImFkZHJlc3MiOiAi5Y+w5YyX5biC5Lit5q2j5Y2A6YeN5oW25Y2X6Lev5LiA5q61MTIy6JmfIiwKICAgICAgICAiZ2VuZGVyIjogMCwKICAgICAgICAiYmlydGhkYXkiOiAiMTk4Ni0wNi0wNiIsCiAgICAgICAgImxhbmd1YWdlIjogInpoLXR3IiwKICAgICAgICAibm90ZSI6ICJJIGFtIGEgbm90ZSIsCiAgICAgICAgImF2YXRhciI6ICJodHRwczovL2V4YW1wbGUuY29tL2F2YXRhci5qcGciLAogICAgICAgICJtZXRhZGF0YSI6IHt9CiAgICAgIH0KICAgIH0=
