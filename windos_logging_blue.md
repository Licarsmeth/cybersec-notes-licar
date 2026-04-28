---


---

<h2 id="windows-logging">Windows Logging</h2>
<ul>
<li><strong>Security Logs</strong>
<ul>
<li>Authentication
<ul>
<li>Successful Logon/ Failed Logon (4624/ 4625)
<ul>
<li>Logon type is a login method: 10 for RDP, 3 for Network, etc</li>
<li>In the details, look for “new logon” section. You can find things like
<ul>
<li>Security ID</li>
<li>Account name</li>
<li>Account Domain</li>
<li>Logon ID (take note, it’s unique)</li>
</ul>
</li>
<li>Inside “Network information”, you see things like source IP (can be trusted) and hostname (can’t be trusted)</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
<li><strong>User Management</strong>
<ul>
<li>4720/4722/4738 : A user account was created / enabled / changed</li>
<li>4725/4726 : A user account was disabled / deleted</li>
<li>4723/4724 : A user changed their password / User’s password was reset</li>
<li>4732/4733 : A user was added to / removed from a security group</li>
<li>Structure of the Events:
<ul>
<li>Subject: The account doing the action. Note the Logon ID field - you can use it to correlate this event with the preceding 4624 login event!</li>
<li>Object: This can be named differently depending on an event ID (e.g. New Account or Member), but it always means the same - the target of the action</li>
<li>Details: A target group for 4732 and 4733 events, or new user’s attributes like full name or password expiration settings for the 4720 event.</li>
</ul>
</li>
</ul>
</li>
<li><strong>Process Monitoring - Sysmon</strong>
<ul>
<li>4688 (Security Log: Process Creation)
<ul>
<li>Disabled by default</li>
<li>Log an event every time a new process is launched, including its command line and parent process details</li>
<li>Service creation (autostart for persistence) 4697</li>
<li>Scheduled task creation 4698</li>
</ul>
</li>
<li>1 (Sysmon: Process Creation)
<ul>
<li>External tool not installed by default</li>
<li>Replace 4688 event code and provide more advanced fields like process hash and its signature</li>
<li>Most common sysmon fields:
<ul>
<li>Process info: PID, path (image), command line</li>
<li>Parent info</li>
<li>Binary info</li>
<li>User context: A user running the process, and most importantly- the logon ID</li>
</ul>
</li>
<li>Some sysmon events
<ul>
<li>11/13 (File Create, eg: startup programs / Registry Value Set, eg: “run” registry program)
<ul>
<li>Detect files dropped by malware or its changes to the registry (e.g. for persistence)</li>
</ul>
</li>
<li>3/22 (Network Connection / DNS Query)
<ul>
<li>Detect traffic from untrusted processes or to known malicious destinations</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgxMDA0NDAwXX0=
-->