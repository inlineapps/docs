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
        "email": "an-email@gmail.com"
      },
      "thirdPartyMember": {
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
        "metadata": {}
      }
    }

# 測試資料

companyId: `-KmdXp8GIqM0ypiX1lV3:inline-live-2a466`

branchId: `-KmdXp8GIqM0ypiX1lV4`

Example:

https://inline.app/booking/-KmdXp8GIqM0ypiX1lV3:inline-live-2a466/-KmdXp8GIqM0ypiX1lV4?pre_filled_form=ewoJImxhbmd1YWdlIjogInpoLVRXIiwKCSJjdXN0b21lciI6IHsKCQkibmFtZSI6ICJLZW4iLAoJCSJnZW5kZXIiOiAwLAoJCSJwaG9uZSI6ICIrODg2OTg4MzE1NTU1IiwKCQkiZW1haWwiOiAiYW4tZW1haWxAZ21haWwuY29tIgoJfSwKCSJ0aGlyZFBhcnR5TWVtYmVyIjogewoJCSJpZCI6ICJtZW1iZXItaWQiLAoJCSJ0eXBlIjogInZpcCIsCgkJInByb3ZpZGVyIjogInByb3ZpZGVyLWlkIiwKCQkibmFtZSI6ICJLZW4iLAoJCSJwaG9uZSI6ICIrODg2OTg4MTExMjIyIiwKCQkiZW1haWwiOiAia2VuQGlubGluZWFwcHMuY29tIiwKCQkiYWRkcmVzcyI6ICLlj7DljJfluILkuK3mraPljYDph43mhbbljZfot6%2FkuIDmrrUxMjLomZ8iLAoJCSJnZW5kZXIiOiAwLAoJCSJiaXJ0aGRheSI6ICIxOTg2LTA2LTA2IiwKCQkibGFuZ3VhZ2UiOiAiemgtdHciLAoJCSJub3RlIjogIkkgYW0gYSBub3RlIiwKCQkiYXZhdGFyIjogImh0dHBzOi8vZXhhbXBsZS5jb20vYXZhdGFyLmpwZyIsCgkJInVwZGF0ZWRUaW1lIjogMTUxNDI0NzM0NTcxMCwKCQkiY3JlYXRlZFRpbWUiOiAxNTE0MjQ3MzQ1NzEwCgl9Cn0%3D
