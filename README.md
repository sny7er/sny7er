<h2>Recent Work</h2>
<br>
<b>Mimikatz on Hardened Endpoint</b>
<br>On a recent test I was able to run Mimikatz on a hardened laptop via the WinPwn tool.  I ran into blocks attempting to run Mimiktaz using AMSI-bypass techniques including many from https://github.com/S3cur3Th1sSh1t/Amsi-Bypass-Powershell which is not very surprising these days.  However WinPwn; https://github.com/S3cur3Th1sSh1t/WinPwn ran which contains many enumeration and exploit tools.  Since Mimikatz is loaded in an indirect way the endpoint allowed it to run.    

<br><br>

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
c. Then you cat that file into burp using 'cat net.scan --proxy localhost:8080'<br>
   You should now see the web sites appearing in Burp<br>




