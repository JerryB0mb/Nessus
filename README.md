# Nessus Basics
-- The Fundamentals of Vulnerability Management &amp; Remediation --

Tools used:

- Oracle VirtualBox (https://www.virtualbox.org/)
- Tenable Nessus (https://www.tenable.com/)
- Virtualized Windows 10 machine. 

Tenable Nessus is a powerful tool used by organizations to keep attack surface to a minimum and ensure all regulatory patches occur. This project goes over the basics of Nessus, comparing credentialed vs. non-credentialed scans, and remediating vulnerabilities.

I created a Windows machine via VirtualBox, then installed Nessus on my local machine. In my VM, I navigated to my Windows Defender Firewall, and ensured my Domain, Private and Public profiles were all turned OFF. This is to ensure that the machine will accept all ICMP (ping) requests. 
After ensuring connectivity to my VM, I ran a Basic Network scan in Nessus. (NO credentials, nothing added onto the scan.) 

I ensured that I selected “Host-only Adapter” in Virtualbox, otherwise Nessus was unable to find my VM, despite being able to ping it from my local machine. 
After a few minutes, Nessus finished scanning the machine and found 16 vulnerabilities (yours may vary), as well as a CVSS score.


![nessus](https://github.com/JerryB0mb/Nessus/assets/49531794/7d41f3c7-789c-428a-80a0-7286a1c4b621)


Clearly there’s already a predetermined hierarchy of vulnerabilities that Nessus has categorized, but the majority of alerts you receive here should just be labeled as “Info”. These aren’t explicitly vulnerabilities, but something you should be aware of. An example of this is finding the Traceroute alert, which demonstrates that ICMP is allowed to ping this machine.


![Inkednessus2](https://github.com/JerryB0mb/Nessus/assets/49531794/eda43634-9246-493c-a8cb-aa24ccfba99f)


Another one is “**Target Credential Status by Authentication Protocol”,** which is only a result of us not giving Nessus any credentials to the host we’re scanning. 

Let’s execute a credentialed scan, and compare results.

Go to services.msc in search bar, and enable Remote Registry to Automatic. Go to User Account Control Settings and turn down the slider to disable any notifications from Windows when apps try to install software or make changes (Usually a dangerous practice).

Enable file and printer sharing through Control Panel. Mine was already on, but check just in case.

Go into Regedit and add a key. HKEY_LOCAL_MACHINE > SOFTWARE > MICROSOFT  > WINDOWS > CURRENT VERSION > POLICIES > SYSTEM - Create a DWORD called LocalAccountTokenFilterPolicy, set the value to 1. Restart the VM.

Go back to Nessus Essentials, and configure your saved scan and add a credentialed scan via “Credentials” tab. 


![nessus3](https://github.com/JerryB0mb/Nessus/assets/49531794/7a6e622e-c7c9-4514-bef5-2cd28c0854e0)


After completing a scan (it takes a while), you’ll definitely have a LOT more vulnerabilities, with some in the “critical” section. 

Click on some vulnerabilities, and check them out. You can see the CVSS score on the side, the details on the vulnerability, what it includes, and a brief solution. 


![nessus4](https://github.com/JerryB0mb/Nessus/assets/49531794/10ea83db-250c-4298-86f3-fd00efa79cfd)


Notice there are a lot more "Mixed" options here. This just means varied alerts are organized in groups, as clicking on them will reveal more vulnerabilities, including critical ones. Here's me viewing a MS 365 code execution vulnerability that apparently exists on the machine. Notice how it also gives the solution on the same page, as well as the CVSS scores (both 2.0 and 3.0) on the right.


![nessus5](https://github.com/JerryB0mb/Nessus/assets/49531794/f79f7163-9cce-44f7-ba93-d9fe2bee8d27)


You can also go to the Remediations tab, and view all high-level remediation options Nessus gives you. 


![nessus6](https://github.com/JerryB0mb/Nessus/assets/49531794/ebe2c248-a0dd-4295-a0df-5932b2827e9f)


In enterprise/corporate environments, you obviously wouldn’t have to go through each individual vulnerability and remediate them manually. There should and will always be 3rd party options to automate things like OS and software patching, etc.

Let's make this more interesting - for this project, I chose an outdated version of Firefox to install to purposely increase the attack surface of this host. Any old version will do. (DO NOT install an outdated release on your local machine! Make sure you’re doing this in your VM. You can find Mozilla’s archive with the link above.)

https://ftp.mozilla.org/pub/firefox/releases/?_gl=1*1ys7kjl*_ga*MTQ1NjM2NDE1Ny4xNzEyNjkxNTY2*_ga_2VC139B3XV*MTcxNDgzODAwOS4xLjAuMTcxNDgzODAwOS4wLjAuMA

Run another scan, and let’s see what we find. 


![Nessus7](https://github.com/JerryB0mb/Nessus/assets/49531794/e57e75d3-0af7-4064-8812-f848e5fb943d)


We have a LOT more vulnerabilities this time. Upon viewing the Vulnerabilities tab, you’d see that there are literally hundreds of vulnerabilities, many are associated with each individual deprecated version of Firefox that are under the latest version. As time goes on, the amount of vulnerabilities this causes will go up, as Firebox gets more updates - of course, if you view the Remediations tab, simply updating Firefox removes a solid amount of these. 

So, let’s do so. Fun fact: open the Run.exe app, and type appwiz.cpl - this takes you directly to the list of programs you have installed (control panel). Uninstall Firefox, then search “Check for updates” in your search bar, and update all that you can. You’ll have to restart the machine at least once to do this. Once again, do another Nessus scan.


![nessus8](https://github.com/JerryB0mb/Nessus/assets/49531794/55c74cb9-ebc6-4db4-ad45-d004ff106926)


So, still a few vulnerabilities somehow - we reduced it by quite a lot by just simply removing Firefox and updating Windows. But this is the manual process, and Nessus makes this mindnumbingly easy to do. Obviously, it's a great solution to monitor your network. There are settings to enable scheduled scans, further automation and other requisites to your credentialed scans. The key to learning any tool is to learn it in your own time CONSISTENTLY, but this project helped me get on my feet.

Thanks for reading! 

Inspiration: https://www.youtube.com/watch?v=lT6Px9zJM3s&t=1s&ab_channel=JoshMadakor




