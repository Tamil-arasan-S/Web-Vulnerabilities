## The low level security
<img width="721" height="644" alt="image" src="https://github.com/user-attachments/assets/c3b1dc8b-699c-4bac-a222-c85694d28635" />

So in the low level security bruteforce, while analysing the source code the inputs that are received by the users are not properly sanitized so it can lead to SQLi vulnerablity in the first place. Also there is no proper proper validation to prevent the bruteforce attacks so the bruteforce attacks can be perfomed using the command line tools like `HYDRA` or with `BURP SUITE`.

```
<?php

if( isset( $_GET[ 'Login' ] ) ) {
    // Get username
    $user = $_GET[ 'username' ];

    // Get password
    $pass = $_GET[ 'password' ];
    $pass = md5( $pass );

    // Check the database
    $query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    if( $result && mysqli_num_rows( $result ) == 1 ) {
        // Get users details
        $row    = mysqli_fetch_assoc( $result );
        $avatar = $row["avatar"];

        // Login successful
        echo "<p>Welcome to the password protected area {$user}</p>";
        echo "<img src=\"{$avatar}\" />";
    }
    else {
        // Login failed
        echo "<pre><br />Username and/or password incorrect.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?> 
```

We'll be using the burpsuite,

We can send the captured request to the Intruder to perform the sniper attack,
<img width="1337" height="605" alt="image" src="https://github.com/user-attachments/assets/3ab19032-966a-4fed-b314-38241fc53c46" />

We need to find the admin's password so `username=admin` and the add the payload in the password parameter,
<img width="1332" height="736" alt="image" src="https://github.com/user-attachments/assets/54373950-70c5-4825-b37f-14c4ac9f9e02" />

Select the `runtime file` as the payload type and select the `rockyou.txt ` file,
<img width="462" height="177" alt="image" src="https://github.com/user-attachments/assets/07d73b5c-71f6-4e57-a9b2-aec8a4dc0458" />
<img width="799" height="595" alt="image" src="https://github.com/user-attachments/assets/e69ed14c-e31f-4f1a-9a59-4a3048f1b554" />

After performing the attck by sorting out the length of the response we can find the password that is `password`
<img width="1554" height="865" alt="image" src="https://github.com/user-attachments/assets/19041fd3-f6ac-4a90-a2fb-6d3aa0fa1544" />

<img width="542" height="402" alt="image" src="https://github.com/user-attachments/assets/da0e1c83-8d06-46f8-9980-9918576841f0" />

## Medium level security
As the medium level security add's the extra layer of security to `sleep(5)`, that makes the bruteforce a time-taking one but still it works.
Also the inputs from the users are sanitised over here, so the SQLi is mitigated.

```
 <?php

if( isset( $_GET[ 'Login' ] ) ) {
    // Sanitise username input
    $user = $_GET[ 'username' ];
    $user = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $user ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

    // Sanitise password input
    $pass = $_GET[ 'password' ];
    $pass = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
    $pass = md5( $pass );

    // Check the database
    $query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    if( $result && mysqli_num_rows( $result ) == 1 ) {
        // Get users details
        $row    = mysqli_fetch_assoc( $result );
        $avatar = $row["avatar"];

        // Login successful
        echo "<p>Welcome to the password protected area {$user}</p>";
        echo "<img src=\"{$avatar}\" />";
    }
    else {
        // Login failed
        sleep( 2 );
        echo "<pre><br />Username and/or password incorrect.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
```

But still the brute force works, same like the method used for low-lewel security

## High level security

So the high level security makes the brute-force a much harder one but still it can be performed if the concept of CSRF token is understood properly. To perform the bruteforce over here first we have to understand what actually happens when we try to send the request to the server,

<img width="1340" height="569" alt="image" src="https://github.com/user-attachments/assets/ccb775cd-b1bb-432a-9bb0-3ee472706373" />


As you can see a new parameter `user_token` is included to enhance the security. So now let's understand what happens in the every request that is sent to the server,

