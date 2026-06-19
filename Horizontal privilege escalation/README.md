# Access control vulnerablity

  Imagine a situation where you logged into an web-app as a user and able to view all other users personal info, even modify/delete their account in some extends. SO that is Horizonatal privilege escalation. 

  ## User role can be modified in user profile from the protswigger platform:

<img width="1077" height="161" alt="image" src="https://github.com/user-attachments/assets/ff6fcae8-5429-4a67-860d-414222d9321b" />

So we have to access the `/admin` endpoint as a normal user. So first we can login usin the given creds.

<img width="1078" height="513" alt="image" src="https://github.com/user-attachments/assets/ec6fb107-b491-400c-984b-b49c3d571ade" />

At first i got confused where find the `roleid` parameter, but then found the parameter by updating the mail. Usually i thought only the request matters and it holds all the info's but this lab proved me that I was wrong. 

Got frustrated to see that i can't find the `roleid` parameter and even tried to add the own values like `admin:true` in the cookies header, but nothing worked. Even changed the `id=` to `roleid=` to check if i can see any changes but nope.

<img width="1598" height="758" alt="image" src="https://github.com/user-attachments/assets/7d007049-35b2-43b0-959d-01b3062a78a1" />

Then with the solution, realised that the even the response is immportant during the pentesting. Analyzing what gets stored in the server also matters. So added the `roleid=2` in the request header which was in the JSON format and even now i got the `500 Internal Error`. Then by properly analyzing found that the JSON format was missing the ',' and then by adding the `roleid=2`, i can change the `roleid`, and i was able to access the admin panel delete the Carlos account.

<img width="1667" height="647" alt="image" src="https://github.com/user-attachments/assets/a76aa1f7-d932-4701-bd68-3d4d32ace53d" />

Knowledge Learnt:
 > Give equal importance to the response section of the burp. What's getting stored in the server is also important.
 > Even a ',' plays a major role
 > Have the habit of exploring each and every element and features in the Web-App
