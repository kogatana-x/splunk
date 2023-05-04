# Installing Universal Forwarders
## Downloading The Packages
 ### Windows
 - https://download.splunk.com/products/universalforwarder/releases/9.0.1/windows/splunkforwarder-9.0.1-82c987350fde-x64-release.msi

 ### Linux
  #### Debian-Based
- 	`wget -O splunk.deb https://download.splunk.com/products/universalforwarder/releases/9.0.1/linux/splunkforwarder-9.0.1-82c987350fde-linux-2.6-amd64.deb` 
-	`dpkg -i splunk.deb`
  #### RHEL-Based

 - `wget -O splunk.tgz https://download.splunk.com/products/universalforwarder/releases/9.0.1/linux/splunkforwarder-9.0.1-82c987350fde-Linux-x86_64.tgz` 
-	`tar -xvzf splunk.tgz`

## Installing The Forwarders
- `./splunk start --accept-license`
- `./splunk enable boot-start`
- `./splunk add forward-server x.x.x.x:9997`
- `./splunk add monitor /var/log/`
- `./splunk restart`

##  Sysmon
### 1. Install using the following command:
- `Sysmon64.exe -accepteula -I sysmonconfig-export.txt`

### 2. Configure the forwarder to send Sysmon data to Splunk.  
3. Navigate to `c:\program files\splunkuniversalforwarder\etc\apps\splunkuniversalforwarder\local\inputs.conf` and add the following to the end of the file:
> `[WinEventLog://Microsoft-Windows-Sysmon/Operational]
 disabled = false
 renderXml = true
 source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational`

4. Restart the service.

## Easy Search To See Forwarders:
- `index=main | stats count(_raw) by host`
- 
## Web monitoring input
- http://x.x.x.x:8000/en-US/app/website_monitoring/web_ping_batch_create
 ![](images/21.png)

- http://x.x.x.x:8000/en-US/manager/website_monitoring/data/inputs/web_ping
  ![](images/21.png)


