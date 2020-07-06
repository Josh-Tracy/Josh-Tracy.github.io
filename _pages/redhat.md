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
  * <a href="#manadmin"> Access managment console at port 9990 </a>
  * <a href="#logs"> SSO Logs </a>
* <a href="#Satellite"> Satellite </a>
* <a href="#IDM"> Identity Manager </a>
* <a href="#Openshift"> Openshift </a>
  * <a href="#Exited"> Restart Exited Containers </a>
  * <a href="#Restart"> Restart Master Services </a>
* <a href="#AnsibleTower"> Ansible Tower </a>
  * <a href="#AnsibleGit"> Configure Project Runs from GitLab </a>
  * <a href="#TowerErrors"> Tower Errors </a>
* <a href="#Quay">Quay</a>
  * <a href="#QuayS3"> Quay S3 Registry Settings </a>
  * <a href="#QuaySSO"> Quay SSO Configuration </a>

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

<h4 id="manadmin"> Access Management Console at port 9990</h4>
{: style="color:DodgerBlue;"}

* /opt/rh/rh-sso7/root/usr/share/keycloak/bin/add-user.sh
{: style="color:black; font-size: 80%;"}

* follow the prompts choosing (a) for management user, this adds the user to <b>/opt/rh/rh-sso7/root/usr/share/keycloak/standalone/configuration/mgmt-users.properties</b>
{: style="color:black; font-size: 80%;"}

* Ensure you run standalone.sh using <b> -bmanagement 0.0.0.0 </b> in the command.


<h4 id="logs">SSO Log Location</h4>
{: style="color:DodgerBlue;"}

* /opt/rh/rh-sso7/root/usr/share/keycloak/standalone/log/server.log
{: style="color:black; font-size: 80%;"}

* Reboot server
{: style="color:black; font-size: 80%;"}

<h3 id="Satellite">Satellite</h3>

<h3 id="IDM">IDM</h3>

<h4> List Users</h4>
{: style="color:DodgerBlue;"}

* kinit admin
{: style="color:black; font-size: 80%;"}

* ipa user-find --all ( or single user)
{: style="color:black; font-size: 80%;"}

<h3 id="Openshift">Openshift</h3>

<h4 id="Exited">Find "Exited" containers and restart</h4>
{: style="color:DodgerBlue;"}

docker ps -a | grep Exited | awk '{print $1}' | xargs -L1 docker restart
{: style="color:black; font-size: 80%;"}

<h4 id="Restart">Restart Master Services</h4>
{: style="color:DodgerBlue;"}

* /usr/local/bin/master-restart api
* /usr/local/bin/master-restart controller
{: style="color:black; font-size: 80%;"}


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

<h3 id="Quay">Quay</h3>

<h4 id="QuayS3">S3 Registry Settings</h4>
{: style="color:DodgerBlue; font-size: 80%;"} 

* Storage engine: Amazon S3
{: style="color:black; font-size: 80%;"}
* S3 Bucket: Bucket name
{: style="color:black; font-size: 80%;"}
* Storage Directory: /datastorage/registry
{: style="color:black; font-size: 80%;"}
* AWS Access key: Add your key
{: style="color:black; font-size: 80%;"}
* AWS Secret Key: Add your Key 
{: style="color:black; font-size: 80%;"}
* S3 Host (Optional): s3.<b>your_region</b>.amazonaws.com
{: style="color:black; font-size: 80%;"}

<h4 id="QuaySSO">SSO Config for Quay</h4>
{: style="color:DodgerBlue; font-size: 80%;"}

Place the following in /quay/config/config.yaml
{: style="color:black; font-size: 80%;"}

<pre>
SSO_LOGIN_CONFIG: {CLIENT_ID: quay, CLIENT_SECRET: xxxxxxxxxxxxxxxxxxxxxxxxxxxxx,
  OIDC_SERVER: 'https://URL_OF_SSO_Server/auth/realms/REALMNAME/', SERVICE_NAME: SSO}
</pre>
{: style="color:black; font-size: 70%;"}

Also add the CA bundle to a directory called /data/quay/config/extra_ca_certs on the quay host and reboot the container. 
{: style="color:black; font-size: 80%;"}

#### S3 Policy for allowing Quay Registry access
{: style="color:black; font-size: 80%;"}

<pre>
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Principal": {
"AWS": "arn:aws:iam::AWS-account-ID:root"
},
"Action": [
"s3:ListBucket",
"s3:GetBucketLocation",
"s3:ListBucketMultipartUploads",
"s3:PutBucketCORS"
],
"Resource": "arn:aws:s3:::bucket-name"
},
{
"Effect": "Allow",
"Principal": {
"AWS": "arn:aws:iam::AWS-account-ID:root"
},
"Action": [
"s3:PutObject",
"s3:GetObject",
"s3:DeleteObject",
"s3:ListMultipartUploadParts",
"s3:AbortMultipartUpload"
],
"Resource": "arn:aws:s3:::bucket-name/*"
}
]
}
</pre>
{: style="color:black; font-size: 70%;"}
