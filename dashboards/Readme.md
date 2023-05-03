# Splunk Dashboards
> All dashboards use the `main` index to parse logs

## Web Activity Monitoring
> This dashboard looks for common SQLi, File Inclusion, and XSS attacks along with irregular web activity such as weird HTTP requests, possible web shells, and user agent strings. 
 ### Search Fields: 
 - Search Timeframe
 - Target Host
 - Client IP
 - HTTP Response Code
 ### Example Dashboard Activity:
  ![](images/web-activity/1.png)
  ![](images/web-activity/2.png)
  ![](images/web-activity/3.png)
  ![](images/web-activity/4.png)


## User Activity Monitoring
> This dashboard monitors anomalous user activity on both Windows and UNIX OSes. It takes the top/most interesting fields from the Windows Activity and the Linux Activity Dashboards. 
 ### Search Fields: 
 - Search Timeframe
 - Target Host
 - Client IP
 - User Name
 ### Example Dashboard Activity:
  ![](images/user-activity/1.png)