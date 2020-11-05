#Install Kali with Vmware
#Download, Install and Use of DVMA as Target
  
  #PhpStudy
    (use Phpstudy for reduce the complexity of setting)
    - lauch Apache, and SQL database
    
  #DVMA
    - decompression of DVMA_Master.zip (which downloaded from ‘http://www.dvwa.co.uk/’) under the folder 'phpstudy_pro/wwww/.'
    - modify suffix of document 'config.inc.php.dist' ->> 'config.inc.php'
    - modify the content of 'config.inc.php' --->>  modify values of 'db_username' and 'db_password' as 'root'
          --> namely, 'db_username'='root' and  'db_password' = 'root'
    - open the dvma webseit 'http://127.0.0.1/setup.php'
    - login with default credential
          --> namely, 'username' = 'admin', 'password' = 'password'
    
    
  #nikto -h www.baidu.com
  #ipconfig /all ipv4 address: 192.168.178.34
  #nikto -h 192.168.178.34
  
        + Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a mod_log_rotate/1.02
        + Cookie PHPSESSID created without the httponly flag
        + Retrieved x-powered-by header: PHP/7.3.4
        + The anti-clickjacking X-Frame-Options header is not present.
        + The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
        + The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
        + Root page / redirects to: login.php OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
        + /login.php: Admin login page/section found.
        + /.gitignore: .gitignore file found. It is possible to grasp the directory structure.
        + 8724 requests: 0 error(s) and 8 item(s) reported on remote host
        + End Time:2020-11-05 23:13:56 (GMT8) (78 seconds)
        + The anti-clickjacking X-Frame-Options header is not present.
        + The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
        + The site uses SSL and the Strict-Transport-Security HTTP header is not defined.
        + The site uses SSL and Expect-CT header is not present.
        - Sent updated info to cirt.net -- Thank you!


#DVWA Sulutions: (Low Security Level)
  #-Brute Force 
    ->View Source
    ->Find that, only 'Login' will be validated, isset(*) checks whether para* is set and return True/False.
    ->Without any defense of Brute Force, also without any Filter of 'username' and 'password'.
      $Method 1: Usage of Burpsuite for Brute Force 
        Steps:
            1.1 open Burpsuite (Burpsuite is easy to get/download and install from web).
            1.2 click Proxy, set Intercept off firstly, then click 'open browser' to load the webseite, which need to be intercepted.
            1.3 input arbitrary value to login, and Burpsuite will get the info about its.
            1.4 'Ctrl+l' or chose 'Action' as 'Send it to Intruder'.
            1.5 Add password as playload value.
            1.6 Fill the playload set, and start attack.
            1.7 Actually, you will find the response length of the correct password is different as others.
            
       #Medium Level:
        ->Different from Low Level, a new function is added.
            -->> mysql_real_escape_string function, this function will transfer the special symbols, like " ' ", namely, this function can defence Sql Injection Attack
                 thus, the Sql Injection Attack in medium level unfeasible nowly.
            -->> but the Brute Force with Burpsuit is still work in this level.(same as low level, see the steps discription of low level)
            
       #Highl Level:
        ->At high level, a function of generation of CSRF Token is added. 
            -> namele, // Generate Anti-CSRF token  generateSessionToken();
            -> that means, the Request Url will contains a 'user_token' by sending to Server. And the Sever checks the recieved token firstly, if token is valid, then the Server
               operates the Sql queries.
               
      
      $Method 2: Sql Injection
        2.1 -username：  admin ' or ' 1 ' = ' 1
            -password: (none)
            
        2.2 -username: admin ' #   (#:annotation of rest request sentence.)
            -pass: (none)
           
        
        
    
  #-XSS(Reflected)
      ->View Source
      ->Find that, value of param 'name' is directly used, and without any Checking and filtering of Script.
      ->Input: <script>alert('hello world')</script>
