## Download, Install and Use of DVMA as Target <br>

  
  ### PhpStudy
	
   >(use Phpstudy for reduce the complexity of setting)<br>
   * lauch Apache, and SQL database
    
  ### DVMA
	
   * Decompression of DVMA_Master.zip (which downloaded from ‘http://www.dvwa.co.uk/’) under the folder 'phpstudy_pro/wwww/.'
   * Modify suffix of document 'config.inc.php.dist' ->> 'config.inc.php'.
   * Modify the content of 'config.inc.php' --->>  modify values of 'db_username' and 'db_password' as 'root'.
      >namely, 'db_username'='root' and  'db_password' = 'root'.
   * Open the dvma webseit 'http://127.0.0.1/setup.php'
   * Login with default credential
       >namely, 'username' = 'admin', 'password' = 'password'
    
    
 

### DVWA Sulutions of Brute Force: 
  #### Brute Force 
  ##### *(Low Security Level):
    *   View Source
    *   Find that, only 'Login' will be validated, isset(*) checks whether para* is set and return True/False.
    *   Without any defense of Brute Force, also without any Filter of 'username' and 'password'.
  ##### Method 1: Usage of Burpsuite for Brute Force 
  * Steps:
            * 1.1 open Burpsuite (Burpsuite is easy to get/download and install from web).
            * 1.2 click Proxy, set Intercept off firstly, then click 'open browser' to load the webseite, which need to be intercepted.
            * 1.3 input arbitrary value to login, and Burpsuite will get the info about its.
            * 1.4 'Ctrl+l' or chose 'Action' as 'Send it to Intruder'.
            * 1.5 Add password as playload value.
            * 1.6 Fill the playload set, and start attack.
            * 1.7 Actually, you will find the response length of the correct password is different as others.
   ##### Method 2: Sql Injection
  * 2.1 
	-username：  admin ' or ' 1 ' = ' 1
        -password: (none)
  * 2.2 
	-username: admin ' #   (#:annotation of rest request sentence.)
        -pass: (none)
            
   ##### *(Medium Security Level):
        ->Different from Low Level, a new function is added.
            -->> mysql_real_escape_string function, this function will transfer the special symbols, like " ' ", namely, this function can defence Sql Injection Attack
                 thus, the Sql Injection Attack in medium level unfeasible nowly.
            -->> but the Brute Force with Burpsuit is still work in this level.(same as low level, see the steps discription of low level)
            
       #Highl Level:
        ->At high level, a function of generation of CSRF Token is added. 
            -> namele, // Generate Anti-CSRF token  generateSessionToken();
            -> that means, the Request Url will contains a 'user_token' by sending to Server. And the Sever checks the recieved token firstly, if token is valid, then the Server
               operates the Sql queries.
               
      
      
           
        
        
    
  #-XSS(Reflected)
      ->View Source
      ->Find that, value of param 'name' is directly used, and without any Checking and filtering of Script.
      ->Input: <script>alert('hello world')</script>
