
##  Level 16 - Level 17

Tags:
#Bandit 

---

## Level Goal

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

---

## Commands

Commands you may need to solve this level

ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

---

## Tips

Helpful Reading Material

[Port scanner on Wikipedia](https://en.wikipedia.org/wiki/Port_scanner)


---

## Analysis

This level requires scanning a range of ports (31000–32000) on localhost to find which ones are open. Among the open ones, only one uses SSL/TLS and returns the actual credentials after you send the current password. All other services either echo input or reject the connection. Tools like `nmap` help quickly identify open ports, and `openssl s_client` allows checking which ports expect encrypted connections.

---

## Methodology
**Step 1:** **Scan ports 31000–32000 to find open ones**

`nmap -p31000-32000 localhost`

You will typically find only a few open ports (e.g., 31518, 31790, 31691, etc.).


<img width="668" height="237" alt="image" src="https://github.com/user-attachments/assets/14efa6ce-40e2-42b3-a3a7-2de7a16de971" />


**Step 2:** **Test each open port to see which one uses SSL/TLS**

Example:

`openssl s_client -connect localhost:31790`

If you see TLS handshake info, it's SSL/TLS.  
If it errors out or hangs, it's plain text.

<img width="1317" height="385" alt="image" src="https://github.com/user-attachments/assets/ee2f2b8d-294e-44e5-bd1b-b5779ad3c619" />


<img width="780" height="349" alt="image" src="https://github.com/user-attachments/assets/93fc3a8a-23de-450c-b544-a0c5e1b660c9" />



**Step 3:** **Send your Level 16 password to the SSL port**

Once connected using `openssl s_client`, paste your Level 16 password and press **Enter**.  
The server will respond with the **private SSH key** for bandit17.

Copy that key and save it as `bandit17.key` locally.



**Step 4:** Copy the password and log in to the next level

Use SSH again, replacing the username with the next level:

ssh bandit17@bandit.labs.overthewire.org -p 2220

When asked, paste the password you found.

---

## Password


---

## Reflection

This level combined port scanning, protocol identification, and SSL/TLS testing—mimicking real security reconnaissance workflows. It reinforces using `nmap` for discovery and `openssl s_client` for encrypted service verification.

---

