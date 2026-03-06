## Snort
- **Basics (testing, logging and sniffing)**
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
		- -n : Specify the number of packets to be processed or read. Snort will stop after reading the specified number of packets.

- **IDS/IPS mode** 
	- -N : Disable logging.
	- -D : Background mode.
	- -A : Alert modes
		- **Full:** Full alert mode, providing all possible information about the alert. This mode is also the default; once you use -A and don't specify a mode, Snort defaults to this mode. (no output in console)
		- *Fast mode displays the alert message, timestamp, source and destination IP addresses*, along with port numbers.
		- **Console**: Provides fast style alerts on the console screen.
		- **cmg:** CMG style,  basic header details with payload in hex and text format.
		- **None:** Disabling alerting.
<!--stackedit_data:
eyJoaXN0b3J5IjpbODY5NzM2NTExLC03NTEyMTI4NiwtMTg4MT
c2MDQ1OCwtMTU0MjMzNzM0MywxNjI0NjMxNDk0LC05MjIyMzcw
MjddfQ==
-->