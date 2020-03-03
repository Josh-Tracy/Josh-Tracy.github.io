---
title: "RedHat Lessons Learned"
permalink: /redhat/
header:
  image: "/images/banner.png"
---
## RedHat Products Lessons Learned

### Table Of Contents
* <a href="#SSO"> Single Sign On </a>
* <a href="#Satellite"> Satellite </a>
* <a href="#IDM"> Identity Manager </a>
* <a href="#Openshift"> Openshift </a>
* <a href="#AnsibleTower"> Ansible Tower </a>

<h3 id="SSO">Single Sign ON</h3>
<h4> Create Initial Admin User using CLI </h4>
{: style="color:DodgerBlue;"}

* Navigate to keycloak-add-user script:
{: style="color:black; font-size: 80%;"}
<pre>
/opt/rh/rh-sso7/root/usr/share/keycloak/standalone/configuration/keycloak-add-user.sh
</pre>
{: style="color:gray; font-size: 70%;"}

* Run:
{: style="color:black; font-size: 80%;"}

<pre>
./add-user-keycloak.sh -r master -u <"username"> -p <"password">
</pre>
{: style="color:gray; font-size: 70%;"}

<h4>SSO Log Location</h4>
{: style="color:DodgerBlue;"}

* /opt/rh/rh-sso7/root/usr/share/keycloak/standalone/log/server.log
{: style="color:black; font-size: 80%;"}

* Reboot server
{: style="color:black; font-size: 80%;"}

<h3 id="Satellite">Satellite</h3>

<h3 id="IDM">IDM</h3>

<h3 id="Openshift">Openshift</h3>

<h3 id="AnsibleTower">Ansible Tower</h3>
<h4>Configure Project Runs from GitLab</h4>
{: style="color:DodgerBlue;"}
Configuring Ansible Tower to clone repos from a private GitLab server.

1. In Ansible Tower Settings -> Credentials
{: style="color:black; font-size: 80%;"}
{:refdef: style="text-align: center;"}
![My Image]({{ site.baseimg }}/images/towercreds.PNG)
{: refdef}
* If GitLab is using SSH Key authentication <b>(PREFFERED)</b> paste the private key to the public key stored in GitLab under User Settings -> SSH Keys.
{: style="color:black; font-size: 80%;"}
   * Name
   * Credential Type = "Source Control"
   * All other can be blank. Save
  {: style="color:black; font-size: 80%;"}
