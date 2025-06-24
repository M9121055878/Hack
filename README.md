# overthewire.org

I'm trying to solve the challenges of overthewire.org.

<!-- Note, Tip, Important, Warning, Caution -->

> [!Important] 
> The description of each step is essentially derived from the previous step. For example, the solution to Natas1 is something that must be found in Natas0.

- All user is name of levels

        http://natas`X`.natas.labs.overthewire.org/

        user:natas`X`
        pass:*******

## natas0 http://natas0.natas.labs.overthewire.org/

- He himself gave us the username and password in overthewire.org for start HACKGAME.

        user:natas0
        pass:natas0

## natas1 http://natas1.natas.labs.overthewire.org/

1. Get the page's source.
2. Look the HTML comment.

        <!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->

## natas2 http://natas2.natas.labs.overthewire.org/

- You cannot right-click.
1. Get the page's source.
2. Look the HTML comment.

        <!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->

## natas3 http://natas3.natas.labs.overthewire.org/

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

## natas4 http://natas4.natas.labs.overthewire.org/

- he have comment in page's source

        <!-- Not even Google will find it this time... -->

1. Sites often use a ‍‍‍‍‍‍‍‍`robots.txt` file in root of site to tell search engines what files and paths they should not see.

        http://natas3.natas.labs.overthewire.org/robots.txt

    in this file you can find a path : `/s3cr3t/`

2. go to : `http://natas3.natas.labs.overthewire.org/s3cr3t/`

    in this path you can find : `users.txt`

        natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

## natas5 http://natas5.natas.labs.overthewire.org/

- he has a text message :

        You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"

1. When you click `Reload Page`, the message changes to this :

        Access disallowed. You are visiting from "http://natas4.natas.labs.overthewire.org/index.php" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"

    you need change `Referer` HTTP header

2. for change referer http header, i use `curl`

    i run ubuntu in windows with `wsl`

        ~$ curl -u natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ --referer http://natas5.natas.labs.overthewire.org/ http://natas4.natas.labs.overthewire.org

    tada :

        Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK

## natas6 http://natas6.natas.labs.overthewire.org/

- he has a text message :

        Access disallowed. You are not logged in

1. Okay. Matt's login information must be stored somewhere.

    So first I went to check the `storage`, I'm using `Chrome`.

2. Pressing the `F12` button in Google Chrome opens the developer tools, in the `Application` tab, I look for something in storage and found it in the `Cookies` section.
   
        loggedin : 0

3. cheange loggedin to `1` and refresh.

        Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
