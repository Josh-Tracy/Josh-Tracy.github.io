---
title: "Auotmating Cisco IOS devices with Ansible"
date: 2020-01-27
tags: [Networking, Automation, Cisco, IOS, Information Technology]
header:
  image:
excerpt: "Networking, Automation, Information Technology"
mathjax: "true"
---

### What is Ansible?

Ansible is an IT automation software used to simplfy routine tasks such as provisioning, configuration, app deployment, security automation, and much more. The benefits of Ansible are:
{: style="color:black; font-size: 80%;"}

* Ease of use due to playbooks written in YAML language
* It is Agentless. It uses the SSH protocol to push to your client devices
* Ansible is idempotent -- only accomplish what needs to be done, and don't change what doesn't

In this post we will go over a simple configuration of a Cisco IOS device using Ansible running on a Ubuntu VM and a Cisco device running in GNS3.
{: style="color:black; font-size: 80%; text-align: center";}

### Ansible and Networking

Due to Ansible's ability to push configuration changes using SSH, Ansible can be used on almost any networking device. Ansible has modules written for many existing network products such as Cisco, Juniper, Palo Alto, etc. Ansible's modules are pre-written unites of code designed to accomplish certain tasks within a playbook. Think of them as plugins.
{: style="color:black; font-size: 80%;"}

### Preparing Ansible to Configure a Cisco Router




