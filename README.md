# Natas

Nata foucus on the server-side web security. I will encounter things like source code inspection, HTTP headers, Cookies, SQL injection, command injection. Each level gives you a URL, username, and you need to find the password. 

# Level 0 

This level gave me the URL `http://natas0.natas.labs.overthewire.org.` So I pulled up my web browser and typed into the bar. When asked I entered in: 
username: `natas0` 
password: `natas0`

<img width="795" height="325" alt="image" src="https://github.com/user-attachments/assets/6c43f4e0-265a-4b58-9a64-f2e21b0a8230" />


<img width="865" height="783" alt="Screenshot 2026-04-07 152815" src="https://github.com/user-attachments/assets/0184669b-d4d3-4f95-b605-35584796deb5" />

Knowing what I already know about source code inpsection I right clicked on the page and selected `View Page Source` 

<img width="907" height="630" alt="image" src="https://github.com/user-attachments/assets/e418cefb-87d8-431e-b7c3-51b9a0428129" />

It showed me the HTML source code and lo and behold the password was hidden in it. 

<img width="857" height="361" alt="Screenshot 2026-04-07 153516" src="https://github.com/user-attachments/assets/33db3368-ac03-4ac4-8f0c-c8359528ca6d" />

## Natas 0 ---> 1 

**Vulnerability:** Information disclosure via HTML source comments

**Difficulty:** Easy

**Category:** Web Security

### What I Did

The page stated that the password was present but not visible and that rightclicking has been blocked. In the last level all I had to do was right click and view page source. 

<img width="860" height="783" alt="image" src="https://github.com/user-attachments/assets/8463ae11-e433-4cbc-abd3-c5cc0ae1e82c" />

Using my keyboard I `Ctrl + U` and it brought up the raw HTML, from there I was able to find the password. 

<img width="872" height="415" alt="Screenshot 2026-04-07 155547" src="https://github.com/user-attachments/assets/ffe08858-a31c-48ce-8ef9-941ff4789a26" />

### Tools Used
- Browser, View source / Dev Tools

###  What I Learned 
- Client-side controls are not security. Anything implemented in JavaScript can be bypassed because the user controls the browswer.
- Never rely on UI restrictions to protect sensitive information. If the data is sent to the client, the user can always access it.
- HTML comments are not private. They are fully visible to anyone who views the source.

### Real-World Impact
- This level mirror real-world mistakes where developers try to "hide" functionality behind disabled buttons, hidden form fields, JavaScript-based restrictions, and CSS visibility tricks.
- Attackers can by pass all of these instantly
- Bug bounty hunters frequently find hidden admin panels, debug endpoints, API keys, hardcoded credentials, feature flags and sensitive comments let by devlopers. 

## Natas 1 ---> 2 

**Vulnerability:** Exposed directory listing revealing sensitive files 

**Difficulty:** Easy

**Category:** Web security/ Misconfiguration

# What I Did

When I first logged in the all the page said was there was nothing on the page 

<img width="958" height="423" alt="image" src="https://github.com/user-attachments/assets/8addeba5-63d8-42aa-8495-623380f99b30" />
















