---
title: "Configuring Ansible Tower/AWX with RedHat SSO / Keycloak"
date: 2020-03-30
tags: [RedHat, Ansible, Tower, AWX, Keycloak, SSO, OpenID, SAML]
header:
  image: "/images/ospf_banner.png"
excerpt: "RedHat, Ansible, Tower, AWX, Keycloak, SSO, OpenID, SAML"
---
### Configuring Ansible Tower to use RedHat Single Sign on

<span style="background-color: #9DFBA5">Keycloak is the upstream project for RedHat SSO. When troubleshooting look at keycloak documentation as well</span>
{: style="color:black; font-size: 80%;"}
Ansible AWX is the opensource free version of RedHat's Ansible Tower. 
{: style="color:black; font-size: 80%;"}

#### Create Key, Certificate, and Key Store
Follow these steps to create the key, certificate and key store to enable Ansible Tower SSO integration.
{: style="color:black; font-size: 80%;"} 
*	If you do not possess an x509 cert, enter the following to create one:
{: style="color:black; font-size: 80%;"}
<pre>openssl req -new -x509 -days 365 -nodes -out saml.crt -keyout saml.key</pre>
{: style="color:black; font-size: 80%;"}
*	Save <b>saml.key</b> in a notepad
{: style="color:black; font-size: 80%;"}
*	Save <b>saml.crt</b> in a notepad
{: style="color:black; font-size: 80%;"}
*	Log in to the RedHat SSO GUI as an admin
{: style="color:black; font-size: 80%;"}
*	Navigate to <b>your realm</b> -> <b>Keys</b> -> <b>Providers</b>, then click <b>Add keystore</b> as shown below
{: style="color:black; font-size: 80%;"}
