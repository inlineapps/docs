# App Integration Guide

# Prerequisite

請先透過 inline 取得以下串接資訊：

- API Key
- provider id
- group id
- companyId & branchId mapping tables

# 訂位串接

inline 提供以下 api 供 app 串接

1. 透過 [Group API](https://api.inline.app/docs/#/groups/getBranchInGroupV2)，查詢分店資訊與可訂位時段
1. 透過 [建立訂位 API](https://api.inline.app/docs/#/reservations/createReservation) 建立一筆訂位
1. 後續可透過 [查詢訂位 API](https://api.inline.app/docs/#/third_party/thirdPartyMemberQueryReservations) 查詢該客人曾經的訂位資訊

# 候位串接

inline 提供以下 api 供 app 串接

1. 透過 [Group API](https://api.inline.app/docs/#/groups/getBranchInGroupV2)，查詢分店資訊與可候位時段
```
"waitingInfo": {
        "estimatedWaitingMinutes": 60, //預計等候時間 60 分鐘
        "waitingCount": 3, //隊伍等待組數 3 組
        "status": "open" //候位開放中
      }
```

2. 透過 [建立候位 API](https://api.inline.app/docs/#/waitings/createWaiting) 建立一筆候位

# 優惠券串接

1. [查詢餐廳品牌優惠券 API](https://api.inline.app/docs/#/vouchers/getVouchers) ，取得指定餐廳品牌所有優惠券
1. [發放優惠券 API](https://api.inline.app/docs/#/vouchers/issueThirdPartyMemberVoucher) 將優惠券發給指定會員
1. [取得會員優惠券 API](https://api.inline.app/docs/#/vouchers/getThirdPartyMemberIssuedVouchers) ，將可透過會員編號取得該會員擁有優惠券
1. [核銷優惠券 API](https://api.inline.app/docs/#/vouchers/useIssuedVoucher) ，可在 app 上核銷優惠券

# Webhook

App 可透過 webhook 即時接收 inline 訂位、優惠券更新事件

[webhook 文件](./webhook.md)
