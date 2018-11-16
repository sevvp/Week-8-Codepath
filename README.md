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

The blue site has a session hijacking vulnerability where you could use the same PHP Session ID to log in as admin on the other color sites as well. What I did was, I logged in as admin on the blue site with the session ID changed to "hacker" using the "public/hacktools/change_session_id.php" tool, then opened the green site, went to the same tool for the green site, changed my session ID to "hacker", and once I clicked login on the green site, I was automatically logged in as the admin, effectively hijacking the session. 

Vulnerability Demonstration: https://youtu.be/vepQhYWRmYU

Vulnerability #2: SQL Injection

The blue site has an SQL injection vulnerability where the input is not being sanitized properly compared to the other colored sites. On the red and green sites, if I tried to SQL inject the "id" parameter on the salesperson page I would be redirected to the directory of all the sales people, because my input was being sanitized. However, on the blue site, once I SQL injected the "id" parameter on the salesperson page, I was not redirected, and I could still see my input in the URL. This proves that the input is not being sanitized properly, and opens up the possibility to various SQL injections.

Vulnerability Demonstration: https://youtu.be/P9jFFPcQnaY


## Green

Vulnerability #1: Username Enumeration

The green site has a username enumeration vulnerability where an attacker could find out if a certain username exists in the database of the site or not. On the red and blue sites, if you try to login using a username with the wrong password, it will say "Log in was unsuccessful" in unbolded text no matter if the username exists in the database or not. However, on the green site, if you attempt to log in with a username that DOES exist on the database, it will say "log in was unsucessful" in **bold** text. So, this allows an attacker to see if a username exists in the database just by seeing if "log in was unsuccessful" is bold or not in the server response. 

The reason this vulnerability exists is because in the "failure" class of the log in page on the green site, the font-weight is bold. 

Vulnerability Demonstration: https://youtu.be/Jf20r2X3Wdk

Vulnerability #2: Cross-Site Scripting

The green site has a cross-site scripting vulnerability where a user can use a script in their feedback message, submit it, and once the admin of the site opens it, the "trap is sprung." This is a stored XSS attack. To demonstrate, this is the script I used in my feedback submission:

```
<script>alert('sev found the XSS!');</script>
```

Once I submitted this, I logged in as the admin, went to the feedback section, and a pop-up appeared with my message. 

Vulnerability Demonstration: https://youtu.be/QvXvKahg9_0


## Red

Vulnerability #1: __________________

Vulnerability #2: __________________


## Notes

Describe any challenges encountered while doing the work
