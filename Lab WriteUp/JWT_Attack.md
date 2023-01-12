# Title: JWT Attack , *Web Academy*
- 👨‍💻By: Nathan Hailu
- 🗒Description: Labs are solved and their write up is here.
- 📅Date: Thu 01/12/2023

================= { START } ==============================

A) JWT authentication bypass via unverified signature

    - JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library jsonwebtoken has verify() and decode().

    Occasionally, developers confuse these two methods and only pass incoming tokens to the decode() method. This effectively means that the application doesn't verify the signature at all.
    = The served don't validate if the token is original or not, so we can change it to administrator.
      - eyJraWQiOiI3NGIwNjQ0Ny02N2ZhLTRiMmItYmJkNS1hMWY0ZGJmZjgyN2UiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2NzM1NDk2OTZ9.sNPYnjK3a2yjUzsEUL4VwBxO3u-7Sagqtz-uHGi-y8rdxiKfqwCEi2hzIC0VESWrsBZelrGmSx1SHl6XIouRuWif6XI6PrmmCWqWocoz9MqzlPJtheVK9lAS4QOjVHARGzsuRXwqymPpGDqqry8kYhKaEsuMN6vk3TjPitj26YlB2vFxYUsP4lJO0sepPmdEVRHiBZUyHSGnrqhfxoSN9_L4cEyjfoKXvS-UoNSQBPEKgrYp4PKaOQrt4JSneVqvYIJnKrv_swWYZo-fBptZZT9BgcjDHdp35LIFIFD5LDlGYY3RXoWK3vxBkyfhOvD9Nk2u-ccxs-uC0Jwyp1s1yw

B) 