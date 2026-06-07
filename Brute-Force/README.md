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





