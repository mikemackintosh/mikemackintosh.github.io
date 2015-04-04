---
layout: post
title: "Running A One-Man Security Team"
description: ""
category: Security
tags: security limited-resources informational netsec infosec syssec buttsec appsec
redirect_from:
 - /security/2015/02/28/one-man-security-teams/
---


### Introduction

Yes. And there's nothing you can do about it. Many companies are not willing to invest in security until they are hit with a massive and expensive breach. Others simply don't see the benefit after seeing companies like Anthem, who has over 200 security professionals on payroll get hit with the largest compromise in web history.

As a one-man security team, I know you have a heck of a lot of responsibilities, with very minimal resources to accomplish your goals. The biggest downfall to one-man security teams is the fact that you don't get to innovate; you're too busy fighting fires on a daily basis and cleaning up other peoples messes. As a result, and to allow you to focus your skillset correctly, you need to define your perimeter.


### Establishing a Perimeter

As a one-man security team, you need to shrink your responsibility as much as possible without sacrificing the integrity of the brand or company. This can simply be done by separating what a security engineer can do from should do. Failure to do so will result in burnout, lack of focus, and lack of motivation.

Here are a few key areas which are often contested in terms of ownership:


#### Application Security

A one-man security engineering team **should:**

  - Work with vendors like [SignalSciences](https://www.signalsciences.com/), [SecurityScorecard](https://www.securityscorecard.io/) and [MXTools](http://www.mxtools.com/) to increase security visibility and decrease exposed footprints. 

    * SignalSciences helps you to automate the defense of your web servers by identifying attacks and responding automatically. 

    * SecurityScorecard helps you assess the security posture of yourself, your brands and your vendors using publicly available information.
    
    * MXTools helps you reduce your exposed footprint, by allow you to block bad networks and identify spammers, malware domains and more.  


  - Assess and grade the risk and impact of an identified security bug on the web application. 

  - Create a proof of concept to automate the exploit of the identified security vulnerability.

  - Teach, Train or Mentor developers and QA folks on how to not suck at security. It doesn't matter what language you use, how long you've been developing, or what level of education a developer has, they WILL introduce a security bug. 

  - Know how to identify successful and unsuccessful brute-force attacks for employees and customers.


A one-man security team **should NOT**:

  - Be told no, they can't use a vendor because security doesn't have a budget. If the company cares about investing in security, but not willing to commit to headcount, they'll be willing to sign on new vendors.

  - Perform code review. Senior developers should be able to do this. 

  - Manage an application bug bounty program. This should be done by QA, because the bugs that have been found should have already been identified by them before going public. Every time a Bug Bounty comes it, it should be a ding on QA.

  - Monitor apache and nginx logs for awkward behavior and 500's. 

  - Write or push public security patches to code base for which an active development team is responsible for. An entire development team already exists for this type of work.

  - Send customer emails when accounts are breached. This is legal and customer service. Legal needs to let each state know how many accounts were compromised, while customer service is best equipped to work with customers in these situations.


#### System Security

A one-man security engineering team **should:**
  
  - Setup visibility and alerting on successful SSH logins, Console Session Longevity, etc

  - Help define Firewall Rule risk-assessments, and assist in creation of network segmentation.

  - Be the first to know and create an attack plan for incidents like GHOST (glibc), POODLE and etc.

A one-man security team **should NOT**:

  - Be the one logging out all console sessions over 24 hours old. This is an operational workflow problem that needs to be solved with good 'ol ass-whoopin. **It is _NEVER_ ok to not log out of a root console.**
  
  - Manage firewall rule bases

  - Log into every server and patch glibc. This is the responsibility of the operations team.

  - Create their own logging infrastructure. There should already be in place (to be PCI compliant), and you need to piggy back off this.


#### Corporate Security

A one-man security engineering team **should:**

  - Create alerting on suspicious network activity

  - Know how to identify the last time a user logged into the system, and where it was logged in from.

  - Create automated tooling to manage off-boarding auditing. An ex-employee is should not be treated differently than King Gawaaali from Nirobi who has a $10mil inheritance waiting for you on the other side of a link.

A one-man security team **should NOT**:

  - Be solely any SSO or authentication related resources (AD, SSO, SAML, RADIUS, LDAP, etc).

  - Reset employee passwords when they appear on dump sites. Security should provide details to IT, who should then work with the end-user.

  - Enforce password complexity and retention policies. This is an IT, and Legal thing. If security is involved, it should be to define the best 2FA system available.

 

#### Virus Protection and Malware

A one-man security engineering team **should:**

  - Understand the current threats (Zeus, Dyreza, etc) and create a way to monitor and educate employees on phishing attacks and malware.

  - Setup a Snort instance, or get a company like [Vectra Networks](http://www.vectranetworks.com/) (costly, but cheaper than headcount) to monitor traffic in the data center and office.

  - Know what VirusTotal is, and what Reflective DLL Injection is. If your one man security team doesn't know, do him a favor, have him stop fighting viruses and allow him to make one.

  - Provide input on the threat and risk of security patches.

A one-man security engineering team **should not**:

  - EVER, EVER, EVER remove a virus from an infected machine. There is an IT department to do this. If there is no IT department, find a company that isn't about to go flop.

  - Deploy security patches to workstations and laptops. This again, is the responsibility of the IT department. 


#### Acceptable-Use and BYOD Policies

A one-man security engineering team **should:**

  - Know what tools are available to them under Google Apps or Exchange ActiveSync. Know that you have the ability to remotely wipe a device of a disgruntled and opportunistic employee. If you don't have this ability, create it.

  - Work with IT to deploy a secure and guest network, which are isolated from each other. 


A one-man security engineering team **should not**:

  - Waste their time looking into what percentage of traffic is Facebook, Twitter and Pandora for management.
  
  - Be filing abuse complaints or take-down requests on behalf of the organization. This should be done by legal. Litigation takes much time, and that's something a one-many security team does not have.


#### PCI/SOX Compliance

A one-man security engineering team **should:**

  - Work with operations and IT teams to ensure different aspects of compliance are upheld and enforced, like making sure all data center entrances are badged, audited and accompanied by floor-to-ceiling walls.

  - Collaborate with development teams to ensure all credentials and credit card data is encrypted from browser to database (B2D).

  - Perform weekly/bi-weekly scans against all publicly accessible and internally unsecure real-estate. This includes deploying a Qualys, Nessus, Arachni, SQLMap, Masscan, NMAP scan against all assets. This isn't compliance related, but more of vulnerability management. 

A one-man security engineering team **should not:**

  - Handle, attest or submit PCI compliance, unless they are sponsored by their company as an ISA (Internal Security Assessor) as required by PCI. Compliance != security, it is an operations, legal and accounting problem. Don't side-track a security engineer with this busy-work.

  - Be responsible for overseeing the stop gates of SOX compliance release workflows. Your development team executives need to have their balls on the chopping block at some point, and this is it.

  
#### SSL Certificates and PKI

A one-man security engineering team **should:**

  - Validate the CA chain to ensure so MITM attacks are possible. They should also ensure that all certs are SHA-2 and at least signed 2048-bits of RSA. 

  - Deploy Perfect Forward Secrecy, to ensure something like heartbleed can't leak public and private key exchanges in the future.
  
  - Come up with a plan for executives who travel to technology-aggressive areas. For example, they should formalize a plan to replace executives business laptops with burner Chrome notebooks which won't get infected and leak confidential information.


A one-man security engineering team **should not:**

  - Be responsible for generating, upgrading and pushing SSL certificates. If your operations team doesn't understand how to roll out a new SSL cert, including creating it, and getting it signed by a trusted CA, then you have bigger problems to worry about.

### Exposing Your Visibility

The other hurdle a one-man security team will have is exposing their impact. In short, if you are never hacked, you are doing your job. If you are hacked, well, that just sucks cause we know you did all humanly possible of you. That leads us to your own audit trail.

#### Log Everything You Do

Who cares what it is. If you did something log it. If you had to talk to a dev about a best practice, log it. If you told QA they missed a vulnerability, log it. If you see something weird popping up in a dashboard somewhere, log it. 

If you can account for every minute of your day, you can easily show management the impact you bring, the checks and balances you provide, and that you probably work close to 80 hours a week and they under value you. Using something like Trello or Jira is a great way to get this started.

#### Create Dashboards

Hopefully you're in a position where you can easily create dashboards from data your vendors provide or from internal data.

Here are a few simple ideas of metrics to track:

##### Tracking Internal Server Errors and 500's (SQL Injections)
For example, log the number of 500's returned in a web application, before and after giving a talk on the importance of preparing SQL statements. If developers actually paid attention, you will see a huge decrease in blind sql injections.

##### Tracking XSS and Hacker Talk
Using Google Search API's and dorks, run daily queries on search results that return XSS and other hacker talk across the web. 

##### Create a Twitter Bot
Using Twitter bots is a great way to see what people are saying about your site. People are pride happy. If they find a vuln on your site, best believe it's on twitter and retweeted before you even know. Use this to show the time between first tweet to patching. Record this time and stick it on a dashboard.

##### Security Bug Bounty
Record the `days since` a critical or high severity bug was identified by an app scanner.

##### Monitor Failed Login Attempts via SSH
This one is awesome. You can create a quick webhook in `.sshrc` on a node that will send notification on successful login. 

##### Brute Force and Dictionary Attacks
Use this to identify how many abusers you've identified, to graphing of blocked requests after you ban abusive IP's. This one is always going to be awesome, especially during a huge attack.

##### Total Time to Patch (TTP)
This one is huge. Every time an internet killing vulnerability appears, you can use this towards your advantage. Record the time that you are alerted on the vulnerability, to the time a patch is released, to the time you work with operations to schedule an upgrade, to the time patching has been completed. Use this to show management where communication or responsibilities needs to be corrected.


### Conclusion

I know, it can suck. You are in a position where it is difficult to have other teams take responsibility, every battle is uphill, and many of your bright ideas will get shot down thanks to lack of funding. But remember, you're now going to be immersed in a world of hackers, trackers, malware and botting. This is the best place to learn and grow, both professionally and technically, before moving on to your next adventure.

