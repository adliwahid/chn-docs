Deploying More Honeypots
=============================

## Prerequisites

The default deployment model uses Docker and Docker Compose to deploy containers for the project's tools, and so, require the following:

* Docker >= 1.13.1
* Docker Compose >= 1.15.0

**Please ensure the user on the system installing the honeypot is in the local
 docker group**
 
 Please see your system documentation for adding a user to the docker group.

## Deploying More Honeypots

Currently the following Honeypots are supported with CHN:

* [Cowrie](cowrie.md)
* [Dionaea](dionaea.md)
* [Glastopf](glastopf.md)
* [Wordpot](wordpot.md)
* [Conpot](conpot.md)
* [Amun](amun.md)
* [RDPHoney](rdphoney.md)

The general deployment model is the same for each of these:
* Select the deploy script you wish to deploy via the web interface
* Paste the "Deploy Command" into a VM host *with Docker installed already* 
* Verify installation succeded by examing the command line output and 
checking the view sensor page on CHN Server

## Customizing your deployments

To customize your deployment of a particular honeypot, you can copy an 
existing deployment script, change the drop down script selection option to 
"New Script", and paste into the script space. You can then create a new 
name, add notes, and save your new script.

You can then edit and re-save the script as desired. Some examples of items 
you might want to change include:

* Changing the listening port in the docker-compose.yml file
* Customizing options in the sysconfig file
* Addition additional code to ease deployment such as installing 
docker-compose and adding the local user to the docker group

**Please Note** that you should generally not change the ports in the 
sysconfig files, but rather change the ports that Docker translates 
connects to (i.e., in the )

### Adding tags to your honeypots

Tags are a feature that allow you to add strings to your honeypot 
configuration, and those strings will be propogated through the data 
transport to your logging infrastructure. This can ease the data analytics 
processes by letting you add metadata about your sensor directly to your log 
data.

To configure tagging, simply add a `TAGS` variable to your sysconfig file. 
For instance:

```bash
TAGS="provider:aws,type:cowrie,owner:alex"
```

This tag will then show up in your logs from [hpfeeds-logger](hpfeeds-logger.md) like this:

```bash
2019-02-19T20:34:56.425698 direction="inbound", protocol="ip", ids_type="network", tags="provider:aws,type:cowrie,owner:alex", dest="0.0.0.2", ssh_username="system", app="cowrie", transport="tcp", dest_port="2223", src="0.0.227.13", src_port="34038", severity="high", vendor_product="Cowrie", sensor="4a494c3c-51ab-4d9d-b55c-fbb6f14cc54f", ssh_password="shell", signature="SSH login attempted on cowrie honeypot", type="cowrie.sessions"
```

#### Guidance on tagging
The only rules around tagging are ensuring that you quote your list of tags, 
and we suggest not using spaces in your tags (YMMV). We suggest "key:value" 
formatted tags, as generally these are easy to parse out later and provide 
context. We have a few ideas on how tags might be used, which are captured 
below. Please do let us know what ideas you have for tags, or how you use them operationally!

```yaml
# sensor location related tags
country:US
asn:13371
bgp_prefix:152.3.0.0/16

# data related tags
personality:aws-ubuntu16

# the next option could be used in the future to prevent any sensor data from 
# being sent to the common CIF instance, or shared via hpfeeds sharing
exportable:false

# Prehaps administrative groups are useful for automation purposes
group:us-east-1b
fleet:aws
```
