# Splunk Guide
> Note: prior to following this guide, install any Linux OS and assign a static ip address

## Installing Splunk Server
### Downloading Splunk
1. Download the package on any Linux VM
- `cd /opt`
##### Debian-based
- 	`wget -O splunk.deb https://download.splunk.com/products/splunk/releases/9.0.1/linux/splunk-9.0.1-82c987350fde-linux-2.6-amd64.deb `
-	`dpkg -i splunk.deb`
##### RHEL-based
 -	`wget -O splunk.tgz https://download.splunk.com/products/splunk/releases/9.0.1/linux/splunk-9.0.1-82c987350fde-Linux-x86_64.tgz`
-	`tar -xvzf splunk.tgz`

### Starting the Installation
2. Install the server from the `/opt/splunk/bin/` folder:
- 	`./splunk start --accept license`
- 	`./splunk enable boot-start`

3. After the installation completes, navigate to the web interface IP & Login with the credentials \
 ![](images/1.png)

## Installing Apps
> Note: these are some useful applications and add-ons i enjoy using

### Table of Apps 
| App                                   | Link										| Sourcetype/Index  |
| :----------------------------------- 	| :---------------------------------------  | :---------------	|
| Sysmon								| https://splunkbase.splunk.com/app/1914/ 	| xmlwineventlog	|
| Alert Manager							| https://splunkbase.splunk.com/app/2665 	| 					|
| Enterprise Security (restricted☹) 	| https://splunkbase.splunk.com/app/263 	| 					|
| MITRE ATT&CK							| https://splunkbase.splunk.com/app/4617 	| 					|
| CIM									| https://splunkbase.splunk.com/app/1621 	| 					|
| Sankey Diagrams for MITRE 			| https://splunkbase.splunk.com/app/3112 	| 					|
| Palo Alto Add-on						| https://splunkbase.splunk.com/app/2757 	| 					|
| Palo Alto App 						| https://splunkbase.splunk.com/app/491 	| 					|
| pfSense add-ons						| https://splunkbase.splunk.com/app/1527 	| 					|
| Website Monitoring 					| https://splunkbase.splunk.com/app/1493 	| 					|
| Pstree								| https://splunkbase.splunk.com/app/5721 	| N/A 				|
| *Requirement for pstree* | *pip3 install splunk-dev*						|   					|

### Instructions for Installation
1. Download whatever apps you want
2. Go to `Search & Reporting > Apps > Manage Apps` \
 ![](images/2.png)
3. Select "Install app from file" \
 ![](images/3.png)
4. Upload the app by clicking browse, selecting your file, then clicking upload \
 ![](images/4.png) 

## Configuring Receiving for Universal Forwarders and Syslog
1. Login to the Management UI – [https://xxxxx:8000](https://xxxxx:8000)

### Syslog Receiving – 
> Used for networking devices, like Cisco Routers, Switches, PA-VMs, etc.

2. Navigate to `Settings > Data inputs` \
 ![](images/5.png)
3. Find the UDP Local Input Type and Click ‘Add new’ \
 ![](images/6.png)
4. Under Port, type 514, then click next \
 ![](images/7.png)
5. On the next page, select the source type of “syslog”, you may have to search & scroll down for it. Then under index, create a new index for these logs. Name it syslog & leave everything else default \
 ![](images/8.png)
6. On the review page, click Submit to finish adding the log input

### Universal Forwarder Receiving -
1. Go to `Settings > Forwarding and Receiving` \
 ![](images/9.png)
2. Under `Receive data > Configure Receiving`, click Add new \
 ![](images/10.png)
3. On the next page, type 9997 for “Listen on this port”, then click Save \
  ![](images/11.png)

## Setup Forwarder Monitoring
1. To verify your inputs, go to `Settings > Monitoring Console` \
 ![](images/12.png)
2. Then navigate to `Forwarders > Forwarders Instance` \
 ![](images/13.png)
3. You will have to enable Forwarder Monitoring before you can see anything, so on this page, click the setup link \
 ![](images/14.png)
4. Click Enable under Forwarder Monitoring and then Save \
 ![](images/15.png)
5. If you at any point fuck up your forwarders, change a hostname, etc. You can go back to this page and “Rebuild forwarder assets” \
 ![](images/16.png)
6. Go back to `Forwarders > Forwarders: Instance`. And you will see within 15minutes, all of the configured forwarders 😊

## Creating Windows & Linux Indexes
> Add two new indexes: **linux** and **windows** so you can parse through logs easier

1. Go to `Settings > Indexes` \
 ![](images/17.png)
2. New Index \
 ![](images/18.png) \
3. Set the following parameters for both the linux and windows index \
 ![](images/19.png)
4. Set up forwarders w the following command: 
-`./splunk add monitor / -index linux`


## Backing Up Your Indexes 
> From the server console (ie login to the computer that is hosting your splunk server and open a terminal)

1. Backup your existing cold/warm buckets:
` cp -r /opt/splunk/var/lib/splunk/<indexname>/* ~/<indexname>/`

2) Roll Hot DB to Warm DB
`./splunk search "| debug cmd=roll index= <indexname> "`

3) Backup new warm bucket:
` cp -r /opt/splunk/var/lib/splunk/<indexname>/db/* ~/<indexname>/db/`
