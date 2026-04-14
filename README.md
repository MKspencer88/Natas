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

"You didn't come from the right place, go away." Is the message I am getting. '

The command `curl -u natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ -e "http://natas5.natas.labs.overthewire.org/" http://natas4.natas.labs.overthwire.org/` 

<img width="944" height="443" alt="Screenshot 2026-04-08 154751" src="https://github.com/user-attachments/assets/9e091ca1-e0f9-45ab-a36b-1c9d1465afef" />

The `-e` flag stands for referer. It manually sets the Referer header in the HTTP request to whatever value you give it. 

### What I Learned 

HTTP headers are fully client-controlled and cannot be trusted for access control. The Referer header can be set to any value using curl, Burp Suite, or browser extenstions. Real access control requires server-side session tokens that cannot be forged. 

### Real-World Impact

Maps to OWASP A01, broken access control(#1 web security risk). Referer-based checks have been found in the wild protecting admin panels, password reset flows, and API endpoints. All trivially bypassed with a single curl command. 

# Natas 4 ---> 5 

**Vulnerability:** Weak authentication using client-side modifiable cookies

**Difficulty:** Easy

**Category:** Web Security / Authentication Flaws

### What I Did

When I first went to `http://natas5.natas.labs.overthewire.org` the error on the page said "Access disallowed. You are not logged in." 

<img width="938" height="347" alt="Screenshot 2026-04-09 113308" src="https://github.com/user-attachments/assets/3882a155-39ab-48f8-b37c-703cbdaebb7e" />

I'll admit I wasn't sure where to go from here so I did some research and all signs were pointing me to look at the sessions cookies. So I opened `Web developers tools --> storage ---> cookies` 

<img width="960" height="291" alt="image" src="https://github.com/user-attachments/assets/c33707f8-a109-4107-9a48-9c28b882cb5f" />

I saw a cookie named `loggedin` with a value set to `0`. This mean the server is trusting the client to tell it whether the user is autenticated. 

I clicked on the value and changed it from `0` to `1` and refreshed the page 

<img width="973" height="279" alt="Screenshot 2026-04-09 121024" src="https://github.com/user-attachments/assets/a43ebd90-6d42-4b5d-bd31-d440151653f4" />

<img width="944" height="582" alt="image" src="https://github.com/user-attachments/assets/6d00170e-cefc-4b09-b5ea-3be3916b608f" />

### What I Learned
- Never trust client-side state for authentication, cookies can be modified by the user at anytime.
- Authentication must be enforced server-side. The server should validate sessions using signed cookies, server-side session storage, and tokens that cannot be forged.
- Binary flag like `loggedin=0/1` are insecure. Attackers can flip them instantly. This level reinforces a core principle, if the user can modify it, it cannot be used to enforce security.

### Real-World Impact
- This vulnerability mirrors real-world issues such as sites using `isAdmin=true` cookies, applications trustint client-side JWTs without signatures, debug flag left in production, session IDs that are predictable or modifiable.
- Attackers can exploit these to bypass login, escalate privileges, access admin panels, impersonate other users.
- This is a classic example of broken authentication, one of the OWASP Top 10.

# Natas 5 ---> 6

**Vulnerability:** Sensitive server-side secrets stored in publicly acccesible files. 

**Difficulty:** Easy

**Category:** Web Security / Server-Side Code Analysis

### What I Did

When I logged on to `http://natas6.natas.labs.overthewire.org` this was the main page. 

<img width="948" height="393" alt="image" src="https://github.com/user-attachments/assets/9aa3686b-cd90-4f53-b5b8-80e2c0e1ab77" />

I clicked on `View Source Code` and it a certain part stood out to me. 

<img width="952" height="668" alt="image" src="https://github.com/user-attachments/assets/3c5d020a-69bf-4e9f-b7ef-f1444fbf8174" />

I noticed that `include "includes/secret.inc"` so if I were to add `includes/secret.inc` to the URL hopefully it gave me the secret needed. At first it showed me a blank page bu then I remembered to view the source code. 

<img width="362" height="134" alt="image" src="https://github.com/user-attachments/assets/8bfa5e6c-f557-4881-b93f-f09bcf815a47" />

<img width="888" height="281" alt="image" src="https://github.com/user-attachments/assets/b1896368-0ecf-4f91-b9cd-5806bf8b3779" />

And when I submit that query it revealed the password to natas7 to me. 

<img width="858" height="268" alt="Screenshot 2026-04-09 130238" src="https://github.com/user-attachments/assets/d80ce704-df79-4590-afed-20587d30e2ed" />

### What I Learned
- Included files can be directly accessible if stored inside the web root. If a developer places sensitive configurations files in a publicly reachable directory, attackers can retrieve them simply by navigating the path.
- Server-side secrets must never be stored in web-accessible locations. They should be placed outside the document root or protected by server configuration.
- Understanding PHP behavior is essential for web exploitation. If the the code references an external file, alwasy check whether that file is exposed.
- Security depends on sever configuration, not just code.

### Real-World Impact
- This vulnerability is extremely common in real environments, developers accidentally deploy `.inc`, `.bak`, `.old`, or `.php~` files. Miconfigured Apaches/Nginx servers allows directory traversal, backup files or environment configs are left in public folders, and sensitive key or credentials are stored in web-accessible directories.
- Attackers routinely find, database password, API keys, internal configuration files, source code and secrets used for authentication or encryption.
- This is a classic example of information disclousure due to insecure file placement. A frequent finding in pen tests and bug bounty programs.

# Natas 6 ---> 7 

**Vulnerability:** Directory traversal via unsanistized `page` parameter

**Difficulty:** Easyish

**Category:** Web Security / Path Manipulation

### What I Did

When I logged into `https://natas7.natas.labs.overthewire.org` it displayed two links. 
- `Home`
- `About`

<img width="958" height="310" alt="image" src="https://github.com/user-attachments/assets/5b140ce4-b3e5-48eb-885b-9f7b9548035c" />

The URL changed depending on which link I clicked. 
```
?page=home
?page=about
```
Taking what I learned from the previous levels I clicked on view source code and in the code I was given a hint on where to find the password for Natas8. 

<img width="948" height="455" alt="image" src="https://github.com/user-attachments/assets/e166d585-8bc5-4037-ab47-b7a4b38a7d27" />

the hint told me that I could find the password in `/etc/natas_webpass/natas8`. 

In the URL after `?page=` I entered `/etc/natas_webpass/natas8` when I pressed enter it revealed the new password to me. 

<img width="932" height="392" alt="Screenshot 2026-04-09 133728" src="https://github.com/user-attachments/assets/16e4121e-edbf-4e20-9fa5-5dba27d40e3b" />

### What I Learned
- Never pass user input directly into file fucntion. Functions like `include()`, `require()`, `file_get_contents()`, and `fopen()` are dangerous when combined with unsanitized parameters.
- Directory traversal is one of the oldest and most reliable web vulnerabilities. Attackers can use `../` to absolute paths to access sensitive files.
- Server-side file inclusion must always be validated. Developers should use whitelists, restrict to specific directories, and avoid dynamic includes entirely.
- User input shoud never control which server-side files are execute or read.

### Real-World Impact
- Directory traversal vulnerabilities are extremely common in, PHP applicatons using dynamic includesm CMS plugins, legacy codebases, poorly validated file download endpoints, and debug or devlopment tools left exposed.
- Attackers can use this flaw to read configuration files, extract database credentials, access environment variables, read source code, and escalate to remote code execution.
- This a textbook example of path traversal leading to sensitive data exposure, a critical issue in penetration testing and bug bounty work.

# Natas 7 ---> 8

**Vulnerability:** Predictable server-side encoding used to "hide" a secret

**Difficulty:** Medium

**Category:** Web Security / Encoding and Logic Reversal

### What I Did

When I first logged into `https://natas8.natas.labs.overthewire.org` and it was another `input secret` box. 

<img width="856" height="286" alt="image" src="https://github.com/user-attachments/assets/107cd0f0-b712-4dbc-90eb-560a3b5b4115" />

I clicked the `view source code` link and reading through the code and this code is what stood out to me. 

<img width="540" height="241" alt="Screenshot 2026-04-09 141351" src="https://github.com/user-attachments/assets/fd9ede4c-8717-4444-bbd1-efea552b6bd9" />

What I need to do is to decode it. According to the code it was converted from text to a hexstring. In my terminal
I ran the command `echo "3d3d516343746d4d6d6c315669563362"" | base64 -d` and this commmand decodes this hexstring into a base64 string. 

<img width="465" height="77" alt="image" src="https://github.com/user-attachments/assets/11f467b1-ef8d-4e6e-99aa-42572b026ceb" />

According to the code I found in the source code it said the string was reversed. So my output was `==QcCtmMml1ViV3b` but in order to get the correct `secret` I revesered it. 

My next command `echo "b3ViV1lmMmtCcQ==" | base64 -d` 

<img width="348" height="69" alt="image" src="https://github.com/user-attachments/assets/cbf2808a-22ea-400a-bb59-f7bff4d6bb1a" />

I input this output into the input box it gave me the secret. 

<img width="760" height="364" alt="Screenshot 2026-04-10 125923" src="https://github.com/user-attachments/assets/dbe1dfa7-959d-41b1-828e-476a397b7073" />

### What I Learned
- Encoding is not encryption. Anyone can reverse encoding if algorithm is visible.
- Security through obscurity is not security. Hiding a secrety behing reversible transformations does not protect it.
- Source code analysis is a powerful technique. When you can see the logic, you can replicate or revsere it.
- The attacker can see the algorithim, they can reverse the algorithm.

### Real-World Impact
- This vulnerability mirrors real mistakes developers make storing API keys or secrets "encoded" form, using reversible transformations instead of proper encrytpion. Assuming attackers won't read reverse client-side logic and obfuscating values instead of protecting them.
- Attacker routinely reverse base64, hex, ROT13, XOR, custom "encryption" functions, and JavaScript obfuscation.

# Natas 8 ---> 9 

**Vulnerabilities:** Command Injection via unsanitized user input passed to `grep`

**Difficulty:** Medium

**Category:** Web Security / Command Injection

### What I Did

When I first logged into to Natas9, there wasn't on anything on the screen to indicate any sort of clues. 

<img width="820" height="296" alt="image" src="https://github.com/user-attachments/assets/851b058c-b3a7-448c-97db-65a05766d2b5" />

I clicked the `View sourcecode` link to inspect the backend logic. 

<img width="570" height="304" alt="image" src="https://github.com/user-attachments/assets/e672b643-685e-4734-8456-03133a5c6cfb" />

This is a command injection vulnerability because:
- User input(`needle`) is inserted directly into a shell command
- There is **no sanitization**
- The command is executed using `shell_exec()`

Meaning I can break out of the `grep` command and run any shell command the server allows. 

To break out of the `grep` command I used this `; cat /etc/natas_webpass/natas10` the `;` ends the original command and starts a new one. So the full URL becomes `http://natas9.natas.labs.overthewire.org/?needle=%3B+cat+%2Fetc%2Fnatas_webpass%2Fnatas10&submit=Search` 

When I pressed search it revealed the password to me. 

<img width="961" height="393" alt="Screenshot 2026-04-10 143900" src="https://github.com/user-attachments/assets/67a2125b-1b40-48fe-b93e-8e50840d444f" />

### What I Learned
- Never pass user input directly into shell commands functions like `shell_exec()`, `exec()`, `system()` and backticks(`command`) are extremely dangerous when combined with unsanitized input.
- Attackers can chain commands using `;`, `&&`, `|`, backticks, and `$()`
- Command injection is one of the most severe vulnerabilities it often leads to file disclousure, remote code execution, and full system compromise.
- If user input reaches the shell, assume the attacker owns your server.

### Real-World Impact
- Command injection is devestating in real systems attackers can read `/etc/password`, SSH keys, API keys, modifiy of delete files, pivot into terminal networks, install backdoors, and execute arbitrary code.
- This vulnerability is frequently found on web apps calling system utilities, file search features, PDF/image converters, backup scripts, IoT devices, routers and embedded systems.
- This is why secure coding guidelines strongly recommend avoiding shell calls entirely, using parameterizewd APIs, escaping input, and validating against strict whitelists.

# Natas 9 ---> 10 

**Vulnerability:** Filtered command injection (bypassing blacklists)

**Difficulty:** Medium --> Medium-High

**Category:** Web Security / Command Injection / Filter Evasion

### What I Did

When I logged into Natas10 it was almost identical to Natas9. I clicked `View sourcecode` 

<img width="958" height="421" alt="image" src="https://github.com/user-attachments/assets/4ac7b456-5543-45f0-826c-a1263408af5c" />

The script is similar to the one in Natas9, except the server blocks, `;`, `|`, `&`, these characters we used in Natas9 to break out of the `grep` command. So now I need to figure out a new way to escape the `grep` commmand. 

It took me a minute to find the correct command that could escape the `grep` command. The one that finally worked for me was `cat . /etc/natas_webpass/natas11` and it gave me the password and a bunch of other noise but I got the password. 

<img width="929" height="502" alt="image" src="https://github.com/user-attachments/assets/73aa460f-dc1d-4380-9492-8a6b9e3dca5f" />

### What I Learned
- Blacklists are weak, blocking a few chatacters does not stop attckers. They simply use alternative operators.
- Command substitution is extremely powerful.
- Input validation must be whitelisted-based, not blacklisted based. The only safe approach is strict whitelists, escaping, and avoiding shells commands entirely.
- If user input reaches the shell, assume it can be exploited, even with filters.

### Real-World Impact
- This vulnerability is common in web apps that call systems utilities, file search features, backup scripts, IoT devices, and admin panels with diagnostic tools.
- Attackers can use filter bypasses to read sensitive files, execute arbitrary commands, escalate privileges, and pivot deeper into the system.
- This is why modern secure coding practices strongly discourage shell calls with user input.

# Natas10 ---> 11

**Vulnerability:** Weak XOR "encryption" + trusting client-side cookies

**Difficulty:** Medium-High

**Category:** Web Security / Crypto Misuse / Cookie Tampering 

### What I Did

When I loaded Natas11, the page tells me: **"Cookies are protected with XOR encryption"** 
There is also a link to view to source code so I clicked on it. 

**XOR:** exclusive OR is a logical/bitwise operation. 

The Rule the XOR outputs true only when the input are different. 

A     B    A XOR B 
0     0    0 
0     1    1 
1     0    1
1     1    0 

Think of it as one or the other not both. 

<img width="927" height="555" alt="image" src="https://github.com/user-attachments/assets/6aea98f4-dd26-4add-894e-e848acd5e30a" />

The source code revealed:
- The sever reads a cookie named `data`
- It base64-decodes it
- it XOR-decrypts it using a repeating key
- It expects the decrypted JSON to look like

The keyword that stood out to me was `cookie`. That clued me in that I need to look at the session cookies. This is going to require me to write a few python scripts. The first script is desgined to find the key and the second one is going to forge a cookie to insert in order to get the password. 

findthekey.py

<img width="552" height="237" alt="image" src="https://github.com/user-attachments/assets/e9947acf-fc24-4203-a13d-5d4abb47df1c" />















































































