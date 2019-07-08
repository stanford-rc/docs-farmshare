---
layout: page
title: Farmshare 2
permalink: /
---

# Welcome to Farmshare 2

<div style="float: left; width:20%; margin-right:20px">
     <img src="{{ site.baseurl }}/assets/img/logo/farmshare-red-pot.png">
</div><br><span>
FarmShare is Stanford's <a href="#eligibility">community computing environment</a>. It is intended for use in coursework and unsponsored research; it is not approved for use with <a href="#highRisk">high-risk data</a>, or for use in <a href="#sherlock">sponsored research</a>.
   </span>

<br>

## Connecting

FarmShare 2 uses SSH to make an encrypted connection from your computer to one of the cluster nodes.

If you want to connect to one of the login nodes, SSH to rice.stanford.edu using your SUNetID. You will be connected to whichever *rice*node is the least-loaded at the time. If you have an allocation on one of the compute nodes, you can use SSH to connect directly to the node where your job has started, but please use this for troubleshooting only. In either case, two-step authentication is part of the login process.

 - [Read more about connecting to FarmShare 2 with SSH](docs/connecting) 
 - [get FarmShare 2 host keys and fingerprints](docs/connecting-key)


## Using Software

FarmShare 2 has lots of free and commercial software available to use. We provide software that comes with Ubuntu, software that we package ourselves, and we also provide the capability for you to build and use software yourself!

 - [Read more about software on FarmShare 2](docs/software)


## Running Jobs

FarmShare 2 uses the [Slurm](http://slurm.schedmd.com/) job-submission system. This is the same system used elsewhere in SRCC (FarmShare 2 has its own installation, separate from the others). You can use Slurm to submit batch jobs (which run while your're away), start interactive jobs (which automatically connects you to a compute node), and request a general allocation of computing resources (so you can connect multiple times).

## Eligibility

FarmShare 2 is available to anyone who has a *full-service SUNetID*. A full-service SUNetID is one that has email service; if you can successfully get to [Stanford Webmail](https://webmail.stanford.edu), then you are eligible to use FarmShare 2 for academic work and small research jobs! If you do not already have a full-service SUNetID (maybe because you are a visiting researcher), you can get a sponsored full-service SUNetID. 

  - [Read more about SUNetID levels](https://uit.stanford.edu/service/accounts/sunetids)

Note that, in order to get a sponsored SUNetID, a monthly fee will be charged by University IT. Only people with spending authority may sponsor a SUNetID. Sponsorships can be obtained and paid for through [Sponsorship Manager](https://uit.stanford.edu/service/sponsorship/). Current rates are available from the [Sponsored Account Rates page](https://uit.stanford.edu/rates/sponsorship).

FarmShare 2 is meant for [low- or moderate-risk data](https://uit.stanford.edu/guide/riskclassifications), and is primarily intended for class work and for other general- and personal-use work (research and training). It is not meant for sponsored research (where you have a dedicated source of funding), and is *not* approved for handling high-risk data.

### CS Students

If you're a CSD (Stanford Computer Science Department) student, that you may want to use the Xenon or myth clusters. 

 - [Read more in CS's Computing Guide](http://cs.stanford.edu/computing-guide/overview).

### Using Sherlock for Sponsored and Departmental Research

If you are doing sponsored or departmental research, then FarmShare might not be the right place for you. Instead, if the data you are working with is all low-risk, then you should consider getting access to Sherlock! [The Sherlock web site](https://sherlock.stanford.edu) has more information about how to get access.

### High-Risk Computing

If you have high-risk data of any kind, please [let us know](mailto:research-computing-support@stanford.edu?subject=High-risk%20computing%20request&body=Hello!%0A%0AI%20would%20like%20to%20talk%20with%20someone%20from%20SRCC%20about%20doing%20research%20involving%20high-risk%20data.%0A%0A(Also%2C%20please%20say%20if%20your%20high-risk%20data%20is%20PHI!)), and we can help you figure something out!

## Getting Help

Most FarmShare 2 support is provided during business hours, either via email or during academic-year office hours.

For email support, send a message to [srcc-support@stanford.edu](mailto:research-computing-support@stanford.edu?subject=FarmShare%202%20(Add%20more%20detail%20here)&body=Please%20type%20your%20support%20request%20here). Make sure you have "FarmShare 2" somewhere in the subject line, and please be as detailed as possible with your request!

During the academic year (Autumn through Spring, excluding holidays and breaks), in-person support is available at SMACC (Stat, Math, Algorithmic and Computational Consulting) [office hours](http://stanford.edu/~rezab/smacc/), which take place weekly.

### Adding Capacity

FarmShare 2 does not have a dedicated funding source available to it, and we appreciate any contributions that people can make. For example:

* If you are using FarmShare 2 for a class, let your department chair know that you are using it.
* If you are a registered student group using FarmShare 2, let your faculty representative and/or ASSU know that you are using it.
* If you have spare funds available, [email us](mailto:srcc-support@stanford.edu?subject=FarmShare%202%20funds%20contribution&body=Hello!%0A%0AI%20have%20some%20spare%20funds%20available%2C%20and%20would%20be%20interested%20in...%0A%0Adoing%20an%20iJournals%20transfer%20to%20SRCC.%0Ahelp%20purchase%20FarmShare%202%20hardware%20using%20funds%20from%20my%20PTA.) and we can arrange either a iJournals transfer to our PTA, or a capital purchase from your PTA.
* If you already have hardware being supported by SRCC, we may be able to use that hardware once you are done with it.

We appreciate anything you can do to get the word out about FarmShare 2 and how awesome it is!

If you would like to contribute to the site or ask a question, please
[open an issue]({{ site.repo }}/issues)
