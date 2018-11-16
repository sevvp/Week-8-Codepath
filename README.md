# Project 8 - Pentesting Live Targets

Time spent: **7** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: Session Hijacking
-
The blue site has a session hijacking vulnerability where you could use the same PHP Session ID to log in as admin on the other color sites as well. What I did was, I logged in as admin on the blue site with the session ID changed to "hacker" using the "public/hacktools/change_session_id.php" tool, then opened the green site, went to the same tool for the green site, changed my session ID to "hacker", and once I clicked login on the green site, I was automatically logged in as the admin, effectively hijacking the session. 

Vulnerability Demonstration: https://youtu.be/vepQhYWRmYU

Vulnerability #2: SQL Injection
-
The blue site has an SQL injection vulnerability where the input is not being sanitized properly compared to the other colored sites. On the red and green sites, if I tried to SQL inject the "id" parameter on the salesperson page I would be redirected to the directory of all the sales people, because my input was being sanitized. However, on the blue site, once I SQL injected the "id" parameter on the salesperson page, I was not redirected, and I could still see my input in the URL. This proves that the input is not being sanitized properly, and opens up the possibility to various SQL injections.

Vulnerability Demonstration: https://youtu.be/P9jFFPcQnaY


## Green

Vulnerability #1: Username Enumeration
-
The green site has a username enumeration vulnerability where an attacker could find out if a certain username exists in the database of the site or not. On the red and blue sites, if you try to login using a username with the wrong password, it will say "Log in was unsuccessful" in unbolded text no matter if the username exists in the database or not. However, on the green site, if you attempt to log in with a username that DOES exist on the database, it will say "log in was unsucessful" in **bold** text. So, this allows an attacker to see if a username exists in the database just by seeing if "log in was unsuccessful" is bold or not in the server response. 

The reason this vulnerability exists is because in the "failure" class of the log in page on the green site, the font-weight is bold. 

Vulnerability Demonstration: https://youtu.be/Jf20r2X3Wdk

Vulnerability #2: Cross-Site Scripting
-
The green site has a cross-site scripting vulnerability where a user can use a script in their feedback message, submit it, and once the admin of the site opens it, the "trap is sprung." This is a stored XSS attack. To demonstrate, this is the script I used in my feedback submission:

```
<script>alert('sev found the XSS!');</script>
```

Once I submitted this, I logged in as the admin, went to the feedback section, and a pop-up appeared with my message. 

Vulnerability Demonstration: https://youtu.be/QvXvKahg9_0


## Red

Vulnerability #1: Insecure Direct Object Reference
-
The red site has a vulnerability where once you are on the salesperson page, you can change the ID # in the URL, and access profiles that you are not supposed to see if you are not the administrator. In this case, there is no sensitive infomration in the hidden profiles, but other sites that have the same vulnerability may have hidden profiles with sensitive information that could be accessed with insecure direct object reference. 

Vulnerability Demonstration: https://youtu.be/kTaKHmrs5Cg

Vulnerability #2: Cross-Site Request Forgery
-
The red site has a cross-site request forgery vulnerability where an unauthenticated person could still change the information on the salesperson page. This exploit takes advantage of a logged in user's access permissions. To take even more advantage of this vulnerability, you could make an HTML form that automatically edits the salesperson page while taking advantage of a logged in user's access permissions (an administrator for example.) 

Vulnerability Demonstration: https://youtu.be/_S8G6Ph8WeM


## Notes

The challenges that I had with this week was mainly with the CSRF vulnerability. It did take me a while to find this vulnerability and how to perform it. 

Which attacks were easiest to execute? Which were the most difficult?
-
The easiest attacks to execute were the insecure direct object reference vulnerability and the cross-site scripting vulnerability. They were straight forward and easy to find the problem. The most difficult attack was the cross-site request forgery, it took me a while to find the vulnerability. 

What is a good rule of thumb which would prevent accidentally username enumeration vulnerabilities like the one created here?
-
The problem was caused by the "font-weight: bold" in the "faliure" classs in the css page of the site. There should not be any visual differences when you specify a correct/incorrect username in a database.

Since you should be somewhat familiar with the CMS and how it was coded, can you think of another resource which could be made vulnerable to an Insecure Direct Object Reference? What code could be removed which would expose it? (Hint: It was also the answer to the first bonus objective to the Weekly Assignment for week 3.)
-
Another resource that could be made vulnerable to an IDOR could be the logged in page. Once you are logged in, you may be able to change the "user" or "id" parameter, and get access to a different user. So, if the admin had a user number of "1", and the users had numbers ranging from 2-100, you could be a user and change your ID to 1, which can log you into the admin user account.

Many SQL Injections use OR as part of the injected code. (For example: ' OR 1=1 --'.) Could AND work just as well in place of OR? (For example: ' AND 1=1 --'.) Why or why not?
-
"AND" may not work in place of "OR" because it could be sanitized so these exploits could not be used. In this case, only "OR" will work for this SQL injections on this site. 

A stored XSS attack requires patience because it could be stored for months before being triggered. Because of this, what important ingredient would an attacker most likely include in a stored XSS attack script?
-
An importnat ingredient an attacker should most likely include would be for the script to automatically send an e-mail once the script is triggered. This way, the attacker could still access the status of his script even if he is kicked off the system. If the attacker inlcludes this in his script, he will be notified once his stored XSS attack is executed.

Imagine that one of your classmates is an authorized admin for the site's CMS and you are not. How would you get them to visit the self-submitting, hidden form page you created in Objective #5 (CSRF)?
-
I could submit a link to the file on the feedback section. Once I submit the feedback saying something like "You need to check this out" or something along those lines, followed by a link to the HTML form, they will click it and it will automatically execute using the administrators permissions which are full access.

Compare session hijacking and session fixation. Which attack do you think is easier for an attacker to execute? Why? One of them is much easier to defend against than the other. Which one and why?
-
For an attacker, it is easier to execute session hijackiing. If the token for a site is poorly encrypted, for example if the CSRF token is simply base-64 encoded, an attacker could get the base-64 encoded string of the word "admin" and hijack the administrator's session. Session hijacking is easier to defend against becuase you could just use hash values for your tokens, which are usually hard to crack and very time consuming. 
