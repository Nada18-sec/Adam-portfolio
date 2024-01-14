---
title: "Threat Hunting and Threat Hunting Frameworks"
description: "Threat Hunting and Threat Hunting Frameworks"
dateString: Jan 2024
draft: false
tags: ["Threat Hunting", "Frameworks", "Vulnerabilities", "Incident Response"]
weight: 100
cover:
    image: "/blog/threat-hunting/Threat-Hunting.jpg"
---

# What is Threat Hunting?
A quick google search on the term threat hunting brings the following result; "*Threat hunting is the practice of proactively searching for cyber threats that are lurking undetected in a network*". Look at it this way, when an organization employs initial endpoint security mechanisms, malicious actors can still be able to go through their defenses undetected. Since a lot of organizations lack the advanced detection capabilities needed to stop the advanced persistent threats (APT) from remaining in the network, threat hunting is a crucial step in ensuring the improved security posture of the organization. 
The main reason for threat hunting is that it is a more proactive approach to defending an IT environment. This is because it does not wait for an alert to be triggered and doesn't require the administrative overhead to whitelist all approved actions. It takes into account the current vulnerabilities, environment, and processes to apply human expertise against the evidence. Further it provides a completely different perspective from that of the Security Operations Center (SOC) since it aims at looking at the evidence on the endpoints to determine whether there was some activity that was missed or the SOC tools haven't been updated to detect the threats.

# The need for a Threat Hunting Framework

Because we have established that threat hunting is a structured search for evidence of malicious activities that have not yet triggered security alerts, the reality is that the methodology may vary from one organization to the next. It may vary from simple processes like matching file hashes to complex methods like analytics and machine learning (ML) investigations depending on the maturity level of the threat hunting team/SOC.
For instance, a security team starting out in threat hunting would ideally start by hunting for Indicators of Compromise (IoCs) on their endpoints in relation to recent threat intelligence feeds. This may include a technique matching file hashes against existing repositories on VirusTotal (VT).
An intermediary team would take it a notch higher and customize their IoC lists by incorporating contextual information to improve the quality of the intelligence. Further, some would employ machine learning-aided anomaly detection to better understand baseline conditions in the environment.
A mature team would form a hypothesis and use it as a driving force for their hunts. This will be aided by anomaly detection and properly selected IoCs as inputs that would inform a more comprehensive understanding of the organization's specific risk profile.
Alright, we are ready to hunt but where do we start?

# Threat Hunting Frameworks

A threat hunting framework is a system of adaptable, repeatable processes designed to make your hunting expeditions both more reliable and more efficient. The choice of framework should be based on the following key areas:
a. It should answer the "why". The choice of framework should aim at answering the reason why the hunt is being conducted in the first place.
b. It should endeavor to approve/disapprove a formulated hypothesis.
c. It should identify and utilize the best data sources to test the hypothesis formulated.
The types of frameworks are as shown below:

1. ## The Sqrrl Threat Hunting Reference Model (2015)

Sqrrl's framework was not only the first, but remains one of the most influential threat hunting frameworks. It defines the **hypothesis-driven hunting process** as a loop with four stages:

a. Create hypothesis

b. Investigate via tools & techniques

c. Uncover new patterns & TTPs

d. Inform & enrich automated analytics

2. ## TaHiTI: Targeted Hunting Integrating Threat Intelligence (2018)
The TaHiTI framework, created by a consortium of financial institutions known as the Dutch Payments Association, is another popular threat hunting framework. We can summarize TaHiTI as:

a. Building on pieces of the Sqrrl framework, such as the Hunting Maturity Model.

b. Adding a new type of hunt, known as the **unstructured or data-driven hunt**.

c. Providing more detailed guidance on the hunting process and on potential metrics for hunt program success.

3. ## PEAK: Prepare, Execute & Act with Knowledge (2023)

The PEAK Threat Hunting Framework incorporates experience gained and lessons learned during the last several years of threat hunting. It is a vendor- and tool-agnostic framework that was co-created with one of the original creators of the Sqrrl framework. The PEAK Framework:

a. Adds a third type of hunt, **Machine-Assisted Threat Hunting (M-ATH)**, which involves machine learning approaches like clustering, classification, or anomaly detection.

b. Provides even more detailed implementation guidance for each hunt type.

Even with these available frameworks, a successful hunt is dependent upon a clear set of goals and a well defined process that can be replicated over and over again. This will enable outcomes to be accurately measured and generation of detailed reports to incident responders and management produced.

Threat hunting has become vital to improving organizations' cyber security posture as well as identifying and mitigating existing weaknesses within their infrastructure. Despite threat hunting being resource intensive, more organizations are encouraged to adopt the practice and create a culture around the same.


# References
1. https://www.crowdstrike.com/cybersecurity-101/threat-hunting/

2. Practical Threat Intelligence and Data-Driven Threat Hunting by Valentina Costa-Gazcó

3. https://www.splunk.com/en_us/blog/learn/threat-hunting.html