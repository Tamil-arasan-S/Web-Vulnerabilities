# URL-based access control can be circumvented

This lab is in the portswigger, where we'll be modifying the request header's method URL that is sent to the server. Every time we send the request to server, it will be hitting many middleman's namely reverse proxies. These reverese proxies will send the header named X-Original-URL to the server, the X-Original-URL states the server the orginal source of the request. What if we can modify this header...

This is exactly what we perform in this lab, we will be adding the X-Original-URL value to the unauthorised endpoint, by which we can access those endpoints. In this example we'll be accessing the admin panel as a normal user and delete a user.

<img width="1012" height="371" alt="image" src="https://github.com/user-attachments/assets/11da74e4-1bcf-45dd-87ab-7d36a514126d" />

When we try the access the `/admin` panel, it says `Access denied`, which is a front-end block. We can bypass this by adding only `/` in the request method and a non-standard HTTP header named `X-Original-URL: /admin`. By this we can acess the admin panel and perform the targeted action.

![Uploading image.png…]()
