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
4. After doing some research on other commands that we could use that didn't have illegal symbols, I hit a wall. But of course, once again, simplifying things made it clear. 
5. I tried inputting just a `a`. I got a list of words. As long as our payload gets through, we should be able to see what is going on. The `;` at the beginning of the command in the previous level was our delivery method. The `a` is now our new delivery method....
6. So I tried, `a ls -la`. BINGO! It executed and returned everything after the `a`
7. Naturally, my next payload was `a cat /etc/natas_webpass/natas11` which revealed the password! `UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk`

Level 11:
1. Upon logging in, I see page that allows me to provide an input, specifically a hexadecimal color value, and then the websites background color will change to what I provide. 
2. There is also a note at the top that reads: "Cookies are protected with XOR encryption"
3. There is another "View Sourcecode" button that I will go ahead and inspect as well. 
4. This code is much more sophisticated than what we have seen in previous levels. 
```
 <?

$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);

?>
```

4. I am just going to break down what I think each function does to help myself get through this.
   
   function xor_encrypt{$in}
	- takes $in as an argument and feeds it into $text
	- defines the $key variable, but censors it so we can't see it
	- A loop iterates through each digit in $text and performs a bitwise XOR operation against the $text and the $key. To ensure the entire thing gets operated on, the modulo (%) operation is used to when comparing the $text and the %key which effectively loops back around to the beginning of the key if the text is too long or short. 
	- The result of the operation is appended to the $outText variable and returned by the function after it has completed. 
	- I had ChatGPT summarize the code as well and that seems to be more or less what is going on: *"The code iterates over each character in `$text`, XORs it with a corresponding character from `$key` (repeating the key if necessary), and appends the result to `$outText`. This could be used for a simple encryption or encoding algorithm."*

	function loadData{$def}
	-  This one was a little above my programming knowledge so I had ChatGPT break it down for me: 
	  
	- **Input**: The function accepts an argument `$def`, which is an array.
	- **Process**: It checks if a cookie named `"data"` exists. If the cookie exists:
	    - It base64-decodes and XOR decrypts the cookie's value.
    - It attempts to decode the decrypted value as JSON.
    - It checks if the decoded data contains specific keys (`showpassword`, `bgcolor`) and if the `bgcolor` is a valid hex color code.
	- **Output**: The function returns the modified or unmodified `$mydata` array. If the cookie data is valid, it updates `$mydata` with values from the cookie; otherwise, it returns the original `$def` array.

5. It seems to be that the entire script takes the users input, sanitizes it, encodes it, and stores it in a cookie. Then the cookie is read and the background color of the website it set when the cookie is decoded. 
6. So... it seems like we need to bypass encryption, perhaps by encrypting our own payload and submitting it?
7. I opened up the console and inspected the cookie that was made by my previous submission. Inside the cookie was the data in, what appears to be, its encrypted form: "HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1Gd2MJAyU2TRg%3D"
8. We have to go through the motions of working backwards from this....
9. The encryption was handled using base64_encode(XOR_encode(Json_encode(plain_text)))
10. We are trying to get the plaintext first so that we can solve for XOR. So I throw up an interactive PHP shell to do a JSON_encode of the plaintext that was given to us in the source code "showpassword"=>"no", "bgcolor"=>"#ffffff"
11. `php > echo json_encode(array("showpassword"=>"no", "bgcolor"=>"#ffffff"));` gives us `{"showpassword":"no","bgcolor":"#ffffff"}`
12. We then put that as our key for the XOR operation and feed the string that we get from decoding the cookie data in Cyberchef. To break that down:
	1. We first do a base64 decode of the string `HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1Gd2MJAyU2TRg=`
	2. Then we take that output, which looks like a mess, and set that as our new input for our next operation `f$ 3'7   uUG*8MIf5+; fmMF"1	"1M`
	3. The next operation is to use XOR, and use our "`{"showpassword":"no","bgcolor":"#ffffff"}`" as the key. The new output is: `eDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoeDWoe`
	4. This is actually correct, because if we recall in the function, the text was shorter than the key, so it loops around. Which means we know the text is `eDWo`
13. Now that we have our key, we need to work our way back up and try to craft a cookie that tricks the browser into sending "{"showpassword":"yes","bgcolor":"#ffffff"}"  (note the "yes" instead of the "no" this time)
14. In Cyber chef, we set our input to that JSON string with the Yes, and then we do XOR and Base64 in that order. We set the XOR key to `eDWo` and get: `HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5`
15. We go back into our browser and plug replace the old cookie data with our new cookie and refresh the page and BOOM!
16. Password is: `yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB`

