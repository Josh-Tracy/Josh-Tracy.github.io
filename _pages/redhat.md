---
title: "RedHat Lessons Learned"
permalink: /redhat/
header:
  image: "/images/banner.png"
---
## RedHat Products Lessons Learned

### Table Of Contents
* <a href="#SSO"> Single Sign On </a>
  * <a href="#admin"> Initial Admin creation from CLI </a>
  * <a href="#logs"> SSO Logs </a>
* <a href="#Satellite"> Satellite </a>
* <a href="#IDM"> Identity Manager </a>
* <a href="#Openshift"> Openshift </a>
* <a href="#AnsibleTower"> Ansible Tower </a>
  * <a href="#AnsibleGit"> Configure Project Runs from GitLab </a>
  * <a href="#TowerErrors"> Tower Errors </a>

<h3 id="SSO">Single Sign ON</h3>
<h4 id="admin"> Create Initial Admin User using CLI </h4>
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

<h4 id="lofs">SSO Log Location</h4>
{: style="color:DodgerBlue;"}

* /opt/rh/rh-sso7/root/usr/share/keycloak/standalone/log/server.log
{: style="color:black; font-size: 80%;"}

* Reboot server
{: style="color:black; font-size: 80%;"}

<h3 id="Satellite">Satellite</h3>

<h3 id="IDM">IDM</h3>

<h3 id="Openshift">Openshift</h3>

<h3 id="AnsibleTower">Ansible Tower</h3>
<h4 id="AnsibleGit">Configure Project Runs from GitLab</h4>
{: style="color:DodgerBlue;"}
Configuring Ansible Tower to clone repos from a private GitLab server.

In Ansible Tower Settings -> Credentials
{: style="color:black; font-size: 80%;"}
{:refdef: style="text-align: center;"}
![My Image]({{ site.baseimg }}/images/towercreds.PNG)
{: refdef}
* SCM Private Key: If GitLab is using SSH Key authentication <b>(PREFFERED)</b> paste the private key to the public key stored in GitLab under User Settings -> SSH Keys. Paste under 
* Name: Giv it a name.
* Credential Type: "Source Control"
* All other can be blank. Save.
{: style="color:black; font-size: 80%;"}
In Ansible Tower go to Projects -> Add
{: style="color:black; font-size: 80%;"}
{:refdef: style="text-align: center;"}
![My Image]({{ site.baseimg }}/images/towerproject.PNG)
{: refdef}
* Name: Pick one
* Organization: Pick one
* SCM Type: GIT
* SCM Credential: The one you just made
* SCM URL: The URL of the repo you will clone from. If HTTPS:// is not an option or doesnt work, use SSH://<"user@repo"> like in the picture above.
* Save
{: style="color:black; font-size: 80%;"}
<h4 id="TowerErrors">Errors</h4>
{: style="color:DodgerBlue; font-size: 80%;"} 
"The Peers certificate issuer could not be recognized"
{: style="color:red; font-size: 80%;"}
SSL Is not configured on GitLab. If GitLab is using a self signed cert:
{: style="color:black; font-size: 80%;"}

* Copy the self signed cert from GitLab to:
* /etc/pki/ca-trust/source/anchors/
* run: <b>update-ca-trust extract</b>
{: style="color:black; font-size: 80%;"}

