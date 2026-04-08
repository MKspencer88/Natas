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

When I first logged in the all the page said was there was nothing on the page and I knew that was just an outright lie. 

<img width="958" height="423" alt="image" src="https://github.com/user-attachments/assets/8addeba5-63d8-42aa-8495-623380f99b30" />

Using what I learned from the last two levels I pressed `Ctrl + U` and I noticed that on line 15 of the HTML source code `src="files/pixel.png` 

<img width="942" height="359" alt="Screenshot 2026-04-07 163119" src="https://github.com/user-attachments/assets/87ccdcb1-3af5-426d-a712-5d055f4bb635" />

I clicked on the image and it was just a blank screen, with a dot in the middle. 

<img width="906" height="650" alt="image" src="https://github.com/user-attachments/assets/f411fd42-ac1d-41d7-ae2b-75fa233f7a93" />

I wondered what would happen if I took off the `pixel.png`. As I am looking at this new HTML code I see a possible file called `users.txt`.

<img width="948" height="366" alt="image" src="https://github.com/user-attachments/assets/c8be7726-761d-41d6-b5ee-634bc0705878" />

In the URL after ***/files*** I typed in `users.txt` and the page it pulled up was unreal! I had found all the users and their passwords. Including the password to the next level. 

<img width="792" height="293" alt="image" src="https://github.com/user-attachments/assets/34b5fcdb-e572-4cbd-80a4-8f0df19ede07" />

### What I Learned

Web servers like Apache serve directory listings by default unless explicitly disabled. Sensitive files left in web-accessible directories becomes trivially exposed. Never store credentials in public paths. 

### Real-World Impact

A common misconfiguration in bug bounty programs, rated medium to high severity. Exposed files often include `.env`, `config.php`, database dumps, or backup archives with credentials. 

# Natas 2 ---> 3 

**Vulnerability:** robots.txt disclosure + sensitive file exposure

**Difficulty:** Easy

**Category:** Web Security / Information Disclosure / OSINT

### What I Did

When I first logged on to `https://natas3.natas.labs.overthewire.org` the main page said there was nothing on the page. 

<img width="948" height="336" alt="image" src="https://github.com/user-attachments/assets/51f7d8eb-05cd-4d78-99b4-f0db6130edad" />

As we all know that is just a lie, I pressed `CTRL + U` and it pulled up the HTML source. 

<img width="929" height="389" alt="Screenshot 2026-04-08 150000" src="https://github.com/user-attachments/assets/212a524b-6512-4b8c-976a-286bb69ad510" />

Line 15 of the code said this `<!-- No more information leaks!! Not even Google will find it this time... -->`. This was my hint reffering to a speical file called `robots.txt`. It tells a website to use search engine crawlwers whcih pages they're allowed or ***not*** allowed to index. It is always the same with every webisite. 

So I entered that into the URL. 

<img width="825" height="165" alt="image" src="https://github.com/user-attachments/assets/a5d0d012-e19e-46f8-b10c-797bc13fe021" />

- `user-agent: *` : Meaning this applies to all crawlers.
- `Disallow: /s3cr3t/` : Telling Google not to crawl this directory.

In the URL I replaced `robots.txt` with `/s3cr3t/` and it pulled up another HTML source code page. Like the last level I saw that it had a files names `users.txt` 

<img width="958" height="371" alt="image" src="https://github.com/user-attachments/assets/c741132a-4a71-4717-844a-69bfee41a8c4" />

When I clicked on it...

<img width="771" height="108" alt="image" src="https://github.com/user-attachments/assets/bb3cf199-cf44-409a-866d-5131628bf3cf" />

### What I Learned
- `robots.txt` is a public file listing a path under `Disallow` advertises it to anyone who checks. Security through obscurity is *not* access control. True protection requires authentication, not hidden URLS.
- I also learned about crawlers, it can also be called a spider or a bot. It is an automated program that browses the web. It visits a webpage, reads it content, then follows every link on that page. Search engines like Google user crawlers to discover and index web pages so they show up in search results.

### Real-World Impact

Checking `robots.txt` is a standard first step in web recon and bug bounty hunting. Disallowed path freqeuntly expose admin panels, staging envrionments, backup folders, or internal tools. You can go on anyweb page an type in at the end of the URL `/robots.txt` and you can view the file. 

# Natas 3 ---> 4 

**Vunlnerability:** Broken access control via spoofed Referer header

**Difficulty:** Easy

**Category:** Wed Security / HTTP Header Manipulation / Broken Access Control

### What I Did

When I first when to the URL `http://natas4.nata.labs.overthewire.org` 

<img width="949" height="403" alt="image" src="https://github.com/user-attachments/assets/d6e9a336-0859-42af-b5ae-6cffbde7e4b3" />

This indicated to me that there was something wrong with the Referer header. Using `curl` I put in this commmand `curl -u natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ http://natas4.natas.labs.overthewire.org/` 

<img width="947" height="392" alt="Screenshot 2026-04-08 154252" src="https://github.com/user-attachments/assets/87125d46-469a-4350-aed4-418128b2b5df" />

This reavealed the server was checking the 'Referer' HTTP header to make access control decisions. Since the Referer headers is fully client-controlled, it can be set to any value. 

"You didn't come from the right place, go away." Is the message I am getting, 




































