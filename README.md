# vRealize Orchestrator package for interaction with phpIPAM

Updated Package version 0.0.4
on vRealize Orchestrator 8.5.1
Against phpIPAM v1.4
by Fabrice Verbrouckl

Original Package Version: 0.0.3
Developed on vRealize Orchestrator 7.0.1
Against phpIPAM v1.26 rev030 (Commit 6aff3873fa496d0611c47d574aead5b650516698) 
Developed by Mads Fog Albrechtslund  
Website: [https://hazenet.dk](https://hazenet.dk)  
Twitter: [@Hazenet](https://twitter.com/Hazenet)  

## Download

The package can be downloaded from the [Repository Releases page](https://github.com/archit3kt/vrealize-orchestrator-phpipam-package/releases)  

## Original Author Information

This package is developed in a similar way as the default VMware packages included with vRealize Orchestrator.  
That means you are free to view, duplicate and export the workflows, but you are not able to edit the workflows directly.

The reason for this, is so I can maintain workflow ID's for future revisions of the package.  

Because of this, it is not directly possible to delete this package again, just like the default VMware packages.  
But it can be manualy delete, by enabling the follow setting in the vRO Workflow Designer client.  

Tools -> User Preferences... -> General -> Delete non empty folder permitted

When that setting is enabled, it is possible to delete non-editable workflows, complete folders with workflows and folders inside.  
This setting allows the delete of both these phpIPAM related workflows and VMware default workflows.  

**SO BE VERY CAREFUL WHEN USING THIS SETTING**  
**AND DISABLE IT IMMEDIATELY AFTER**  

## Update Information

Almost all the work was done by hazenet here https://github.com/hazenet/vrealize-orchestrator-phpipam-package, thank him for the hard work !

I respected original author's desire not to update his workflow, so every worklow I updated were duplicated first and got the prefix "New phpipam 1.4"

Not much changed, in VRO 8.5 you've got to add the rest host with vmware's workflow with no auth. New phpIPAM 1.4 - Execute REST Request had some minor changes to reflect that :

- A new header is added in Execute Token Request : request.setHeader("Authorization", auth_base64); where auth_base64 is a SecureString in the Configuration Element of the package. This is typically a base64 encryption of your username and password for API login. If you added your phpIpam in your postman and used basic auth, it adds automatically the right header, you can take the content and put it in the configuration element. It should looks like something like this : "Basic dnelpoO12HSExGYVzzezerreTTdt123hX"
- I added a condition in Execute Request to check if phpipamTokenAttribute == "getToken". It allows to skip Execute Token Request and adds the right authorization header when the workflow is called by New phpIPAM 1.4 - Create Token
- New phpIPAM 1.4 - Create Token has now a fix value of "getToken" for the attribute phpipamToken to reflect the last point
- Every updated workflow must use the New phpIPAM 1.4 - Execute REST Request to work correctly
- New phpIPAM 1.4 - Create First Free Address by Subnet ID has a new body-content to be able to set hostname and description for the new entry if you want to

## Installed content

The package installs:  

Workflows -> Library -> phpIPAM  
Actions -> net.phpipam  
Configurations -> phpIPAM  
Packages -> net.phpipam  

## Usage

**Step 1 - Create phpIPAM App ID**  
In phpIPAM web-interface, go to Administration -> API  
  
Click "Create API key"

* Choose a "appid", which is a short unique identifier that will be part of the API URL, e.g. "vro" or "myapp"
* Leave the generate "App code"
* Select Permissions, recommend "Read / Write / Admin" for full functionality
* App Security, I have only tested it with "none"

**Step 2 - Add phpIPAM Rest Host**  

Add the rest Host with vmware's default Library / HTTP-REST / Configuration / Add a REST host and choose No Auth.

**Step 3 - Use the provided workflows**  
All workflows have a INPUT of "phpIPAM Token", this is an optional INPUT.  
If nothing is provided, the workflow will retrieve a phpIPAM Token automatically.  

For longer sequences of phpIPAM related workflows, consider getting a phpIPAM Token as the first step in the sequence, to lessen the load on the phpIPAM Server. 

**Step 4 - Enable/Disable logging of the REST Responses**  
Included in the package is a Action called "loggingOfResponse".  
This Action is used in the workflow "phpIPAM - Execute REST Request".  
To decide whether or not to enable this logging feature, use the Configuration Element "phpIPAM Logging", which has a Attribute called "loggingOfResponse".
