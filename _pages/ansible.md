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


<h3 id="Playbooks">Playbooks</h3>
Admin Scripts
{: style="color:black; font-size: 80%;"}
Main repository: <a href="https://github.com/Josh-Tracy/Admin-Scripts.git"> Admin-Scripts</a>
{: style="color:black; font-size: 80%;"}
<h4>Add users on RedHat Linux machines</h4>

<h4>Register machines to a RedHat Satellite Server</h4>
Define the machines in the host inventory that you want to registers. Modify the <b>activation_key,</b> <b>satellite_org_id,</b> <b>satellite_katello_rpm,</b> and <b>satellite_url</b> under group vars

<h3 id="Errors">Errors</h3>