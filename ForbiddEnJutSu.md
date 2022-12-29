# Web - ForbiddEn JutSu

## Challenge description:

![image](https://user-images.githubusercontent.com/70543460/209949450-0119cb60-c0d1-4016-8b2a-42d2b3709560.png)


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

### (2) LFI

After understanding the source code, I tried passing ``/etc/passwd`` to the **karma** parameter, and the content was printed out:

![image](https://user-images.githubusercontent.com/70543460/209953379-8fa8cb4f-854d-488d-bb8e-7c8c61daccbd.png)

And according to the comment in the source code which says:
```php
//Our secret is in the root directory; You can't reveal it without achieving RCE Jutsu ;)
```
I need to achieve RCE because I won't be able to know the name of the secret file which contains the flag which is located in the root ``/`` directory.

----------

### (3) LFI to RCE
