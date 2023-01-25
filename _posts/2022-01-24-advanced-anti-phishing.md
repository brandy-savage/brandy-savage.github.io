---
layout: post
title:  "Advanced Anti-Phishing"
author: db
---
Hey all, today I want to talk about some advanced strategies to detect phishing campaigns targeting your brand. This will not be a talk on traditional email security controls, such as SPF, DKIM, and DMARC. Nor will this discus attachment, link or body analysis, rather we are going to look at techniques that will detect phishing before the first email is ever sent. The main topics we will cover are monitoring DNS registration, monitoring TLS certificate registrations, backdooring JavaScript to check where it's being run, monitoring referrer / access logs, and checking look-a-like sites programmatically. All in all, this creates some high fidelity, early signal on new phishing campaigns and threats targeting your brand. You can couple this with techniques to steal the source of said sites as they develop them for even more intelligence on the campaign. All of this will be based on open source solutions which are easy to implement yourself. This stuff can also replace several paid products that do similar services, such as RiskIQ. A quick note, both the DNS and TLS strategies require monitoring for key words. Often phishers will use key words that are very close, but not quite the same. It is advised to generate a list of these permutations around your original words to bolster your search list, using a tool such as dnstwist.

## Monitoring DNS Registrations
Here is an amazing open source tool to monitor new domain registrations for your target word lists. This uses zonefiles.io to collect the new registrations and saves matches in a database or can notify slack. This is amazing, but won't catch subdomains that mimic your brand, only top level domains. This is often a classic service people pay for to monitor their domains and brand, but you can easily implement it yourself for free. This tool can be used to alert standalone, or feed into a framework or pipeline for verifying such potential phishing domains.

## Monitoring TLS Transparency Logs
The idea here is pretty simple, when new phishing sites go to get valid TLS certificates for their phishing domains said registrars publish those logs known as Certificate Transparency Logs. That site describes the role of both Monitors, who collect all of the logs, and Auditors, who run searches against the logs. Thankfully, Cali Dog Security publish some awesome open source tools to both host your own monitor, or run your own auditor. They also run a monitoring service, certstream, so we can simply spin up an auditor and point it at their monitor to look for our specific key words. This will catch new certificates for subdomains, but not catch wild card domains, so its good to keep that limitation in mind. Cali Dog Security uses this as a paid anti-phishing service, but you can implement these tools yourself for free. Another solution, that is highlighted in the video below is streamingphish, but I have less experience with this personally.

## Monitoring Access Logs
You can also monitor your access logs for people loading resources such as JavaScript, CSS, or images off of your site. You can check the referrer headers here to see where potential phishing sites are, and you can automate this into some verification techniques to effectively operationalize your access logs. This is less invasive then the JavaScript route but may miss actors that clone the entire site, including all of its resources.

## Backdooring JavaScript
You can backdoor the JavaScript on your site to check which domain it is being loaded on to. If it isn't a domain you control you can have it beacon home, collecting environmental and victim information if you choose. I highly recommend obfuscating this backdoored JavaScript and also leaving a comment for anyone who deobfuscates said code. This will likely miss actors that recreate the site from scratch or audit the source for such backdoors. That said, this is really effective and you will often see actors running the code on their local and dev machines with this technique.

## Automatically Verifying Phishing Sites 
Finally, here is an epic open source tool to verify look-a-like sites with these cool DOM hashing techniques. Normally the techniques mentioned in this article will produce a fair number of false positives as people naturally use some of your key words for other purposes. Here you can use a technique to make sure your previous hits are actually a clone of your brand or site, verifying that someone is setting up a similar site to infringe on your domain in some way.

[![Building a Predictive Pipeline to Rapidly Detect Phishing Domains]
(https://img.youtube.com/vi/s5g7ij5EKoA/maxresdefault.jpg)]
(https://www.youtube.com/watch?v=s5g7ij5EKoA 
