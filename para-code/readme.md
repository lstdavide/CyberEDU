# para-code

###### Category: Web
###### Difficulty: Easy
###### Points: 50
###### Description: I do not think that this API needs any sort of security testing as it only executes and retrieves the output of ID and PS commands. Flag format: CTF{sha256}
---
##### Walktrough: 

This challenge provides an instance on port 1234 and an IP on 34.185.192.227. The IP provided opens the following PHP code:

~~~ php
<?php  
require __DIR__ . '/flag.php';  
if (!isset($_GET['start'])){    show_source(__FILE__);  
    exit;  
} $blackList = array(  'ss','sc','aa','od','pr','pw','pf','ps','pa','pd','pp','po','pc','pz','pq','pt','pu','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','ls','dd','nl','nk','df','wc', 'du'  
);  
  
$valid = true;  
foreach($blackList as $blackItem)  
{  
    if(strpos($_GET['start'], $blackItem) !== false)  
    {         $valid = false;  
         break;  
    }  
}  
  
if(!$valid)  
{  show_source(__FILE__);  
  exit;  
}  
  
// This will return output only for id and ps. 
if (strlen($_GET['start']) < 5){  
  echo shell_exec($_GET['start']);  
} else {  
  echo "Please enter a valid command";  
}  
  
if (False) {  
  echo $flag;  
}  
  
?>
~~~

---
~~~ php
require __DIR__ . '/flag.php';  
if (!isset($_GET['start']))
{    
	show_source(__FILE__);  
	    exit; 
~~~

We see another file 'flag.php' being loaded where we expect the flag to be stored. The application requires a GET parameter 'start', which is required in order for us to exploit this PHP vulnerability. Without the 'start' parameter the application will only show the source code and exits.

~~~ php
$blackList = array(  'ss','sc','aa','od','pr','pw','pf','ps','pa','pd','pp','po','pc','pz','pq','pt','pu','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','pf','pz','pv','pw','px','py','pq','pk','pj','pl','pm','pn','pq','ls','dd','nl','nk','df','wc', 'du'  
); 
  
$valid = true;  
foreach($blackList as $blackItem)  
{  
    if(strpos($_GET['start'], $blackItem) !== false)  
    {         $valid = false;  
         break;  
    }  
}
~~~

This 'blackList' contains two-character strings that are blacklisted, then checks if the input contains any of the strings within the list. 'strpos()' checks for blacklisted values as substrings.

~~~ php
// This will return output only for id and ps. 
if (strlen($_GET['start']) < 5){  
  echo shell_exec($_GET['start']);  
} else {  
  echo "Please enter a valid command";  
}
~~~

We have another restriction refering to the length which has to be smaller than 5, so maximum input length is 4 characters. the start parameter is passed to 'shell_exec()' without being escaped, which is exactly the vulenrability we want to exploit in this challenge, which is command injection.  We also take note of the attached comment which states that output will be returned for id and ps only which suggests that id and ps are relevant to the solution. However, ps is included in the blacklist so '?start=ps' can never reach 'shell_exec()' setting '$valid' to false and returns the source code.

---


The PHP code uses the 'start' GET parameter so i add '/?start=id' to the URL, and it outputs 'uid=1000(www) gid=3000(www) groups=3000(www),2000' meaning 'id' passes the blacklist therefore we have command execution. 

<img width="826" height="492" alt="Screenshot from 2026-09-16 08-21-19" src="https://github.com/user-attachments/assets/07a67f44-aab0-4383-9b56-5385ed812a6c" />

ps does not return any value as it is blacklisted. 'dir' returns the file names. 

<img width="826" height="492" alt="Screenshot from 2026-09-16 09-08-29" src="https://github.com/user-attachments/assets/21c2bd67-0503-4186-a5a5-1e2e9e4ca10f" />

As WSTG-INPV-11 (Testing for Code Injection) states, we need to pass %20 and a PHP wildcard, in this case * to test for PHP Injection. To find what command can pass in order for us to get the flag, we need to see what AIX commands we can input. For this we check the IBM AIX documentation and look at the commands section. 'm4' passes and gives us the flag. 'm4', as the documentation defines it is a macro processor used as a preprocessor for C and other languages. You can use it to process built-in macros or user-defined macros.

<img width="1515" height="729" alt="Screenshot from 2026-09-16 09-22-46" src="https://github.com/user-attachments/assets/bab78b7d-0882-4603-8054-f2923b7ad152" />

---

###### References:
https://www.opencre.org/cre/547-283
https://cwe.mitre.org/data/definitions/676.html
https://cwe.mitre.org/data/definitions/78.html
https://www.ibm.com/docs/en/aix/7.2.0?topic=m-m4-command
