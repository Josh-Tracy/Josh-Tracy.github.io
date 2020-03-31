---
title: "Configuring Ansible Tower/AWX with RedHat SSO / Keycloak"
date: 2020-03-30
tags: [RedHat, Ansible, Tower, AWX, Keycloak, SSO, OpenID, SAML]
header:
  image: "/images/Tower_sso.png"
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
  "url": "<<b>URL OF TOWER SERVER</b>>:<<b>PORT IF NEEDED</b>>",
  "displayname": "RHSSO",
  "name": "RHSSO"
 }
}
</pre>
{: style="color:black; font-size: 70%;"}

* <b>SAML SERVICE PROVIDER TECHNICAL CONTACT:</b>
{: style="color:black; font-size: 80%;"}

<pre>
{
 "givenName": "<<b>YOUR NAME</b>>",
 "emailAddress": "<<b>YOUR EMAIL</b>>"
}
</pre>
{: style="color:black; font-size: 70%;"}

* <b>SAML SERVICE PROVIDER SUPPORT CONTACT:</b>
{: style="color:black; font-size: 80%;"}

<pre>
{
 "givenName": "<<b>YOUR NAME</b>>",
 "emailAddress": "<<b>YOUR EMAIL</b>>"
}
</pre>
{: style="color:black; font-size: 70%;"}

* <b>SAML ENABLED IDENTITY PROVIDERS:</b>
{: style="color:black; font-size: 80%;"}

<pre>
{
 "RHSSO": {
  "attr_last_name": "last_name",
  "attr_username": "username",
  "entity_id": "<<b>SSO SERVER URL</b>>:<<b>PORT IF NEEDED</b>>/auth/realms/<b>YOUR REALM</b>",
  "attr_user_permanent_id": "name_id",
  "url": "<<b>SSO SERVER URL</b>>:<<b>PORT IF NEEDED</b>>/auth/realms/<b>YOUR REALM</b>/protocol/saml",
  "attr_email": "email",
  "x509cert":  “ <<b>PASTE THE X509 PUBLIC CERT HERE IN ONE, NON-BREAKING LINE</b>>"
  "attr_first_name": "first_name"
 }
}
</pre>
{: style="color:black; font-size: 70%;"}

<span style="background-color: #9DFBA5"><b>Note:</b> The x509cert can be found by logging in to the SSO server and navigating to your realm’s <b>realm settings</b> -> <b>keys</b> -> and clicking <b>Certificate</b> under Public keys for the key store you created earlier.</span>
{: style="color:black; font-size: 80%;"}

* Click <b>Save</b>
{: style="color:black; font-size: 80%;"}

#### Step 3: Create the Tower Client in RedHat SSO
Complete the following steps to create the Tower Client in SSO.
{: style="color:black; font-size: 80%;"}

* In SSO, click on <b>your realm</b> -> <b>Clients</b> -> <b>Create</b>
{: style="color:black; font-size: 80%;"}
* Choose <b>Select File</b>
{: style="color:black; font-size: 80%;"}
* SSH into the tower server, or from a CLI that can curl the tower server, and run the following command: 
{: style="color:black; font-size: 80%;"}
<pre>
curl -L -k https://<<b>YOUR TOWER URL</b>>/sso/metadata/saml/
</pre>
{: style="color:black; font-size: 70%;"}
* Save the output into <b>tower_saml.xml</b>
{: style="color:black; font-size: 80%;"}
* Upload the <b>tower_saml.xml</b>
{: style="color:black; font-size: 80%;"}

<span style="background-color: #9DFBA5"><b>Note:</b> Ensure the client ID matches <b>SAML Service Provider Entity ID</b> from the Tower SAML page earlier. They both should be the URL of the tower server: <b>https://tower.example.org</b></span>
{: style="color:black; font-size: 80%;"}

