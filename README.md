# PowerShell Software Packaging Wrapper

This repo contains an powershell scripts to create an wrapping script in powershell to install, uninstall and detect applications through any software deployment solution. My choose for the software deployment solution is Microsoft Intune. After you´ve created the script to deploy the package you can use it to deploy the application on your Windows 10 clients.

## Install:
Select if you want to install your application based on EXE or MSI. You can also add File/Folder or RegKey/Value´s. It is posible to use multiple calls in the $install section.
```powershell
#Install EXE
Start-Process -FilePath "$PSScriptRoot\" -ArgumentList '' -Wait

#Install MSI
Start-Process -FilePath msiexec.exe -ArgumentList '/i "{}" /qn /l* "C:\Windows\Logs\INSTALL-MANUFACTURER-APPLICATION.log"' -Wait

#Add File or Folder
Copy-Item -Path "$PSScriptRoot\" -Destination "" -Recurse -Force

#Add RegKey
New-Item -Path "HKLM:\SOFTWARE\" -Name "" -Force

#Add RegKeyValue
New-ItemProperty "HKLM:\SOFTWARE\" -Name "" -PropertyType "String" -Value "" -Force

```

## Uninstall:
Select if you want to uninstall your application based on EXE or MSI. You can also remove File/Folder or RegKey/Value´s. It is posible to use multiple calls in the $uninstall section.
```powershell
#Uninstall EXE
Start-Process -FilePath "" -ArgumentList '' -Wait

#Uninstall MSI
Start-Process -FilePath msiexec.exe -ArgumentList '/x "{}" /qn /l* "C:\Windows\Logs\UNINSTALL-MANUFACTURER-APPLICATION.log"' -Wait

#Remove File or Folder
Remove-Item -Path "" -Recurse -Force

#Remove RegKey
Remove-Item -Path "HKLM:\SOFTWARE\" -Recurse -Force

#Remove RegKeyValue
Remove-ItemProperty -Path "HKLM:\SOFTWARE\" -Name ""

```

## Detection:
Select if you want to detect your application based on File/Folder, RegKeyValue or MSI Product Code. Uncomment the other $detection variables. It is NOT posible to use multiple calls in the $detect section.
```powershell
#Detect File or Folder
$detection = (Test-Path -Path "")

#Detect RegKeyValue
$detection = (Get-ItemProperty -Path "HKLM:\SOFTWARE\" | Select-Object -ExpandProperty "")

#Detect MSI ProductCode
$detection = (Get-WmiObject -class Win32_Product | Where-Object IdentifyingNumber -match "{}")

```

### Parameter definitions:
- -install installs the application on the windows client.
- -uninstall removes the application from the windows client.
- -detect checks if the application is installed on the windows client.
 
## Logfiles:
The scripts create a logfile with the name of the .ps1 script in the folder C:\Windows\Logs.

## Requirements:
- PowerShell 5.0
- Windows 10

Created by @niklasrast 