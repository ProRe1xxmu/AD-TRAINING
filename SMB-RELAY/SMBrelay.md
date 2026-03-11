# SMB RELAY ATTACK

we already have access to SUIKA account from ntlm poisoning attack and can go to her computer but we can do more things with responder and auth messages than just crack them. We can provide auth message to another machine to get SAM file of even get reverse shell! let's see how it works.
# TRAINING
First thing is learn which computers have SMB pakets signing off or not required. i will do this with NMAP.

> full scan at [nmap_output.txt](https://github.com/ProRe1xxmu/AD-TRAINING/edit/main/SMB-RELAY/nmap_output.txt)

well... nothing here, all ports filtered. We should see something like: script scan 445 at 192.168.31.151: SMB signing enabled, but not required.

Let's try it anyway. By default, SMB signing required on DC but not on Workstations. To perform attack, we'll use responder and new tool called **ntlmrelayx**
> FROM GITHUB: This module performs the SMB Relay attacks originally discovered by cDc extended to many target protocols (SMB, MSSQL, LDAP, etc).  It receives a list of targets and for every connection received it  will choose the next target and try to relay the credentials. Also, if specified, it will first try to authenticate against the client connecting to us. It is implemented by invoking a SMB and HTTP Server, hooking to a few functions and then using the specific protocol clients (e.g. SMB, LDAP). It is supposed to be working on any LM Compatibility level. The only way to stop this attack is to enforce on the server SPN checks and or signing.
If the authentication against the targets succeeds, the client authentication succeeds as well and a valid connection is set against the local smbserver. It's up to the user to set up the local smbserver functionality. One option is to set up shares with whatever files you want to so the victim thinks it's connected to a valid SMB server. All that is done through the smb.conf file or programmatically.

After program start, we need to wait some traffic in domain. Ooops, it seems, user reimu, which has admin rights on both workstations mistype DC netbios name. start [responder](https://github.com/ProRe1xxmu/AD-TRAINING/edit/main/SMB-RELAY/responder_out.txt) and see poisoning. start ntlmrelayx with -tf targets.txt, and wait to dump SAM file with logins and hashes. alright, we dumped hashes on [192.168.31.152_samhashes.sam](1https://github.com/ProRe1xxmu/AD-TRAINING/edit/main/SMB-RELAY/192.168.31.152_samhashes.sam), with this file, we can proceed PtH attacks to local users on affected machine.
> ntlmrelayx output [here](https://github.com/ProRe1xxmu/AD-TRAINING/edit/main/SMB-RELAY/ntlmrelayx_out.txt).
To gain shell access on targeted machine, we can use impacket-psexec, but for now, victim doesn't have shares we can use.

