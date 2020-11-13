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
        "id": "member-id", //your member id
        "type": "vip",
        "provider": "provider-id", //please contact inline to provide your provider-id
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

https://inline.app/booking/-KXIv2zV1CA0OJEVKDDm/-LqZC7ERyO2hLDrIik9P?pre_filled_form=ewogICJsYW5ndWFnZSI6ICJ6aC1UVyIsCiAgImN1c3RvbWVyIjogewogICAgIm5hbWUiOiAiaW5saW5lIiwKICAgICJnZW5kZXIiOiAwCiAgfSwKICAidGhpcmRQYXJ0eU1lbWJlciI6IHsKICAgICJpZCI6ICIxMjMiLAogICAgInR5cGUiOiAidmlwIiwKICAgICJwcm92aWRlciI6ICJnYiIsCiAgICAibmFtZSI6ICJpbmxpbmUiLAogICAgImdlbmRlciI6IDAsCiAgICAiYmlydGhkYXkiOiAiMTk4Ni0wNi0wNiIsCiAgICAibGFuZ3VhZ2UiOiAiemgtdHciCiAgfQp9

# Please Note 注意事項

If the outcome from base64 encoding contains `+` characters, please do URL encoding to replace `+` with `%2B` before appending into target URL. The purpose is to ensure your pre_filled_form can be decode correctly. 
如果 base64 encode 出來的結果帶有 `+` 號，請先做 URL encoding，將 `+` 代換成 `%2B`，再帶入 URL 內，以確保能正確 decode。