* In the Client Protocol box, select SAML
{: style="color:black; font-size: 80%;"}
* Click <b>Save</b>
{: style="color:black; font-size: 80%;"}
* Apply the following settings:
{: style="color:black; font-size: 80%;"}
<pre>
Client ID: <b>URL of Tower Server</b>
Enabled: ON
Consent Required: OFF
Client Protocol: SAML
Include AuthnStatement: ON
Include OneTimeUseCondition: OFF
Sign Documents: ON
Optmize REDIRECT signing key lookup: OFF
Sign Assertations: ON
Signature Algo: PICK
SAML SIGNATURE KEY NAME: PICK
Canonicalization Method: Exclusive
Encrypt Assertations: OFF
Client Signature required: OFF
Force POST Binding: ON
Force Channel Logout: ON
Force NAme ID Format: OFF
Name ID Format: username
Root URL: <b>URL OF TOWER SERVER</b>
Valid Redirect URIs: <b>URL of TOWER SERVER</b>/sso/complete/saml
IDP Initiated SSO URL NAME: <b>URL  OF TOWER SERVER</b>
</pre>
{: style="color:black; font-size: 70%;"}

* Click <b>Save</b>
{: style="color:black; font-size: 80%;"}

#### Step 4: Configure Mappers
Complete the following steps to create and configure mappers on Ansible Tower’s RHSSO client:
{: style="color:black; font-size: 80%;"}

* In the tower client settings, select <b>Mappers</b> tab
{: style="color:black; font-size: 80%;"}
* Click <b>Create</b> and add the 5 mappers show in the following figures
{: style="color:black; font-size: 80%;"}

<span style="background-color: #9DFBA5"><b>Note:</b> firstName and lastName are case sensitive</span>
{: style="color:black; font-size: 80%;"}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/username.png)
{: refdef}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/lastname.png)
{: refdef}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/email.png)
{: refdef}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/userid.png)
{: refdef}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/firstname.png)
{: refdef}

If done properly, the log-in screen for tower should show “SIGN IN WITH” and an S symbol, as shown in the figure below. This will redirect you to SSO sign-in.
{: style="color:black; font-size: 80%;"}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/signinwith.png)
{: refdef}

### Troubleshooting
If you run into issues during the configuration check here first.
{: style="color:black; font-size: 80%;"}

#### Logs for SSO are found on the SSO server at:
<pre><b>/opt/rh/rh-sso7/root/usr/share/keycloak/standalone/log/server.log</b></pre>
{: style="color:black; font-size: 70%;"}

<b>Error:</b> When uploading .xml metadata files:
{: style="color:black; font-size: 80%;"}

{:refdef: style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}
![My Image]({{ site.baseimg }}/images/error.png)
{: refdef}

<b>Fix:</b> Check the log in the above location. There is likely a syntax error in the .xml file.
{: style="color:black; font-size: 80%;"}

<b>Error:</b> “Found an Attribute element with duplicated Name” when attempting to log in with SSO.
{: style="color:black; font-size: 80%;"}
<b>Fix:</b> Navigate to <b>(Your realm)</b> -> <b>Client Scopes</b> -> <b>role_list</b> (saml) -> <b>Mappers tab</b> -> <b>role list</b>, then turn on <b>Single Role Attribute</b>.
{: style="color:black; font-size: 80%;"}

<b>Error:</b> “Signature validation failed” when logging in to tower with SSO.
{: style="color:black; font-size: 80%;"}
<b>Fix:</b> Check whether the x509cert is correct in Ansible Tower’s SAML settings under SAML ENABLED IDENTITY PROVIDERS. After ensuring the x509cert is valid, enter it on one line without spaces.
{: style="color:black; font-size: 80%;"}

<b>Error:</b> “Invalid Signature” when trying to log in with SSO.
{: style="color:black; font-size: 80%;"}
<b>Fix:</b> Turn off <b>Client Signature Required</b> under the Tower client settings tab in SSO.
{: style="color:black; font-size: 80%;"}

<b>Error:</b> “Sign in with SSO” option not showing up on Tower log-in screen.
{: style="color:black; font-size: 80%;"}
<b>Fix:</b> You must complete the <b>SAML ENABLED IDENTITY PROVIDERS</b> section of the tower SAML setup page for this to appear. 
{: style="color:black; font-size: 80%;"}



