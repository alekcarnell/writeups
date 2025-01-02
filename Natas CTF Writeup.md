Level 0: 
1. Logged in to the website with the given credentials and started poking around.
2. First thing I did was check the page source and there it was as a comment in the code: `<!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->`

Level 1:
1. In this room it says right clicking is disabled. However, it might be because I was in a virtual machine, but right-clicking worked just fine. But I could have also used F12 on the keyboard to get around this. 
2. The page source had the next password in the code: `<!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->`
   
Level 2:
1. I took a look at the page source again on this level. I saw that there was now an image being referenced in the source code for a small, pixel-sized, png. It was stored in /files/ in their server. 
2. So I added /files to the end of the URL to see if it would take me to the directory. it did! I was able to click on a "users.txt" file which contained a list of usernames and their passwords and found the next level: `natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`
   
Level 3:
1. Checked the page source here and in a comment it says, "no more hints". So I tried going back to the /files directory and it said "Not Found" BUT it also said `Apache/2.4.58 (Ubuntu) Server at natas3.natas.labs.overthewire.org Port 80`. Might be good to know that later. 
2. I tried seeing if Google could find or enumerate the pages for this website. I found some random website "Seoget" to crawl it and it gave me the error: `Error: Could not fetch robots.txt due to error 401 (Unauthorized)`. I didn't know about this robot.txt before so I will try to access it. 
3. Nice, we got something returned: `User-agent: *Disallow: /s3cr3t/`. Looks like another directory that I can try. 
4. Bingo, I can access the directory no problem and there is another "users.txt" file that contains: `natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ`

Level 4:
1. This one was a bit more challenging. I did have a correct intuition that we needed to mess with some of the requests. So I started looking at the requests under the Networking tab in the Developer View and messed with what I knew (no much). After spending a good amount of time and revisiting this the next day, I decided I needed some help and started googling.
2. What I needed to do was perform a "referrer spoof" or "hijack". This was something I had not done before so I read up on it. It also appeared that I needed some kind of tool in order to edit this part of the Header so I downloaded "Tamper Data" for firefox. 
3. After installing the plugin, I was able to intercept the requests and spoof all of the header information. In this case, I just needed to spoof the referrer to tell the browser that I was coming from "http://natas5.natas.labs.overthewire.org" instead of "http://natas4.natas.labs.overthewire.org". Once I made that change, I sent the request on its way and got the password! `0n35PkggAPm2zbEpOU802c0x0Msn1ToK`
   
Level 5:
1. The next level was a little easier. The page says "you are not logged in". I went into dev tools and poked around and found a cookie that managed the loggins. The value was set to '0', so I just set it to '1' and refreshed the page and got the next password: `0RoJwHdSKWFTYR5WuiAewauSuNaBXned`

Level 6:
1. This next level now has a form! Immediately my mind thinks about all of the many possibilities. But I figure lets start small and simple. 
2. I submit some gibberish into the form to see what kind of response it gives. It says, "wrong secret". 
3. So it is either looking for a particular input from me, or I need to find the password by circumventing the check for the secret. I start with the first assumption that I need to figure out an actual secret passcode to provide this form in order to move on. 
4. I start checking the source code of the page and the form. I was able to view the Form function at `http://natas6.natas.labs.overthewire.org/index-source.html`
   
```
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
```

5. Excellent, so we need to check what the Form wants to see inside of the secret.inc file on the webserver. I will try to navigate to it directly. 
6. I navigated to `http://natas6.natas.labs.overthewire.org/includes/secret.inc` which showed a blank page but viewing the page source reveals the secret! `FOEIUWGHFEEUHOFUOIU`
7. I entered the secret into the form and got the password: `bmg8SvU1LizuWjx3y7xkNERkHxGre0GS`

Level 7:
1. Greeted with the next page, I inspected the page source and saw a clue. "<!-- hint: password for webuser natas8 is in /etc/natas_webpass/natas8 -->"
2. In the last level, I was able to navigate the server directory to find the password. But this would require being able to navigate the file system of the server. There isn't a form to abuse on this level though. So my first thought was to actually go back to the previous level where the form was and try a XSS attack. 
3. I tried a few things, but couldn't get any of them to work. It looks like the input was being sanitized. 
4. Turns out, as is often the case, the simplest solution is the right one. I had the page just point to the directory in the server by modifying the URL and it spat out the password: `xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q`

Level 8:
1. Upon landing, we are presented with another Form and an additional link that says, "view sourcecode". So I clicked it. 
2. I won't copy all of the code, but the part that stuck out was:
```   
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
```

3. This gives us the secret and tells me that it was encoded using a nesting of methods like reverse string and base64. I should be able to crack it with CyberChef.
4. After messing quite a bit with the recipes and order, I got pretty close, but found a MUCH easier way to do it. 
5. Just start an interactive PHP shell in the terminal by using `PHP -a` and then copy and paste the same code from the source code to the terminal, with the encoded secret in place oft he variable, and it gives you the secret. `oubWYf2kBq`
6. Putting the secret into the form gives you the next password! `ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t`

Level 9:
1. After logging in to the next level, we are again presented with a form and some source code. 
2. The source code this time is:
```
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
```

3. It appears that this defines an empty variable, which takes the users input, and then searches a dictionary.txt file for all words that contain the string that the user provided. 
4. I attempt to do a command injection once more and this time it goes through! `;ls` returns a list of files in the directory, namely the dictionary.txt file. 
5. I try something a bit more complex: `;ls -l /etc/natas_webpass/natas10`
6. Success! `t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu`
7. I did attempt to grab the others, not thinking they would let me but wanting to try anyway. Nothing came back

Level 10:
1. upon logging in, I am presented with a very similar level as the last, except there is an added note at the top: "For security reasons, we now filter on certain characters". Well that is good!
2. Upon looking at the source code, we can see what is being filtered:
``` 
if(preg_match('/[;|&]/',$key)) {
```

3. This allows us to use the rest though like "#<>!@" and so on. 
4. After doing some research on other commands that we could use that didnt have illegal symbols, I hit a wall. But of course, once again, simplifying things made it clear. 
5. I tried inputting just a `a`. I got a list of words. As long as our payload gets through, we should be able to see what is going on. The `;` at the beggining of the command in the previous level was our delivery method. The `a` is now our new delivery method....
6. So I tried, `a ls -la`. BINGO! It executed and returned everything after the `a`
7. Naturally, my next payload was `a cat /etc/natas_webpass/natas11` which revealed the password! `UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk`

Level 11:
1. 