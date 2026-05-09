<h2>Recent Work</h2>
<br>
<b>Mimikatz on Hardened Endpoint</b>
<br>On a recent test I was able to run Mimikatz on a hardened laptop via the WinPwn tool.  I ran into blocks attempting to run Mimiktaz using AMSI-bypass techniques including many from https://github.com/S3cur3Th1sSh1t/Amsi-Bypass-Powershell which is not very surprising these days.  However WinPwn; https://github.com/S3cur3Th1sSh1t/WinPwn ran which contains many enumeration and exploit tools.  Since Mimikatz is loaded in an indirect way the endpoint allowed it to run.    

<br><br>
<b>XSS Findings</b><br>
I've found quite a few stored and reflected xss, I enjoy discovering them.  I want to post more about this but for now I just wanted to share a few things.
My typical process is to throw in quick payload into fields like <img src=xss> and see what happens.  Then I'll send those thru burp and see what reflects.  If the >< get converted to &lt; that gives me an idea of what's going on.  I also like to look at the cookies and response headers to see if a WAF or F5 is in play.  Google some of the cookie names and you'll get an idea of the tehnology you're dealing with.   I use developer tools to see where the payload is landing in terms of it's context and what is going on.   Is the payload landing between html tags <> or within a value "" ..  what kind of break-out do I need.   



<br><br>
<b>Enumerating a Network - Getting Web Sites into Burp</b>
<br>I was put on a test where we soon discovered there was a large volume of web sites that needed to be looked over.  Burp is then the go to tool to analyze these. NMAP is the goto tool to scan a network to discover which IP addresses are running http services. 
nmap -p 80 -oN filename

<br>
<br>
<b>Recon Workflow</b>
<br><br>
