---
title: "boto3 and botocore are required for this module error in Ansible on RHEL 7.6"
date: 2020-01-31
tags: [Networking, Automation, AWS, virtualenv, python, boto3, botocore, Ansible, Information Technology, RHEL 7]
#header:
#  image: "/images/Ansible_Cisco.png"
excerpt: "Networking, Automation, AWS, virtualenv, python, boto3, botocore, Ansible, Information Technology, RHEL 7"
---
### Fixing the error "boto3 and botocore are required for this module" when running an Ansible playbook on a RHEL 7 server

Today I was runng into issues on my RHEL 7 server when trying to deploy an Ansible playbook. After a couple minutes of consulting with the Stack Overflow gods, I decided to run a yum update on my server.
{: style="color:black; font-size: 80%;"}

I then proceeded to run my playbok <b>WHICH RAN PERFECTLY EVERYTIME</b> for the last week, only this time I was met with this output:
{: style="color:black; font-size: 80%;"}

<pre>
TASK [Provisioning in AWS using cloudformation template] *********************************
fatal: [localhost]: FAILED! => {"changed": false, "msg": "boto3 and botocore are required for this module"}
</pre>
{: style="color:gray; font-size: 70%;"}

I knew boto3 and botocore were installed, because running the below told me so:
{: style="color:black; font-size: 80%;"}

<pre>
pip freeze|grep boto

boto==2.49.0
boto3==1.10.49
botocore==1.13.49
</pre>
{: style="color:gray; font-size: 70%;"}

### The Fix

#### Virtualenv to the rescue!