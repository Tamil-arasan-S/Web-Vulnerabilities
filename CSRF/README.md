### Cross Site Request Forgery

Imagine you are currently chatting with your BF/GF using any of the platforms like chatwithchat.com and suddenly you receive a mail from the popular brand Pike saying that you've won a 2000 rupee worth gift card. The mail tricks you to open a link due to FOMO you clicked the link and boom.....

Within few hours without your knowledge the message have been sent to all of your chatting account, requesting money and sending the bank details that is not yours. 

This kind of attack is known as CSRF attack, where the attacker sends a request from your logged in session to the server and the server doesn't have the capablity to verify whether it was the authorised user or not and performs the requested action.

In the DVWA the following example is performed for a password change page, where by performing this attack, the attacker can change your account passoword if you have a logged in session(like cookie).

## Low level security

The source reveal's that there is no proper validation to check whether the requested query is from the authenticated user, so by simple carfting a URL like this and embedding them in the images, mails and other social engineering attack the password can be changed as per the attackers wish.

``http://127.0.0.1:4280/vulnerabilities/csrf/?password_new=[pass]&password_conf=[pass]&Change=Change#``

## Medium level security

In the medium level security the HTTP-Referer headers are used to check whether the request made are from the current domain. But is checks only whether the URL contains the domain(chatwithchat.com). THe attacker can easily manipulate it by crafting a URL like,

``maliciousite.com/chatwithchat.com//vulnerabilities/csrf/?password_new=[pass]&password_conf=[pass]&Change=Change#``

The HTTP-Referer headers will have the address of the previous web page from which the request is coming from.

## High level security

This includes the anti-CSRF method of adding a user_token in every request made so the server validates whether the user_token = session_token. The user_token will unique for every request and the 2nd request made with same user_token will result in CSRF mismatch.

So if the site is vulnerable to XSS(Cross-Site Scripting), where we can inject our codes into the web, then we can grab the user_token from the HTML and use that token to change the password. 

If the Same-Origin Policy is implemented in the site, then even if the site is vulenrable to XSS it can read the user_token as the policy states the other site A can't perform any actions on the site B and vice-versa. But by the XSS, the code runs in the targeted site and thus we can fetch the user_token.






