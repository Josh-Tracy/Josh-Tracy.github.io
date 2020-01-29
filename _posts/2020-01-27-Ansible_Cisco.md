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
{: style="color:black; font-size: 80%;"}

In this post we will go over a simple configuration of a Cisco IOS device using Ansible running on a Ubuntu VM and a Cisco device running in GNS3.
{: style="color:black; font-size: 80%;"}

### Ansible and Networking

Due to Ansible's ability to push configuration changes using SSH, Ansible can be used on almost any networking device. Ansible has modules written for many existing network products such as Cisco, Juniper, Palo Alto, etc. Ansible's modules are pre-written unites of code designed to accomplish certain tasks within a playbook. Think of them as plugins.
{: style="color:black; font-size: 80%;"}

### Preparing Ansible to Configure a Cisco Router

Before we can begin we will have to set up our Ansible directory. The recommonded best practice for setting up the directory structure can be found in Ansible's docs: <a href="https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html">Best Practices</a> however for this post, our directory is much simpler. I prefer to set up my directory like the one below:
{: style="color:black; font-size: 80%;"}

<pre>
ansible_project/              # Top level ansible folder
    group_vars/               # Variables to be called from playbooks
    roles/                    # All roles
        role-a                # Role
        role-b                # Role
        ...
    Inventory/                # Folder for inventories
        hosts                 # Hosts to configure defined here
    playbooks/                # All playbooks below here
        plabook.yml           # Example playbook
    ansible.cfg               # Site specific config
</pre>
{: style="color:gray; font-size: 70%;"}

First we have to define our host that we want to configure in a hosts file. You can name it whatever you want. We define a group of hosts disgnated in brackets such as [cisco]. This allows us to call multiple hosts at once by defining one group in our playbook. We can define hosts by IP, by "Router1 ansible_host=192.168.56.130", or by hostname if DNS is configured. Example:
{: style="color:black; font-size: 80%;"}

<pre>
[cisco]                               # Host Group
192.168.56.130                        # Host defined by IP
Router1 ansible_host=192.168.56.130   # Defined by naming convention
router1.example.com                   # Define by hostname
</pre>
{: style="color:gray; font-size: 70%;"}


Ansible also recommends specifying your host groups connection and credential information in the host file in the form of [cisco:var] like below. Here we need to identify the information Ansible will use when attempting to connect to the device using SSH:
{: style="color:black; font-size: 80%;"}

<pre>
[cisco:vars]                     # Host group variables 
ansible_network_os=ios           # The type of Operating System
ansible_ssh_user=admin           # SSH Username
ansible_password=password        # SSH Password
ansible_become=yes               # Permissions upgrade 'yes' or 'no'
ansible_become_method=enable     # Enter enable mode
ansible_become_password=password # Enable password
</pre>
{: style="color:gray; font-size: 70%;"}

### Writing a Playbook to Configure a Device

Once our inventory is defined with the hosts we want to run Ansible against, we have to create a playbook comprised of tasks to complete.
For this example we will be configuring a banner on a Cisco IOS device.

To begin, I  create a banner_playbook.yml file under the playbooks directory
<pre>
---                                     #YAML files are designated by the 3 --- dashes
- name: Configure motd and banner       # Purpose of the playbook
  hosts: cisco                          # Define the group of [hosts] that exist in the inventory
  connection: network_cli               # Connection type- network_cli for IOS devices
  gather_facts: no    

  tasks:                                # Anything below this point are tasks to be run

# ios_banner module used to define banners and messages of the day on Cisco IOS devices
# Banner choices: login / motd / exec / incomin / sip-ppp
# text can be defined here or as a variable called from another place.
    - name: CONFIGURE LOGIN BANNER
      ios_banner:                                 # The name of the module to use
        banner: login                             # The type of banner such as login or exec
        text: |                                   # The text to be used for the banner
          Warning: Authorized employees only. 
          All others will be in trouble.
        state: present                            # Current existential state of the banner

# iosxr_banner module used for IOSXR Cisco Devices
# Banner choices: login / motd
    - name: configure the login banner
      iosxr_banner:
        banner: login
        text: |
          this is my login banner
          that contains a multiline
          string
        state: present

# nxos_banner module for Cisco NXOS devices
# Banner choices: exce / motd
    - name: configure the exec banner
      nxos_banner:
        banner: exec
        text: |
          this is my exec banner
          that contains a multiline
          string
        state: present
</pre>
{: style="color:gray; font-size: 70%;"}
