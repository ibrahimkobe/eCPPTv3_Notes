# WHOIS

- `whois --help` , to see what can you do with the tool `whois` 

- *We're trying to identify some ownership details of a website*
	1. `whois <Website, EX: hackersploit.org>` , now you will see some interesting information's
- *To identify some IP addresses*
	1. `host <hackersploit.org>` , its confusing a bit that we found some IP addresses while the web uses CloudFlare servers which it hides the IP's  using a proxy;  the reason is ==The IP addresses you are seeing belong to **Cloudflare**, not the actual target server (the "origin" server).==

---
# Passive DNS Enumeration

#### DNS Records

+ *A* - Resolves a hostname or domain to an IPv4 address.
+ *AAAA* - Resolves a hostname or domain to an IPv6 address.
+ *NS* - Reference to the domains nameserver.
+ *MX* - Resolves a domain to a mail server.
+ *CNAME* - Used for domain aliases.
+ *TXT* - Text record.
+ *HINFO* - Host information.
+ *SOA* - Domain authority.
+ *SRV* - Service records.
+ *PTR* - Resolves an IP address to a hostname

#### DNS Enumeration tools

- **DNSrecon**
	1. `dnsrecon -d hackersploit.org` , it will give you name servers & IP addresses
- **DNS dumpster** website 

---
# Reviewing Webserver Metafiles

- if you write in a search engine (Firefox, google) the website name and /robots.txt **it will show you more sensitive information.**
>**Example**: `hackersploit.org/robots.txt` 

---
# Web App Technology Fingerprinting

- **whatweb**
	1. `whatweb + WebName.com` , it will give detailed information about the website

---
# web Server Fingerprinting

- `ifconfig` , to identify the target IP address which you will find in eth1
- `nmap -sV -F 192.212.206.3` ,to identify open ports and to know the version its running on
- `searchsploit apache 2.4.18`, we will see what vulnerabilities there is for this version 
- `ls -al /usr/share/nmap/scripts/ | grep -e "http-"` , we are searching for nmap scripts to use (it will give us much more info) , the instructor recommended `http-enum` script
- `nmap -sV -p 80 --script=http-enum 192.212.206.3` , it will give you some crawl directives like `robots.txt` 
- open the web and search for 192.212.206.3 and then start one by one from what you got from the scan.

**We can also use another technique:**
- `msfconsole`
- `search auxiliary/scanner/http/http_version`
- `use auxiliary/scanner/http/http_version`
- `show options`
- `set RHOSTS 192.212.206.3`
- `run`

**Curl**
- To see what port 80 is hosting in the website while we don't have a browser in the lab:==`curl <target IP>` , this will download the webpage==


**What is DIRB?**

```bash
dirb http://192.212.206.3 /usr/share/metasploit-framwork/data/wordlists/directory.txt
```

**DIRB** is a tool used for **web content discovery**.

It tries to find:

- hidden directories
    
- hidden files
    
- admin panels
    
- backup files
    

It works by using a **wordlist** and testing many paths like:

```
/admin
/login
/uploads
/backup.zip
```

---
# ### Web Server Vulnerability Scanning

**Nikto**

| Command                                     | English Explanation                                                       | Arabic Explanation                                         |
| ------------------------------------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `nikto --help`                              | Displays all available Nikto options and parameters.                      | يعرض جميع أوامر Nikto وخيارات الفحص.                       |
| `nikto -h <url/ip>`                         | Performs a basic scan finding server versions and common vulnerabilities. | يفحص الموقع لاكتشاف إصدار الخادم والثغرات والملفات المهمة. |
| `nikto -h "<url>?page=test"`                | Scans a dynamic page with parameters (useful for finding LFI).            | يفحص صفحة تحتوي على باراميتر لاكتشاف ثغرات مثل LFI.        |
| `nikto -h <url> -Tuning 9`                  | Controls the type of tests run (e.g., 9 means all tests).                 | يحدد نوع الفحص (رقم 9 يعني فحص شامل لكل شيء).              |
| `nikto -h <url> -o nikto.html`              | Saves scan results into a file.                                           | حفظ نتائج الفحص في ملف.                                    |
| `nikto -h <url> -Format html -o nikto.html` | Saves results specifically in HTML format.                                | حفظ النتائج بصيغة HTML لعرضها في المتصفح.                  |

---
# File & Directory Brute Force
**Gobuster**

|Command|English Explanation|Arabic Explanation|
|---|---|---|
|`gobuster dir -u <url> -w <wordlist>`|Basic directory and file brute-force mode.|وضع البحث الأساسي عن المجلدات والملفات باستخدام قائمة كلمات.|
|`gobuster vhost -u <url> -w <wordlist>`|Virtual host enumeration (finds hidden subdomains on same IP).|اكتشاف المواقع المختلفة المستضافة على نفس السيرفر.|
|`gobuster dns -d <domain> -w <wordlist>`|DNS subdomain enumeration.|اكتشاف النطاقات الفرعية عن طريق خوادم DNS.|
|`-b 404,403` _(Flag)_|Ignores specific status codes to hide unwanted responses.|يتجاهل نتائج معينة مثل 404 (غير موجود) و 403 (ممنوع).|
|`-x .php,.txt` _(Flag)_|Searches for files with specific extensions.|يبحث عن ملفات بامتدادات معينة.|
|`-r` _(Flag)_|Follows HTTP redirects (301, 302).|يتبع التحويلات في الموقع.|
|`-t 50` _(Flag)_|Controls scan speed (threads).|زيادة سرعة الفحص.|
|`-e` _(Flag)_|Shows full URLs in the output.|عرض الرابط كامل في النتائج.|

