---
title: "Ansible Lessons Learned"
permalink: /ansible/
header:
  image: "/images/banner.png"
---
## Ansible Lessons Learned

### Table of Contents

* <a href="#Playbooks"> Playbooks </a>
* <a href="#Errors"> Errors </a>
* <a href="#AnsibleVault"> Ansible Vault </a>


<h3 id="Playbooks">Playbooks</h3>
Admin Scripts
{: style="color:black; font-size: 80%;"}
Main repository: <a href="https://github.com/Josh-Tracy/Admin-Scripts.git"> Admin-Scripts</a>
{: style="color:black; font-size: 80%;"}
<h4>Add users on RedHat Linux machines</h4>
{: style="color:DodgerBlue;"}
Creates users, adds them to groups, and copys ssh key into authorized_keys
{: style="color:black; font-size: 80%;"}

Link to playbook: <a href="https://github.com/Josh-Tracy/Admin-Scripts/playbooks/add_users.yml"> add_users.yml</a>
{: style="color:black; font-size: 80%;"}

<h4>Register machines to a RedHat Satellite Server</h4>
{: style="color:DodgerBlue;"}
Define the machines in the host inventory that you want to register. Modify the activation_key, satellite_org_id, satellite_katello_rpm, and satellite_url under group vars.
{: style="color:black; font-size: 80%;"}

Link to playbook: <a href="https://github.com/Josh-Tracy/Admin-Scripts/playbooks/register.yml"> register.yml</a>
{: style="color:black; font-size: 80%;"}

<h3 id="Errors">Errors</h3>
Requested Data Type Primary not Available
{: style="color:DodgerBlue; font-size: 80%;"} 
<pre>
TASK [Install updates] *************************************************************************************************************************************************
fatal: [tower00.paas-govcloud.agilesof.org]: FAILED! => {"changed": false, "msg": "Error: requested datatype primary not available\n", "rc": 1, "results": []}
</pre>
{: style="color:gray; font-size: 70%;"}

FIX: Republish the meta-data for the content view subscribed to on the RedHat Satellite Server.
{: style="color:black; font-size: 80%;"} 

<h3 id="AnsibleVault">Ansible Vault</h3>