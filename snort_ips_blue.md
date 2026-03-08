## Snort
- **Basic usage**
	- Test the configs
		- `snort -c path/to/config/file -T` - c gives path to config and T starts the test
	- Sniffer mode
		- -v : Verbose. Display the TCP/IP output in the console.
		- -d : Display the packet data (payload).
		- -e : Display the link-layer (TCP/IP/UDP/ICMP) headers.
		- -X : Display the full packet details in HEX.
		- -i : This parameter helps define a specific network interface to listen to or sniff. Once you have multiple interfaces, you can choose a specific interface to sniff.
	- Logger mode
		- -l : Logger mode, target log, and alert output directory.The default action is to dump as tcpdump format in /var/log/snort
		- -K ASCII : Log packets in ASCII format. Categorized to human readable files and folders for easy navigation
		- -r : Reading option: Review the logged events in Snort.
			- also allows users to filter the binary log files. using Berkeley Packet Filters (BPF)
			- `sudo snort -r logname.log 'udp and port 53'`
			- --pcap-list=" " : Read pcaps provided in command (space separated). (NOT used together with -r)
			- --pcap-show : Show pcap name on console during processing.
		- -n : Specify the number of packets to be processed or read. Snort will stop after reading the specified number of packets.
		- -q : quiet mode. hide stuff like banners and summary
	- IDS/IPS mode
		- -c : Remember to put the conf or rules file to be used
		- -N : Disable logging.
		- -D : Background mode.
		- -A : Alert modes
			- **Full:** Full alert mode, providing all possible information about the alert. This mode is also the default; once you use -A and don't specify a mode, Snort defaults to this mode. (no output in console)
			- *Fast mode displays the alert message, timestamp, source and destination IP addresses*, along with port numbers.
			- **Console**: Provides fast style alerts on the console screen.
			- **cmg:** CMG style,  basic header details with payload in hex and text format.
			- **None:** Disabling alerting. However, it still logs the traffic and creates a log file in binary dump format.
		- IPS mode
			- Snort IPS mode is activated with the `-Q-- daq afpacket` parameters
			- `snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 -A console` 
			- You can also activate this mode by editing the Snort.conf file. 

- **Rules**
	- Structure : `action protocol src_ip src_port direction dst_ip dst_port (options)`
	- Example : `alert tcp any any -> 192.168.1.10 80 (msg:"Possible web attack"; content:"/admin"; sid:1000001; rev:1;)`
	- Parts: 
		-   `alert` → action (alert, log, pass, drop, reject, sdrop)    
			-  alert: Generate an alert and log the packet.
			-   log: Log the packet.
			-   drop: Block and log the packet.
			-   reject: Block the packet, log it, and terminate the packet session.
		-   `tcp` → protocol (tcp, udp, icmp, ip)    
			- Note that Snort2 supports only these four protocol filters in the rules 
			- But you can detect the application flows using port numbers and options
			- Eg: if you want to detect FTP traffic, you cannot use the FTP keyword in the protocol field; instead, you can filter FTP traffic by investigating TCP traffic on port 21.
		-   `any any` -> source IP and port    
		-   `->` -> direction (`->`  or `<>`, there is no `<-`)    
		-   `192.168.1.10 80` -> destination IP and port    
		-   `( ... )` -> options in key:value; pairs, separated by `;`
	- Common options:
		-   `msg:"text"` -> message in alert    
		-   `sid:1000001` -> Snort rule ID (unique)   
			 -  `<100` : Reserved rules
			-   `100-999,999` : Rules came with the build.
			-   `>=1,000,000` : Rules created by user.
		-   `rev:1` -> revision number    
		-   `content:"string"` -> payload match    
		-   `classtype:attempted-admin` -> category    
		-   `priority:1` -> priority level
		- id, flags (tcp flag), dsize (payload size), sameip -> non-payload detection rules
		- `reference:CVE-xxxx` -> reference, duh
	- Special cases: 
		- `192.168.1.0/24` -> CIDR networks
		- `[192.168.1.10,192.168.1.34]` -> IP list
		- `!192.168.1.1` -> Excluding IP
		-   Port range:    
		    -   `1024:65535` (1024 to 65535)      
		    -   `:1023` (up to 1023)        
		    -   `1024:` (1024 and above)
- **Noteworthy side-chicks**
	- Snort rule types

		- **Community rules** -> Free, GPLv2, basic/older coverage, no license restrictions for normal use.
		- **Registered rules** -> Free, but rules are delayed (about 30 days behind subscriber set).
		- **Subscriber rules** -> Paid, latest rules immediately, best/fastest coverage.

	- Rule paths (snort.conf)

		- `rule_path` -> Directory for normal text rules (`*.rules`).
		- `so_rule_path` -> Directory for shared object rules (`*.so` + stub `.rules`).
		- `preproc_rule_path` -> Directory for preprocessor rules (e.g., for HTTP, SSH preprocessors).

Example:
```text
var RULE_PATH rules
var SO_RULE_PATH so_rules
var PREPROC_RULE_PATH preproc_rules
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg2MTA3MjM1NiwtMTQzMDgxOTY5MiwtNj
czNDc3NTUxLDEyMTQ3NDQ4MjUsNTY5MDczNTYxLC0xMDg1MDM0
OTQxLDE0MjU3OTE2NjgsLTIwMTE3Mzc3MzcsMTQwNTA2NDUyLD
E3ODQzOTAxOTgsNjM5MDAxMTE5LDkxNjMzMjA0OSw4Njk3MzY1
MTEsLTc1MTIxMjg2LC0xODgxNzYwNDU4LC0xNTQyMzM3MzQzLD
E2MjQ2MzE0OTQsLTkyMjIzNzAyN119
-->