- Directory Brute-force
1. `search dir_scanner`
2. `use auxiliary/scanner/http/dir_scanner`
3. `show options`
4. `run`

- FILES_BRUTE-FORCING
1. `search files_dir`
2. `use auxiliary/scanner/http/files_dir`
3. `show options`
4. `run
---
# Automated Recon with OWSAP Amass

- `sudo apt-get install amass`
- `amass --help`
- `amass enum -d <zonetransfer.me>` , this will show you the sub-domains
- `amass enum -passive -d zonetransfer.me` , this will perform a passive enumeration

- *I want to use the tool but export the results to a folder*
	1. `mkdir <ZTME>`
	2. `amass enum -d zonetransfer.me - passive -src -dir /home/kali/Desktop/ZTME/`

- *I want to make a brute force using the tool*
	1. `mkdir ZTME_Brute`
	2. `amass enum -d zonetransfer.me -src -ip -brute -dir /home/kali/eDesktop/ZTME_Brute/`
- *Amass Intel*
	1. `amass intel -active -whois -d zonetransfer.me -dir /home/kali/Desktop/ZTME_Intel/`
---
# Identifying & Exploiting Reflected XSS Vulnerabilities

- *WPscan Install*
	1. `sudo apt-get install wpscan` , (WPscan: is a popular open-source security scanner designed specifically for **WordPress** websites.)

- *WPscan commands:*
	1. `wpscan --help` , opens the help menu
	2. `wpscan -url <THE URL OF THE WEB>` , to scan the url
	3. `wpscan --url https://shajmedc0n826inmmw9c0wkuh.us-east-6.attackdefensecloudlabs.com/ --enumerate p --plugins-detection aggressive` , to scan the plugins and to switch from passive enumeration to active.
	4. `wpscan --url https://shajmedc0n826inmmw9c0wkuh.us-east-6.attackdefensecloudlabs.com/ --enumerate p --plugins-detection aggressive --api-token Qa3nuuuf1skis9KLn8qBIiu1B2xc5bsdn2efs6nOeN4` , Uses your personal WPScan API token to query the vulnerability database. If a detected plugin has known security vulnerabilities, WPScan reports them.

- **DON'T Rely only on WPscan**
>We recommend using: `searchsploit <the name of the plugin we want to exploit EX: Relevanssi>`

---
# Identifying & Exploiting Stored XSS Vulnerabilities

- *Installing huepy*
	1. `pip install huepy`
- `cd Desktop`
- `ls`
- `cd MyBBscan`
- `ls`
- `./scan.py`
- now we have to paste the url of MyBB , and the tool will start the scanning
- we have to put this payload : `<BODY ONLOAD=alert("XSS")>` in title of download box
![[Pasted image 20260708092855.png|600]]

---
# Identifying & Exploiting DOM-Based XSS Vulnerabilities

**DOM-based Cross-Site Scripting (DOM XSS)** is a type of web vulnerability where the attack payload is executed entirely within the victim's browser as a result of modifying the DOM (Document Object Model) "environment."

Unlike Stored or Reflected XSS—where the server accidentally includes malicious scripts in the HTML page it sends back—in DOM XSS, the server itself might be completely secure. The vulnerability exists entirely in the **client-side JavaScript**.

![[DOM-XSS-Pic5-e1650538987146.png|300]]
## How It Works: Sources and Sinks

To understand DOM XSS, you need to understand two critical concepts:

1. **The Source:** This is a JavaScript property that accepts user-controlled data. An attacker can manipulate this data.
    
    - _Examples:_ `location.search` (the query string in the URL), `location.hash` (the part of the URL after the `#`), `document.referrer`, or `window.name`.
        
2. **The Sink:** This is a potentially dangerous JavaScript function or DOM object that executes code or renders HTML based on the data it receives.
    
    - _Examples:_ `document.getElementById().innerHTML`, `document.write()`, `eval()`, or `setTimeout()`.
        

A DOM XSS vulnerability occurs when an application takes data from a **Source** and passes it directly to a **Sink** without proper sanitization or validation.

## A Concrete Example

Imagine a simple web page that greets the user based on a URL parameter.

**The Vulnerable Code:**

HTML

```
<div id="greeting"></div>

<script>
  // 1. SOURCE: Grabbing the user's name from the URL query string
  const urlParams = new URLSearchParams(window.location.search);
  const userName = urlParams.get('name');

  // 2. SINK: Putting that name directly into the webpage's HTML
  if (userName) {
    document.getElementById("greeting").innerHTML = "Welcome back, " + userName + "!";
  }
</script>
```

**The Normal Use Case:**

