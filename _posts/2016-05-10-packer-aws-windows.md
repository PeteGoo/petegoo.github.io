---
layout: post
title: Getting Packer to work for Windows on AWS
categories:
- blog
tags:
- aws
- petegoo
- packer
- windows
- winrm
- https
status: publish
type: post
published: true
meta: {}
author:
  login: petegoo
  email: pete@petegoo.com
  display_name: PeteGoo
  first_name: Peter
  last_name: Goodman
---
Getting a Packer build to work with the AWS EBS builder is pretty easy. Getting it to work for Windows can be a series of less-than-obvious discoveries. I had issues trying to find a concise guide on how to get the various pieces working together, so here it is.

[All code available here](https://github.com/PeteGoo/packer-win-aws)

## The goal

We want Packer to create an EC2 AMI using a powershell initialization script. To achieve this Packer will create a new EC2 instance, run our script and then take an image of it before terminating our builder instance. We need any communication with the builder instance to use https rather than http so there is something approaching secure communication (although here we will use a self-signed cert, created on the instance itself).

* Builder: amazon-ebs
* Provisioner: powershell

## Using the amazon-ebs builder

The amazon-ebs builder is actually pretty good. The configuration is well documented and the config will end up looking something like below:

```
{
    "builders": [{
        "type": "amazon-ebs",
        "region": "us-east-1",
        "source_ami": "ami-3d787d57",
        "instance_type": "m3.medium",
        "ami_name": "windows-ami {{timestamp}}",
    }]
}
``` 

## WinRM and the infinite sadness

The next issue is that we need to be able to add a provisioner so we can run some scripts on the new builder instance. On linux boxes this is pretty standard as ssh actually works. Unfortunately on Windows in order to run Powershell remotely on the Packer builder instance we have to use Powershell remoting and that means WinRM. 

WinRM was originally designed for a world that was built on WS-*, SOAP and Kerberos authentication in Windows domains. Hence it has been plagued by configuration woes since it was first introduced. Getting it to work for Packer over the internet can be a pain.

So let's tell Packer to use winrm.

```
{
    "builders": [{
        "type": "amazon-ebs",
        "region": "us-east-1",
        "source_ami": "ami-3d787d57",
        "instance_type": "m3.medium",
        "ami_name": "windows-ami {{timestamp}}",
        "user_data_file":"./ec2-userdata.ps1",
        "communicator": "winrm",
        "winrm_username": "Administrator",
    }]
```

If you run this you will probably end up with the dreaded `waiting for winrm to become available` message from Packer that just sits there looking at you. This means that WinRM is not configured on the instance.

To resolve this problem we need to run a script on the builder instance to bootstrap WinRM. The way we tell an EC2 instance to run a script on first startup is the [UserData](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html#instancedata-add-user-data) script. On Windows this script [can contain](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-instance-metadata.html) a `<powershell></powershell>` section.


```
<powershell>

write-output "Running User Data Script"
write-host "(host) Running User Data Script"

Set-ExecutionPolicy Unrestricted -Scope LocalMachine -Force -ErrorAction Ignore

# Don't set this before Set-ExecutionPolicy as it throws an error
$ErrorActionPreference = "stop"

# Remove HTTP listener
Remove-Item -Path WSMan:\Localhost\listener\listener* -Recurse

# WinRM
write-output "Setting up WinRM"
write-host "(host) setting up WinRM"

cmd.exe /c winrm quickconfig -q
cmd.exe /c winrm quickconfig '-transport:http'
cmd.exe /c winrm set "winrm/config" '@{MaxTimeoutms="1800000"}'
cmd.exe /c winrm set "winrm/config/winrs" '@{MaxMemoryPerShellMB="1024"}'
cmd.exe /c winrm set "winrm/config/service" '@{AllowUnencrypted="true"}'
cmd.exe /c winrm set "winrm/config/client" '@{AllowUnencrypted="true"}'
cmd.exe /c winrm set "winrm/config/service/auth" '@{Basic="true"}'
cmd.exe /c winrm set "winrm/config/client/auth" '@{Basic="true"}'
cmd.exe /c winrm set "winrm/config/service/auth" '@{CredSSP="true"}'
cmd.exe /c winrm set "winrm/config/listener?Address=*+Transport=HTTP" '@{Port="5985"}'
cmd.exe /c netsh advfirewall firewall set rule group="remote administration" new enable=yes
cmd.exe /c netsh firewall add portopening TCP 5985 "Port 5985"
cmd.exe /c net stop winrm
cmd.exe /c sc config winrm start= auto
cmd.exe /c net start winrm
cmd.exe /c wmic useraccount where "name='vagrant'" set PasswordExpires=FALSE

</powershell>
```

We can now try to run the packer build

```
packer build template.json
```
 
## But WinRM still can't connect?

If you still get the `waiting for winrm to become available` message and it doesn't progress after a few minutes then something may have gone wrong in the above script. To diagnose that issue run packer with the debug flag.

```
packer build -debug template.json
``` 

Grab the Administrator login from the Packer output, you will need it. Then add an inbound RDP rule on the Packer build instance's security group so you can RDP to it. Look for the log at `C:\Program Files\Amazon\Ec2ConfigService\Logs\Ec2ConfigLog.txt`. You may need to add logging in the above script to figure out what is going wrong.

## But the security man! 

OK, so this script is ok but the communication is over plain http which is a little less than ideal. To make this https we can generate a new certificate on the machine and use that. We switch the port to `5986` and tell WinRM we are using https. 

``` 
<powershell>

write-output "Running User Data Script"
write-host "(host) Running User Data Script"

Set-ExecutionPolicy Unrestricted -Scope LocalMachine -Force -ErrorAction Ignore

# Don't set this before Set-ExecutionPolicy as it throws an error
$ErrorActionPreference = "stop"

# Remove HTTP listener
Remove-Item -Path WSMan:\Localhost\listener\listener* -Recurse

$Cert = New-SelfSignedCertificate -CertstoreLocation Cert:\LocalMachine\My -DnsName "packer"
New-Item -Path WSMan:\LocalHost\Listener -Transport HTTPS -Address * -CertificateThumbPrint $Cert.Thumbprint -Force

# WinRM
write-output "Setting up WinRM"
write-host "(host) setting up WinRM"

cmd.exe /c winrm quickconfig -q
cmd.exe /c winrm set "winrm/config" '@{MaxTimeoutms="1800000"}'
cmd.exe /c winrm set "winrm/config/winrs" '@{MaxMemoryPerShellMB="1024"}'
cmd.exe /c winrm set "winrm/config/service" '@{AllowUnencrypted="true"}'
cmd.exe /c winrm set "winrm/config/client" '@{AllowUnencrypted="true"}'
cmd.exe /c winrm set "winrm/config/service/auth" '@{Basic="true"}'
cmd.exe /c winrm set "winrm/config/client/auth" '@{Basic="true"}'
cmd.exe /c winrm set "winrm/config/service/auth" '@{CredSSP="true"}'
cmd.exe /c winrm set "winrm/config/listener?Address=*+Transport=HTTPS" "@{Port=`"5986`";Hostname=`"packer`";CertificateThumbprint=`"$($Cert.Thumbprint)`"}"
cmd.exe /c netsh advfirewall firewall set rule group="remote administration" new enable=yes
cmd.exe /c netsh firewall add portopening TCP 5986 "Port 5986"
cmd.exe /c net stop winrm
cmd.exe /c sc config winrm start= auto
cmd.exe /c net start winrm

</powershell>
```

## Adding the provisioner

Finally we can add our provisioner to our template.json.

```
{
    "builders": [{
        "type": "amazon-ebs",
        "region": "us-east-1",
        "source_ami": "ami-3d787d57",
        "instance_type": "m3.medium",
        "ami_name": "windows-ami {{timestamp}}",
        "user_data_file":"./ec2-userdata.ps1",
        "communicator": "winrm",
        "winrm_username": "Administrator",
        "winrm_use_ssl": true,
        "winrm_insecure": true
    }],

    "provisioners": [
        {
            "type": "powershell",
            "script": "init.ps1"
        }
    ]
}
```

Notice that we are now specifying `winrm_use_ssl`. The inclusion of `winrm_insecure` means that the Packer client will not verify the certificate chain which will obviously fail for our self signed certificate.

We can now add whatever setup we need into our init.ps1 script which will run over our (slightly more) secure WinRM connection.

The entire repo for this sample can be found at [https://github.com/PeteGoo/packer-win-aws](https://github.com/PeteGoo/packer-win-aws).
