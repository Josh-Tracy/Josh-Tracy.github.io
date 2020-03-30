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
<span style="background-color: #9DFBA5">Ansible AWX is the opensource free version of RedHat's Ansible Tower. </span>
{: style="color:black; font-size: 80%;"}

#### STEP 1: Create Key, Certificate, and Key Store
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
{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/keystore.png)
{: refdef}
* Select <b>RSA</b> from the dropdown
{: style="color:black; font-size: 80%;"}
* Complete the fields as shown below. <b>(Upload your saml.key and smal.crt)</b>
{: style="color:black; font-size: 80%;"}
{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/rsa.png)
{: refdef}
* Click <b>Save</b>
{: style="color:black; font-size: 80%;"}

#### STEP 2: Configure Ansible Tower for SAML (SSO) Authentication
Follow these steps to configure Ansible Tower for Security Assertion Markup Language (SAML) single sign-on (SSO) authentication.
{: style="color:black; font-size: 80%;"}

*	Log in as the admin user on Ansible Tower and go to <b>Settings</b> > <b>Configure Tower</b> > <b>Authentication</b> > <b>SAML</b>
{: style="color:black; font-size: 80%;"}
* Complete the fields as shown. <b>NOTE:</b> Some cannot be edited.
{: style="color:black; font-size: 80%;"}
* <b>SAML ASSERTION CONSUMER SERVICE (ACS) URL:</b> <b>Cannot edit</b>
{: style="color:black; font-size: 80%;"}
* <b>SAML SERVICE PROVIDER METADATA URL:</b> <b>Cannot edit</b>; used to obtain tower’s SAML metadata using curl.
{: style="color:black; font-size: 80%;"}
* <b>SAML SERVICE PROVIDER ENTITY ID:</b> Enter the URL of the server. <b>Note:</b> Must match Client ID on SSO server.
{: style="color:black; font-size: 80%;"}
* <b>SAML PROVIDER PUBLIC CERTIFICATE:</b> Paste the <b>saml.crt</b>
{: style="color:black; font-size: 80%;"}
* <b>SAML SERVICE PROVIDER PRIVATE KEY:</b> Paste the <b>saml.key</b>
{: style="color:black; font-size: 80%;"}
* <b>SAML SERVICE PROVIDER ORGANIZATION INFO:</b>
{: style="color:black; font-size: 80%;"}

<pre>
{
 "en-US": {
  "url": "<<b>'URL OF TOWER SERVER'</b>>:<<b>'PORT IF NEEDED'</b>>",
  "displayname": "RHSSO",
  "name": "RHSSO"
 }
}
</pre>
{: style="color:black; font-size: 70%;"}
