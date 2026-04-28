---


---

<h2 id="linux-threat-detection">Linux Threat Detection</h2>
<ul>
<li><strong>Discovery Commands</strong>
<ul>
<li>OS and File system discovery: <code>pwd</code>, <code>ls /</code>, <code>env</code>, <code>uname -a</code>, <code>lsb_release -a</code>, <code>hostname</code></li>
<li>User and Groups Discovery: <code>id</code>, <code>whoami</code>, <code>w</code>, <code>last</code>, <code>cat /etc/sudoers</code>, <code>cat /etc/passwd</code></li>
<li>Process and Network Discovery: <code>ps aux</code>, <code>top</code>, <code>ip a</code>, <code>ip r</code>, <code>arp -a</code>, <code>ss -tnlp</code>, <code>netstat -tnlp</code></li>
<li>Cloud or Sandbox Discovery: <code>systemd-detect-virt</code>, <code>lsmod</code>, <code>uptime</code>, <code>pgrep "&lt;edr-or-sandbox&gt;"</code></li>
</ul>
</li>
<li><strong>Auditd</strong>
<ul>
<li>External tool to log processes and other events, like sysmon in windows</li>
<li>Harder to search with only grep since the logs aren’t written down in exact texts, so we use a command called ausearch</li>
<li><code>ausearch -i</code> to interpret numeric entities into human readable form</li>
<li><code>ausearch -i -if &lt;filename.log&gt;</code> to specify the file you’re searching through. It used /var/log/audit/audit.log by default</li>
<li><code>ausearch -i -x sshd</code> search by specific executable name like ssh</li>
<li><code>ausearch -i -f &lt;filename&gt;</code> to search for events related to that file</li>
<li><code>ausearch -i -c python3</code> search by specific comman, like python3</li>
<li><code>ausearch -i --pid &lt;pid&gt; or --ppid &lt;ppid&gt;</code> for pid or ppid search</li>
<li>You can pipe grep like <code>ausearch -i --pid 334 | grep 'proctitle='</code></li>
<li><code>-au auid</code> -&gt; Audit user ID (login session owner, set at login)</li>
<li><code>-ui uid</code> -&gt; Real user ID (process owner)</li>
<li><code>-ue euid</code> -&gt; Effective user ID (privileges used)</li>
<li><code>-ug gid</code> -&gt; Group ID</li>
</ul>
</li>
</ul>

