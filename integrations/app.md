# App Integration Guide

# Prerequisite

請先透過 inline 取得以下資訊

- API Key
- provider id
- companyId & branchId mapping tables

# 訂位串接

inline 提供以下 api 供 app 串接

1. 先透過 [查詢可訂位時間 API](https://api.inlineapps.com/docs/#/bookings/getBookingCapacitiesV2)，查詢分店可訂位時段
2. 透過 [建立訂位 API](https://api.inlineapps.com/docs/#/reservations/createReservation) 建立一筆訂位
3. 後續可透過 [查詢訂位 API](https://api.inlineapps.com/docs/#/third_party/thirdPartyMemberQueryReservations) 查詢該客人曾經的訂位資訊

# 優惠券串接

1. [查詢餐廳品牌優惠券 API](https://api.inlineapps.com/docs/#/vouchers/getVouchers) ，取得指定餐廳品牌所有優惠券
2. [發放優惠券 API](https://api.inlineapps.com/docs/#/vouchers/issueThirdPartyMemberVoucher) 將優惠券發給指定會員
3. [取得會員優惠券 API](https://api.inlineapps.com/docs/#/vouchers/getThirdPartyMemberIssuedVouchers) ，將可透過會員編號取得該會員擁有優惠券
4. [核銷優惠券 API](https://api.inlineapps.com/docs/#/vouchers/useIssuedVoucher) ，可在 app 上核銷優惠券
