## Table of Contents

  - [Event Viewer](#Event\Viewer)
  - [Elements of a Windows Event Log](#Elements\of\a\Windows\Event\Log)


**Event Logs** record events taking place in the execution of a system to provide an audit trail that can be used to understand the activity of the system and to diagnost problems. They are essential to understand the activities of complex systems, particularly in applications with little user interaction


This is where SIEMs (**Security information and event management**) such as Splunk and Elastic come into play.

If you don't know exactly what a SEIM is used for, below is a visual overview of its capabilities.

![The image is showing the capabilities provided by a SIEM such as threat detection, investigation and time to respond](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/c5cd275e2515b64a8e999bf7f0456466.png)

- Even though accessing a remote machines logs is possible, this will not be feasible in large environement. Instead, one can view logs from all endpoints, appliances, etc., in a SIEM. This will allow you to query the logs from multiple devices instead of manually connecting to a single device.


## Event Viewer
The windows Event Logs are not text files that can be viewed using a text editor. However, the raw data can be translated into XML using the WindowsAPI. The events in these log iles are stored in a propietary binary format with a `.evt` or `.evtx` extension. The log files with the `.evtx` file extension typically reside in: `C:\Windows\System32\winevt\Logs`

## Elements of a Windows Event Log
Event logs are crucial for troubleshooting any computer incident and help understand the situation and how to remediate it. 

- Elements stored in Windows Event Logs:
	- System Logs
	- Security Logs
	- Application Logs
	- Directory Service Events
	- File Replication Service Events: Records events associated with Windows Servers during the sharing of Group Policies
	- DNS Event Logs: DNS servers use these logs to record domain events and to map out
	- Custom Logs: Events logged by applications that require custom data storage.

- **Level:** Highlights the log recorded type based on the identified event types specified earlier. In this case, the log is labeled as **Information**.
- **Date and Time:** Highlights the time at which the event was logged.
- **Source:** The name of the software that logs the event is identified. From the above image, the source is PowerShell.
- **Event ID:** This is a predefined numerical value that maps to a specific operation or event based on the log source. This makes Event IDs not unique, so `Event ID 4103` in the above image is related to Executing Pipeline but will have an entirely different meaning in another event log.
- **Task Category:** Highlights the Event Category. This entry will help you organize events so the Event Viewer can filter them. The event source defines this column.











  