A user visits `[https://example.com/welcome.html?name=Alice](https://example.com/welcome.html?name=Alice)`.

The page grabs "Alice" from the URL and safely renders: `Welcome back, Alice!`

**The Attack:**

An attacker tricks a victim into clicking a malicious link:

`[https://example.com/welcome.html?name=](https://example.com/welcome.html?name=)<img src=x onerror=alert('Hacked!')>`

Because the JavaScript uses `innerHTML` (a dangerous sink), the browser tries to render the image. The image source `x` fails to load, triggering the `onerror` event, which executes the attacker's JavaScript (`alert('Hacked!')`). The attacker can use this to steal session cookies, capture keystrokes, or redirect the user.

## How to Prevent DOM XSS

Fixing DOM XSS requires treating all user-controlled data as untrusted and handling it safely before it reaches a sink.

- **Use Safe Sinks:** The most effective fix is to avoid executing or rendering HTML if you only need to display text. Use properties like `textContent` or `innerText` instead of `innerHTML`.
    
    - _Fixing the example above:_
        
        `document.getElementById("greeting").textContent = "Welcome back, " + userName + "!";`
        
        (If the attacker tries the payload here, the browser will literally just print `<img src=x...` as text on the screen, nullifying the attack).
        
- **Sanitize HTML:** If you _must_ use `innerHTML` (e.g., you are building a rich text editor), use a proven client-side sanitization library like **DOMPurify**. It strips out dangerous tags and attributes before they hit the DOM.
    
- **Avoid Execution Sinks:** Never pass user-controlled data to functions like `eval()`, `setTimeout()`, or `setInterval()`.

---
# Finding SQL Injection Vulnerabilities With OWSAP ZAP

OWASP ZAP can be used to perform automated SQL injection testing through its built-in **Fuzzer**, using a predefined payload list rather than a manually crafted injection string.

|Step|Action|
|---|---|
|1|Open OWASP ZAP and click the browser icon (top-middle) to launch the built-in browser.|
|2|Launch the target website within this ZAP browser instance so traffic is proxied through ZAP.|
|3|In the **Sites** tree, right-click the API request containing the parameter to be tested → select **Attack** → **Fuzz**.|
|4|Click **Edit** → remove the existing parameter value → **Save**.|
|5|Select the now-empty parameter value field and click **Add** to define a fuzz location.|
|6|Choose payload type: **File Fuzzers → jbrofuzz → SQL Injection** → **Add** → **OK** → **Start Fuzzer**.|
|7|Review results: check the **State** column in the Fuzzer tab for entries marked **"Reflected"**, indicating a potential injection point.|

**Usage:** Automated SQL injection testing using a predefined list of payloads (jbrofuzz SQL Injection list), useful for quickly identifying reflected/vulnerable parameters without manual payload crafting.

---
## SQLMap

SQLMap is an automated tool used to detect and exploit SQL injection vulnerabilities in web application parameters, and to take over the underlying database.

|Command|Usage|
|---|---|
|`sqlmap -u TARGETURL --data "PARAMETERNAME" -p PARAMETERNAME --method POST/GET`|Attempts SQL injection directly against a target URL and parameter, specifying the HTTP method (POST/GET).|
|`sqlmap -r REQUESTFILE -p PARAMETERNAME --technique=E`|Runs an SQLMap scan using a captured request file (an HTTP request saved from Burp Suite). The `--technique` flag specifies the injection type to test: **E** = Error-based, **B** = Boolean-based, **T** = Time-based, **U** = Union-based, etc.|
|`sqlmap -r REQUESTFILE -p PARAMETERNAME --technique=E --current-db`|Enumerates the current database type, name, and version.|
|`sqlmap -r REQUESTFILE -p PARAMETERNAME --technique=E -D DBNAME --tables`|Lists all tables inside the specified database (`-D DBNAME`).|
|`sqlmap -r REQUESTFILE -p PARAMETERNAME --technique=E -D DBNAME -T TABLENAME --dump`|Dumps all data from the specified table (`-T TABLENAME`) within the specified database.|
|`sqlmap -r REQUESTFILE -p PARAMETERNAME --technique=E --current-user`|Identifies the database user account through which access was obtained (useful for assessing privilege level).|

### Workflow Summary

1. Capture the target request using **Burp Suite** and save it as a request file.
2. Point SQLMap at the request file with `-r` and specify the vulnerable parameter with `-p`.
3. Choose an injection `--technique` based on how the application responds (errors, boolean logic, time delays, or UNION results).
4. Once injection is confirmed, enumerate the database (`--current-db`), then tables (`--tables`), then dump data (`--dump`).
5. Optionally identify the privilege context with `--current-user`.

---
## Quick Comparison

|Aspect|SQLMap|OWASP ZAP Fuzzer|
|---|---|---|
|Approach|Command-line, automated exploitation engine|GUI-based, fuzzing with payload lists|
|Input|Request file / URL + parameter|Captured request within ZAP's Sites tree|
|Output|DB enumeration, table listing, data dump, user context|Reflected/error responses per payload (indicator only)|
|Best for|Confirmed exploitation & data extraction|Initial discovery/detection of injectable parameters|

---
 