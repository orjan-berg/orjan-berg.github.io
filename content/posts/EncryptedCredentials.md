---
title: "Kryptert passord"
date: 2023-02-03T15:24:39+01:00
draft: false
toc: false
images:
tags:
  - powershell
---

## Hvordan kan vi bruke kryptert passord i Powershell skript?

### Lage et kryptert passord

Vi lager en funksjon som lagrer et kryptert passord i en fil på disk.

```
#function to Save Credentials to a file
Function Save-Credential([string]$UserName, [string]$KeyPath)
{
    #Create directory for Key file
    If (!(Test-Path $KeyPath)) {        
        Try {
            New-Item -ItemType Directory -Path $KeyPath -ErrorAction STOP | Out-Null
        }
        Catch {
            Throw $_.Exception.Message
        }
    }
    #store password encrypted in file
    $Credential = Get-Credential -Message "Enter the Credentials:" -UserName $UserName
    $Credential.Password | ConvertFrom-SecureString | Out-File "$($KeyPath)\$($Credential.Username).cred" -Force
}
```
Deretter bruker vi funksjonen ``Save-Credential -Username "vismaadmin" -KeyPath "c:\exsitec\cred"``

Det krypterte passordet kan se ut som dette:
```
01000000d08c9ddf0115d1118c7a00c04fc297eb010000009fdff65875d7ee41850f1a31722de5640000000002000000000003660000c000000010000000b331f6114b6428f1d4a392c2e19194ec0000000004800000a0000000100000007ca12454435ebd491dc697abe692d05918000000883adc0c65048f382253a512e795fdc4b354a1312a393c761400000052cbcca4313d77f80b0a6c679152b30f5cd0f5b1
```

Når vi har det krypterte passordet, kan vi lese det fra fil og bruke det i et skript:
```
#function to get credentials from a Saved file
Function Get-SavedCredential([string]$UserName,[string]$KeyPath)
{
    If(Test-Path "$($KeyPath)\$($Username).cred") {
        $SecureString = Get-Content "$($KeyPath)\$($Username).cred" | ConvertTo-SecureString
        $Credential = New-Object System.Management.Automation.PSCredential -ArgumentList $Username, $SecureString
    }
    Else {
        Throw "Unable to locate a credential for $($Username)"
    }
    Return $Credential
}
```
Vi kaller på passordet på denne måten ``$Cred = Get-SavedCredential -Username "vismaadmin" -KeyPath "c:\exsitec\cred"``   
  
  
ide hentet fra [Sharepoint Diary](https://www.sharepointdiary.com/2020/01/read-write-encrypted-password-file-in-powershell-script.html).
