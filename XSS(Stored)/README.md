# Stored Cross-Site Scripting(XSS)

The injected codes are stored permanently in the server and whenever the end user tries to access the injected code section, the browser autmatically runs the script that was injected by the attackers.

## An example scenario

## Attacker side:
 The attacker finds the endpoint where it is vulnerable to the stored XSS, this endpoint is usually the the input areas where the users can enter the information like forum posts, comment section etc. Once the attacker confirms that the endpoint is vulnerable to the XSS(Stored), the attackers injects the malicious script in the endpoint, the attacker will be able to perform the targeted action.

## User side:
 The user would normally access the section that is malicious script injected, the browser doesn't knows whether the action is malicious or not and without the user's knowledge the script would be executed. This would happen until the injected script is removed from the web server.

## Low severity:
 Here the message section is not sanitised properly, so injecting the basic payload like ``<script>alert(1)</script>`` be used to test whether the site is vulnerablt to XSS. As it is stored XSS, when we try to reload the page, the alert message pops up.

 ## Medium severity:
  The sanitisization is not strict and can be bypassed with the payload like ``<SCRIPT>alert(1)</SCRIPT>``

### High level:
  Here the "script" word is completely blacklisted, but other payloads can be used.

  
  
