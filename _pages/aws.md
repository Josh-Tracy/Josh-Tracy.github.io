---
title: "AWS Lessons Learned"
permalink: /aws/
header:
  image: "/images/banner.png"
---

## AWS Lessons Learned

### Table Of Contents
* <a href="#CLI"> AWS CLI</a>
  * <a href="#Credentials"> Credentials </a>

<h3 id="CLI"> AWS CLI <h3>

<h4 id="Credentials"> CLI Credentials </h4>
{: style="color:DodgerBlue;"}

The AWS CLI configuration is stored in <b>.AWS</b> in the home directory

* ~/.aws/credentials
<pre>
    [default]
    aws_access_key_id=AKIAIOSFODNN7EXAMPLE
    aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
</pre>

* ~/.aws/config