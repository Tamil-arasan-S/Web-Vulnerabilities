# DOM(Document Object Model) XSS

Unlike the other XSS attacks, the attack in the DOM XSS is in the client side's javascript code. Whenever the client's side javascripts are not properly sanitised to the inputs that are taken from the URL are the major vulnerability points for the DOM XSS. In that case the attacker can craft a HTML payload that will be performed in the client side and valuable informations like the session and cookies can be stealed. 

**Low level severity:** Here there is no sanitization, so it can be exploited by executing the scripts in the ?default=

**Medium level severity:** The santised in not proper, only the "script" word is blacklisted, we can use other payloads exploit

**High level severity:** It completely blacklisted the script and any payload that can be entered, but by adding the payloads after the # will be bypassed by not executing those payloads in the server.
