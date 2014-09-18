```
title: Windows Server 2008 R2 on Cloudformation
layout: post
tags: ['cloudformation', 'windows server','post']
```

## Making Windows Server bend to your will

Windows Server 2012 is a breeze on Cloudformation.

The reference for Server 2012 is here: http://aws.amazon.com/microsoft/whitepapers/ad-reference-architecture/
You can find the cloudformation template from that whitepaper here: https://s3.amazonaws.com/microsoft_windows/activedirectory/Template_1_AD_2012.template
This builds a multi-AZ Active Directory environment with multiple subnets ready to deploy your multi-tier web application.
Sometimes a multi-AZ setup is a little bit overkill.  In order to save some server costs in a development or testing environment we can modify the Cloudformation script to remove the second Availability Zone.

Some interesting features demo'd by this sample template:

*Template parameters*
```javascript
...
        "DomainDNSName"    : {
            "Description" : "Fully qualified domain name (FQDN) of the forest root domain e.g. example.com",
            "Type"        : "String",
            "Default"     : "example.com",
            "MinLength"   : "3",
            "MaxLength"   : "25",
            "AllowedPattern" : "[a-zA-Z0-9]+\\..+"
        },
        "DomainNetBIOSName" : {
            "Description" : "NetBIOS name of the domain (upto 15 characters) for users of earlier versions of Windows e.g. EXAMPLE",
            "Type"        : "String",
            "Default"     : "example",
            "MinLength"   : "1",
            "MaxLength"   : "15",
            "AllowedPattern" : "[a-zA-Z0-9]+"
        },
...
```
The template parameters are shown to you during the Stack creation wizard in the AWS console so you can send runtime parameters to your environment.  These two parameters allow you to specify the Active Directory domain instead of having to hard code it in to the images.

*Embedded powershell scripting*
```javascript
...
    "commands" : {
       "2-execute-powershell-script-RenameComputer" : {
           "command" : {
               "Fn::Join" : [
                   "",
                   [
                       "powershell.exe Rename-Computer -NewName ",
                       {
                           "Ref" : "ADServerNetBIOSName1"
                       },
                       " -Restart"
                   ]
               ]
           },
           "waitAfterCompletion" : "forever"
       },
       ...
    }

```

As part of the server initiation powershell (or command) scripts can be run to do things like promote a domain controller or rename the instance.

The template provided in the whitepaper uses Windows 2012 specific domain promotion commands in the powershell so we have to change ours to use the dcpromo method supported by Windows Server 2008 R2.

```javascript
...
   "3-run-dcpromo" : {
      "waitAfterCompletion" : "forever",
      "command" : {
         "Fn::Join" : [
            "",
            [
               "C:\\cfn\\RunCommand.bat \"dcpromo /unattend  /ReplicaOrNewDomain:Domain  /NewDomain:Forest  /NewDomainDNSName:",
               {
                  "Ref" : "DomainDNSName"
               },
               "  /ForestLevel:4 /DomainNetbiosName:",
               {
                  "Ref" : "DomainNetBIOSName"
               },
               " /SiteName:\"AZ1\" /DomainLevel:4  /InstallDNS:Yes  /ConfirmGc:Yes  /CreateDNSDelegation:No  /DatabasePath:\"C:\\Windows\\NTDS\"  /LogPath:\"C:\\Windows\\NTDS\"  /SYSVOLPath:\"C:\\Windows\\SYSVOL\" /SafeModeAdminPassword=",
               {
                  "Ref" : "RestoreModePassword"
               },
               " /RebootOnCompletion:Yes\""
            ]
         ]
      }
   },
...
```

###AA TODO:
* Adjust it for single AZ setup (Host template in github)
* Adjust single AZ for Windows Server 2008 (Host template in github)