1) Every time the credentials are entered and the request is sent to the server, a unique `user token` is also generated everytime. Whenever the user logs in and enters the credentials and send the request to the server(Whether the credentials are correct or wrong), there would be unique `user_token` generated for each and every log in attempts.
2) This token plays a crucial role. During the traditional brute-force it tries all the possiblities with the wordlists provided to find the password. Every time the request in the server are validated and the response is received accordingly, but as the new parmeter of unique `user_token` is sent along with the credentials, it checks the `user_token's` also.
3) The `user_token` is stored in the server everytime for the new request of credentials and reloads(called as the `session_token`). So every time when the attacker tires to enter the credentials, he should also use the unique `user_token` that is generated.

So when the `user_token=session_token` the log in succeeds.

For the first request it shows incorrect password, and with the second attempt it shows CSRF token mismatch
<img width="1341" height="742" alt="image" src="https://github.com/user-attachments/assets/8510e391-5d9c-4847-bf96-446d645bb31f" />
<img width="1340" height="737" alt="image" src="https://github.com/user-attachments/assets/ceccde1f-f8bf-4db7-a93d-6d0c796580a2" />

So when we try to send the request with the same `user_token` for multiple times it shows CSRF mismatch. 

**Attack Methodology**

Need to retreive the new `user_token` that is generated for every brute-force rquests that is made.

To perform that we'll be greping out the `user_token` from the HTML that is hidden.
```
[ Start with Request 1 ] Includes Token AAA (Manually stolen from your browser)
       │
       ▼
 Server checks Request 1 ──> Denies login, but generates NEW Token BBB in the HTML response.
       │
       ▼  (Burp "Grep-Extracts" Token BBB from this response)
       │
[ Form Request 2 ] ───────> Injects Next Password + Token BBB
       │
       ▼
 Server checks Request 2 ──> Denies login, but generates NEW Token CCC in the HTML response.
       │
       ▼  (Burp "Grep-Extracts" Token CCC from this response)
       │
[ Form Request 3 ] ───────> Injects Next Password + Token CCC
```

Send the captured request to the Intruder and set the Pitchfork attack to perform the simulataneos attack at different payloads

<img width="1335" height="737" alt="image" src="https://github.com/user-attachments/assets/9d37a309-e5c9-471e-9a36-830374158bff" />

Rockyou.txt file for the password payload

<img width="1334" height="742" alt="image" src="https://github.com/user-attachments/assets/1b8d0215-c7b3-4ec7-b432-6c2be6edaea0" />

Now for the `user_token`, we have to grep the unique tokens that is generated everytime and and add them as the payload. We can perform that by the grep-extract option from the settings tab of the intruder.
From the grep-extract tab, add the grepped-out token by selecting the `refetch response` option and grepping out the `user_token`.

<img width="1339" height="743" alt="image" src="https://github.com/user-attachments/assets/5424839b-ea1e-4d6f-9f90-56b93ddf0f95" />

After selecting the token, click ok and it will automatically added as the regex in the grep-extract

<img width="1339" height="729" alt="image" src="https://github.com/user-attachments/assets/497b1ca5-851d-4f6a-ae6d-f54583d7f11d" />

Also select to always redirect, to extract the token everytime

<img width="486" height="740" alt="image" src="https://github.com/user-attachments/assets/72d53d24-91ac-4574-8c31-abb570264745" />

Select the payload as the recursive grep and the resource pool's maximum concurrent request to 1,

<img width="552" height="787" alt="image" src="https://github.com/user-attachments/assets/9e9f0665-cc0b-44fc-a131-139b96bbd525" />

<img width="505" height="703" alt="image" src="https://github.com/user-attachments/assets/0c51e229-2b4e-41fc-9e91-58b351708f20" />

Now perform the attack and verify with the response tab,

<img width="1498" height="846" alt="image" src="https://github.com/user-attachments/assets/3c404596-ea68-4b10-9f18-581c99c83274" />
