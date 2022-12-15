# Title: Authentication , Web Academy
By: Nathan Hailu
Description: Labs are solved and their write up is here.
Date: Tue 11/15/2022

================= { START } ==============================

1 -  Vulnerabilities in password-based login  

	A) Username enumeration via different responses

		- The responses can be "INVALID USERNAME" and "INVALID PASSWORD" this tells as if we got the right user name the response will be "INVALID PASSWORD" so we try to bruteforce with diffrent usernames and we can get the correct one.

	B) Username enumeration via subtly different responses

		- The responses will be very same when the Correct username is entered and invalid is entered, but there will be a slight diffrence so try to analyse that by using "Grep Extract" features of burp.

	C) Username enumeration via response timing

		- Some Applications will, do the correct username to take more duration to check the password, mean while hackers can do bruteforce and get the right username by seeing the longer response time.

	D) Broken brute-force protection, IP block

		- Applications will block your IP if you do more invalid credentials using Bruteforce ways, but hackers can bypass this using "X-Forwarded-For: $num" Headers. if they sent each invalid username and pass with different header value the site won't block you.
			-> X-Forwarded-For: <IP> , this is used to identify the original senders IP. if you use VPN's kind they might get ur original IP.

	E) Username enumeration via account lock

		- Applications will block you if you failed a try for 2,3 times and reset the count if you entered the correct credentials, so hackers will bypass this by creating account and, trying the username then insert the their own add the incorrect then their own, this wont block them.

	F) Broken brute-force protection, multiple credentials per request

		- Some Requests will send the username and password value in JSON format, so if we added our password list in the JSON password field, we can crack it.

2 -  Vulnerabilities in multi-factor authentication

	A) 2FA simple bypass

		- Some Sites will ask for OTP code in different page rather than the login, here the site know that u r logged it, so if you have the path for my profile or other pages on the site by just editing the url to that path you can skip the 2FA.

	B) 2FA broken logic

		- Their will be a user id on the Header of the requests that means we can try to get the OTP code with our credentials, then by capturing the request we can bruteforce the OTP of the victim by replacing our user id with the victims on the header. -> if rate limit is not implemented.

	C) 2FA bypass using a brute-force attack

		- Some sites will implement rate limit with csrf token, to bypass this we will use some kinda feature of burp that will load the site each time and use the new token to the site. 

3 - Vulnerabilities in other authentication mechanisms
	
	A) Brute-forcing a stay-logged-in cookie

		- When web applications Use the stay-logged-in features they use many kind of teqniches from them, the one is they will encrypt the username and password and put it in the cookie this will make that user to stay logged in, but if hackers knew this it will create them a chance to bruteforce by encoding and pluging their wordlists.

			base64(username+':'+md5HashOfPassword)

	B) Offline password cracking

		- If site have the Upper vulerability(A) and also have some kinda cookie stealing vuln like XSS we can combine and steal his cookie so as we saw from (A) we can decode the cookie and get the users password.

	C) Password reset broken logic

		- When we ask the site for forgotted password reset, the site sends some link that have a random and uniqeu token that possesses to our account. we cant break this but if we intercept the request and send the new password after we accessed the password reseting page, we might have access to change other users pass to because of the broken logic. like below

			temp-forgot-password-token=FHxuvaqUKBZ31QE5PYpJabawSdcc57vM&username=carlos&new-password-1=test&new-password-2=test
 			-> here we can change the user name and we got his password!

 	D) Password reset poisoning via middleware

 		- Sites send the reseting link via emails, but if the site is vulnerable by allowing X-Forwarded-Host we can exploit it by giving value of our server to the X-forwarded-Host.

 			-> X-Forwarded-Host: <Host> , is a Http-Header used to Identify the original Host. so on the normal senarios the email will be sent to our email, but now the emails will be sent/ forwarded to out host. so we will get them. 
	
	E) Password brute-force via password change
		
		- Their will always be a logical flaw, if we see this application. The site have a place to change the Password of a user. { Current,new,retype} if we add the correct current pass and unidedentical new passwords. the site will prompt "New passwords do not match", but if the current password is not correct the site says "Current Password is not same"