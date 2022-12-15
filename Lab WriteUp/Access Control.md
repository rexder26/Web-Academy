# Title: Access Control , *Web Academy*
- 👨‍💻By: Nathan Hailu
- 🗒Description: Labs are solved and their write up is here.
- 📅Date: Wed 11/23/2022

================= { START } ==============================

A) Vertical Privilege Escalation

    1 -  Unprotected admin functionality

        Some sites will be configuered to access the admin page with just /admin path. or something like that they sometimes put the name in the /robots.txt file.
        -> so if you accessed the /admin page then you have broken access control 

    2 - Unprotected admin functionality with unpredictable URL

        Sites will do the functionality like {1} but the name of the admin page will be un predicatable, but there will be a leak in the source code or some javascript file of the site.

    3 - User role controlled by request parameter

        The access of the admin might be privileged in the cookie with somthing like * admin='false' * so, you can change the admin='false' to admin='true' and BOOM! you got access.

    4 - User role can be modified in user profile

        They add a parameter called 'roleid=a' and with specific id the admin privilege can work. therefore if we try the roleid="$test" and get the admin id number then we are in bros!

    5 - URL-based access control can be circumvented              -> 401 Bypass

        SOme sites wont allow you to access their admin panels. by usingn 'DENY: POST, /admin/deleteUser, managers' this kind thing on the front-end. but hackers can bypass this with X-Original-URL and X-Rewrite-URL. If a web site uses rigorous front-end controls to restrict access based on URL, but the application allows the URL to be overridden via a request header

        Support for these headers lets users override the path in the request URL via the X-Original-URL or X-Rewrite-URL HTTP request header and allows a user to access one URL but have web application return a different one which can bypass restrictions on higher level caches and web servers. 


    6 - Method-based access control can be circumvented            -> 401 Bypass

        Some web sites will do 401(unauthorized) , but you can bypass them by just changing the request method.
        Change request method to GET and use wieners cookie.

B) Horizontal Privilege Escalation

    1 - User ID controlled by request parameter 

        some sites, put users information through id parameters. here if you change the value of the parameter you can get access.
    
    2 - User ID controlled by request parameter, with unpredictable user IDs 

        User's Id sometimes can be unpredictable while on this kind scenario, we can try to get the users profile page or something like that because their will be a user id leak there.
    
    3 - User ID controlled by request parameter with data leakage in redirect 

        Some Pages when you try to use unprivileged pages it will redirect you to the login, but some times the body of the 301 will have the page of the unprivileged body.

C) Horizontal to vertical privilege escalation
    
    1 - User ID controlled by request parameter with password disclosure

        Some Sites will pass the password of a user in the body of the page[hidden], this can be a leak if some one have access to another ppls account with IDOR or some kinda attack.
    
    2 - Insecure direct object references

        soME SITES will give id for their users with predictable so, in this case if a normal user changed the id number and got other users access or changes and got admin access then this site is doomed!

D) Access control vulnerabilities in multi-step processes

    A) Multi-step process with no access control on one step 

        Pages will have different confirmation pages, means... when you click the delete button, 1st, delet 2 > are you sure? yes...no  
        here the requested will be different but at this scenarios the main decsion id done on the second request so if you got that then you have broken access control

    B) Referer-based access control 

        suppose an application robustly enforces access control over the main administrative page at /admin, but for sub-pages such as /admin/deleteUser only inspects the Referer header. If the Referer header contains the main /admin URL, then the request is allowed. 

    








































################################# NOTES ########################################
Hey, I think you are using X-Original-Url / X-Rewrite-Url vector in a wrong way. These headers usually help to bypass front server rules, which are based on URI, but you don't change URI while using these headers.

First, normal request returns 403:

GET /.git/ HTTP/1.1
Host: example.com

This attempt to bypass will return 403 too, because URI hasn't changed and the rule still applies:

GET /.git/ HTTP/1.1
Host: example.com
X-Rewrite-URL: /.git/

This one should bypass the restriction:

GET / HTTP/1.1
Host: example.com
X-Rewrite-URL: /.git/
