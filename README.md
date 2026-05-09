
<h2>Recent Work</h2>
<br>
<b>Mimikatz on Hardened Endpoint</b>
<br>On a recent test I was able to run Mimikatz on a hardened laptop via the WinPwn tool.  I ran into blocks attempting to run Mimiktaz using AMSI-bypass techniques including many from https://github.com/S3cur3Th1sSh1t/Amsi-Bypass-Powershell which is not very surprising.  However WinPwn; https://github.com/S3cur3Th1sSh1t/WinPwn ran which contains many enumeration and exploit tools.  Since Mimikatz is loaded in an indirect way the endpoint allowed it to run.    


<br><br>
<b>Enumerating a Network - Getting Web Sites into Burp</b>
<br>I was put on a test where we soon discovered there was a large volume of web sites that needed to be looked over.  Burp is then the go to tool to analyze these. NMAP is the goto tool to scan a network to discover which IP addresses are running http services. 
nmap -p 80 -oN filename

<br>
<br>
<b>Recon Workflow</b>
<br><br>



