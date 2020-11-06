## Description:
 * `Command Injection Attacks` are possible when an application passes unsafe user supplied data (e.g. form, http headers, etc.) to system shell.
 * This attack differs from `Code Injection`, in that code injection allows the attacker to add their own code that is then executed by the application.
 * In `Command Injection ` , the attacker extends the default functionality of application, which execute system command, without the necessity of injecting code.
 
 
 
# Task-2 of DVMA
## Solutions of Command Injection
### Low Security Level

* At low level, user supplied data (form) will be directly used in php.code, namely be used without any filtering.
* S.t, we just input e.g. '127.0.0.1 && ipconfig ' can solve this task actually.

           * Note: 
             * stristr(string, search, before_search) <---link 'https://www.w3school.com.cn/php/func_string_stristr.asp'
                e.g. result of stristr( 'Hello World','World') is 'World'
                
             * So that, those source codes are used to decide which Operation System used by User ( Windows or Linux )
                  'if( stristr( php_uname( 's' ), 'Windows NT' ) )...else.......'
                
### Medium Security Level

* Differs from Low Level, a blacklist is setted at medium level. But, those codes only filter  ' && ' and ' ; ' characters of user's input.
* That means, we can still use ' & ' to achieve this attack.

        // Set blacklist
            $substitutions = array(
                '&&' => '',
                ';'  => '',
            );
            
* S.t, we can input e.g. '127.0.0.1 & ipconfig ' to solve this task.
* or, we can 'bypass' the filter, e.g '127.0.0.1 &;& ipconfig' to solve this task.
  (';' will be transfered as ' ' according to the blacklist, so that, this input is equivalence to '127.0.0.1 && ipconfig' after filtering )

          * Note:
            Difference between '&&' and '&' is that, for example 'cmd 1 && cmd 2' , cmd 2 will be executed if and only if cmd 1 executed successfuly.But in 'cmd 1 & cmd 2', 
            cmd 2 will be executed even though cmd 1 is failed.
 

### High Security Level

* Differs from Medium Level, much more 'invaild' characters are filterd.

       // Set blacklist
           $substitutions = array(
               '&'  => '',
               ';'  => '',
               '| ' => '',
               '-'  => '',
               '$'  => '',
               '('  => '',
               ')'  => '',
               '`'  => '',
               '||' => '',
           );
  
* Those codes look like that, fast/almost all 'invalid characters are fitered. But actually, the developer made a mistake here '| '.
* So that, we can use '|' to concatenate 'input' and 'cmd' like 'input|cmd'.
* Finally we can input ' 127.0.0.1|ipconfig ' to solve this task, or we can say use this input to ' attack '.
  
  ### Impossible Security Level
  
* Instead of setting blacklist to fiter 'invalid' characters, user's input will be splited into four parts and check each of them, whether those parts are numberic or not.
  
           * Note: 
             $target = stripslashes( $target ); // to remove '/' from string
             $octet = explode( ".", $target );  // Split the IP into 4 octects
