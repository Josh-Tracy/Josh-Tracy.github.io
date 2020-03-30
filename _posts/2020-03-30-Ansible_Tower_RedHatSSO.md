---
title: "Configuring Ansible Tower/AWX with RedHat SSO / Keycloak"
date: 2020-03-30
tags: [RedHat, Ansible, Tower, AWX, Keycloak, SSO, OpenID, SAML]
header:
  image: "/images/ospf_banner.png"
excerpt: "RedHat, Ansible, Tower, AWX, Keycloak, SSO, OpenID, SAML"
---
### Configuring Ansible Tower to use RedHat Single Sign on

Keycloak is the upstream project for RedHat SSO. When troubleshooting look at keycloak documentation as well
{: style="color:black; font-size: 80%;"}
Ansible AWX is the opensource free version of RedHat's Ansible Tower. 
{: style="color:black; font-size: 80%;"}

## Create Ket, Certificate, and Key Store
Follow these steps to create the key, certificate and key store to enable Ansible Tower SSO integration. 
1.	If you do not possess an x509 cert, enter the following to create one:
openssl req -new -x509 -days 365 -nodes -out saml.crt -keyout saml.key
2.	Save <b>saml.key</b> in a notepad
3.	Save saml.crt in a notepad
4.	Log in to the SSO GUI as an admin
5.	Navigate to the existing OCP realm -> Keys -> Providers, then click Add keystore as shown below
