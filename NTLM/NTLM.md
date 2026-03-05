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
> [insert credentials]

But... why is this happend? because of LLMNR. 
LLMNR is Microsoft developed protocol to resolve names without DNS server. if DNS fails, computer send `who knows about domain-exploiit?` to all computers. LLMNR trusts all answers, so we can prefer **Man-In-The-Middle(MITM)** attack.
> See full output of responder on attacker machine in [responer_output.txt](https://google.com/)

after getting all what we need, we can crack this password with **hashcat**
> FROM GITHUB: **Hashcat** is a password recovery tool. It had a proprietary code base until 2015, but was then released as open source software. Versions are available for Linux, macOS, and Windows. Examples of hashcat-supported hashing algorithms are MD hashes, MD4, MD5, SHA-family and Unix Crypt formats as well as algorithms used in MySQL Cisco PIX
> https://hashcat.net/hashcat/



