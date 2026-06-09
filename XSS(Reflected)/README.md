# XSS - Reflected

The vulnerability where the input fields of the site allows the users to enter the scripts and this can be violated by the attackers to execute malicious scripts in the site that can make all the request that the authenticated user can make

### Low Severity
This can be easily bypassed by the payload:
``<script>alert(1)</script>``

### Medium Severity
The server only restricts the "script" word, and this can bypassed by the captial "SCRIPT"
``<SCRIPT>alert(1)</SCRIPT>``

### High Severity
This time the server blocks the exection of the word script completely, so the other alternative payloads in the HTML tags can be used
``<img src=x onerror=alert(1)>``
