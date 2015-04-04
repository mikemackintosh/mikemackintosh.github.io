---
layout: post
title: Building a Security Team in 2015
category: Security
tags: 2015-security trends management
redirect_from:
 - /security/2015/01/01/building-a-security-team-in-2015/
---

### Introduction
> It is late, and I'm tired, but wanted to jot this down before I fell asleep. Revisions and corrections to follow.

In my professional experience, compliance is not security. So, clearly, being compliant is not being secure. There are aspects of compliance which increase security, but they are more in the realm of operations and work flows (think PCI and SOX). What is being secure, is understanding what's happening on your network, within your apps or on your servers, and mission critical files being accessed. Visibility is not everything, yet it is everything at the same time. Without having visibility, you don't know what to secure. You don't know what's being targeted, and you sure as hell don't know what's already been pwned. Visibility also means expressing your impact to executive teams and stakeholders.

In 2014, we saw several large projects riddled with vulnerabilities, exploits floating across in HTTP headers, and fear of plugging USB and Phones into host devices. 

In 2015, we will likely see the same thing, but to pro-actively defend against it, we need a new approach to security.

## The Old Model (?)

Security used to easily be defined as:

  - AppSec, the hardening and testing of applications
  - InfoSec, the securing of information, credentials, account details and more
  - NetSec, the control over ones network

This worked great for the most part. It defined responsibilities to specific security teams. But the problem with this is there is no clear involvement to preventing these problems, or even detecting them.

## The New Model (HSF/ Hackintosh Security Framework)

The new security model I will be implementing follows the following general idea: Justify headcount and prove justification.

It consists of three parts:

  - DefSec
  - OpSec
  - SecEng

You can learn more about these below.

#### DefSec

DefSec simply outlines and explains Defensive Security. This includes vulnerability management, which most organizations outright ignore. This also falls under the realm of patch management, intrusion detection and participates in incident response. An engineer on the Defense team would also need to be fluent with firewall rules and networking. The justification for this is based on the premise of detecting, and preventing pivot and shrinking attack surface during a compromise or attack.

**Examples:** 

  - Some vulnerable piece of software allows a binary to get dropped onto a box. That box then recons and pivots to hosts in the adjacent vlan. A single team should be dedicated in following this, while not requiring input or resources from other non-security teams. The importance here is time to exposure. Requiring input and resources from other teams will flat out destroy MTTR.

  - This team is responsible for patching a hole during an incident rather than investigate the impact or digging through logs to see what might have been the end result.


#### OpSec

OpSec should be comprised of Operational centric personal, focused on maintaining, created and life-cylcing security tools, systems and vendors. This is extremely critical for owning the internal relationships with infrastructure and executives. Coupled in with their responsibilities is the reporting of metrics. Without metrics, no security team can be justified, because until the company gets owned, no executive would understand the need and impact a security team has. They would also be responsible for maintaining the Security Team dashboards and executive digests from the other security teams. This is not a heavily security focused position except for the fact they are also responsible for creating alerting, pages, monitoring of security events. These team members must like numbers and data mining. 

**Examples:** 

  - This team would be responsible for running the hardware that Snort is installed on or working with a vendor used for network-based intrusion detection.

  - The executive team thinks the security team is sitting around and drinking beer all day. This is a wild misconception which the OpSec team must respond to with work digests, pictures from dashboards and graphs of metrics.

  - During an incident or attack, this team would be called upon to mine data out of access logs and correlate it to identify malicious traffic.


#### SecEng

Security Engineering, or SecEng, (yes, sounds like sucking), works closely with developers and QA. Their responsibility is to not create a patch for software and applications, but to help development understand the vulnerability. This team would be responsible for creating custom penetration tooling oiled to the gears of homegrown code. This is a very hands on developer style role. They participate in bug bounty and responsible disclosure programs by validating reports and reverse-engineering findings. They should care about PCI scan results, but care more about the integrity of the brand. They are involved with risk assessments, sprint and rock planning (if you're in an agile environment) and 

**Examples:**

  - During quarterly PCI scans, 3 XSS and an open-redirect vulnerability were discovered. They tweak their tooling to detect the vulnerability quicker, but also work with developers to understand the problem so the developers don't make the same mistake again. They also work with QA in understanding the vulnerability and how to create a Unit Test. In addition to the previous, they would also assess the risk of the XSS (customer impact, brand impact) and work with teams to ensure they are resolved in order of importance.

  - This team is responsible for assessing impact of an event and the fallout which may occur. If the attack was against some specific piece of code on the production network, they reverse engineer it to learn and evolve. They work closely with legal and send off 'your password sucked and you got pwned, not our fault' emails to customers if needed.

## In Conclusion

2015 is going to be interesting as hell. Yesteryear left us with a whole lot of lessons to learn from. If your company wants to stay out of the news, you need to learn and adapt to the ever changing world of security. 

