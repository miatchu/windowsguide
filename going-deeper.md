# Going deeper

## Removing UWP Apps that can't be removed by common measures

### Tools that are required for this Process:

install\_wim\_tweak.exe: [Download it here](https://github.com/Espionage724/Windows/raw/master/install\_wim\_tweak/install\_wim\_tweak.exe) and move/copy it into _C:\Windows\System32_

## The Removing

### Windows Defender

Open Group Policy Editor via _gpedit.msc_ and navigate to _Computer Configuration > Administrative Templates > Windows Components > Microsoft Defender Antivirus_.\
\
Double-click on "Turn off Microsoft Defender Antivirus".\
\
Enabled will disable Windows Defender Antivirus, and Not Configured or Disabled will enable Windows Defender Antivirus.

### **Windows Store**

**Open both Powershell and Command Prompt as Administrator for any App**

Powershell: `Get-AppxPackage -AllUsers *store* | Remove-AppxPackage`\
You can ignore any error that pops up.

Command Prompt:

`install_wim_tweak /o /c Microsoft-Windows-ContentDeliveryManager /r`\
`install_wim_tweak /o /c Microsoft-Windows-Store /r`\
`reg add "HKLM\Software\Policies\Microsoft\WindowsStore" /v RemoveWindowsStore /t REG_DWORD /d 1 /f`\
`reg add "HKLM\Software\Policies\Microsoft\WindowsStore" /v DisableStoreApps /t REG_DWORD /d 1 /f`\
`reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\AppHost" /v "EnableWebContentEvaluation" /t REG_DWORD /d 0 /f`\
`reg add "HKLM\SOFTWARE\Policies\Microsoft\PushToInstall" /v DisablePushToInstall /t REG_DWORD /d 1 /f`\
`reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SilentInstalledAppsEnabled /t REG_DWORD /d 0 /f`\
`sc delete PushToInstall`

### **Music, TV, etc.**

Powershell:

`Get-AppxPackage -AllUsers *zune* | Remove-AppxPackage`\
`Get-WindowsPackage -Online | Where PackageName -like *MediaPlayer* | Remove-WindowsPackage -Online -NoRestart`

### **Xbox and Game DVR**

Powershell: `Get-AppxPackage -AllUsers *xbox* | Remove-AppxPackage`

You can ignore any error that pops up.

Command Prompt:

`sc delete XblAuthManager`\
`sc delete XblGameSave`\
`sc delete XboxNetApiSvc`\
`sc delete XboxGipSvc`\
`reg delete "HKLM\SYSTEM\CurrentControlSet\Services\xbgm" /f`\
`schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTask" /disable`\
`schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTaskLogon" /disable`\
`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f`\
Additionally, go to Start > Settings > Gaming and turn off everything.

### **Sticky Notes**

Powershell: `Get-AppxPackage -AllUsers *sticky* | Remove-AppxPackage`

You can ignore any error that pops up. Alternative: [Notebot](http://fdossena.com/?p=stickynotes/i.frag)

### **Maps**

Powershell: `Get-AppxPackage -AllUsers *maps* | Remove-AppxPackage`

Command Prompt:\
`sc delete MapsBroker`\
`sc delete lfsvc`\
`schtasks /Change /TN "\Microsoft\Windows\Maps\MapsUpdateTask" /disable`\
`schtasks /Change /TN "\Microsoft\Windows\Maps\MapsToastTask" /disable`

### **Alarms and Clock**

Powershell:

`Get-AppxPackage -AllUsers *alarms* | Remove-AppxPackage`\
`Get-AppxPackage -AllUsers *people* | Remove-AppxPackage`\
You can ignore any error that pops up.

### **Mail, Calendar, ...**

Powershell:

`Get-AppxPackage -AllUsers *comm* | Remove-AppxPackage`\
`Get-AppxPackage -AllUsers *mess* | Remove-AppxPackage`

You can ignore any error that pops up.

Alternatives: [Thunderbird](https://www.mozilla.org/thunderbird/)

### **OneNote**

Powershell: `Get-AppxPackage -AllUsers *onenote* | Remove-AppxPackage`

### **Photos**

Powershell: `Get-AppxPackage -AllUsers *photo* | Remove-AppxPackage`

Alternatives: [JPEGView](https://sourceforge.net/projects/jpegview/)

### Camera

Powershell: `Get-AppxPackage -AllUsers *camera* | Remove-AppxPackage`

Ignore the errors that pop up

### **Weather, News, ...**

Powershell: `Get-AppxPackage -AllUsers *bing* | Remove-AppxPackage`

### **Calculator**

Powershell: `Get-AppxPackage -AllUsers *calc* | Remove-AppxPackage`\
Alternatives: [SpeedCrunch](http://www.speedcrunch.org/)

### **Sound Recorder**

Powershell: `Get-AppxPackage -AllUsers *soundrec* | Remove-AppxPackage`

Alternatives: ~~Audacity~~ [Tenacity](https://tenacityaudio.org/)

### **Microsoft Edge**

Dosen't work anymore\
Alternatives: [Firefox](http://www.firefox.com/) | [Chromium](http://chromium.woolyss.com/)

### **Contact Support, Get Help**

Command Prompt: `install_wim_tweak /o /c Microsoft-Windows-ContactSupport /r`

Powershell: `Get-AppxPackage -AllUsers *GetHelp* | Remove-AppxPackage`

Additionally, Go to _Start > Settings > Apps > Manage optional features_, and remove Contact Support (if present).

### **Microsoft Quick Assist**

Get-WindowsPackage -Online | Where PackageName -like \*QuickAssist\* | Remove-WindowsPackage -Online -NoRestart\


### **Connect**

Command Prompt: `install_wim_tweak /o /c Microsoft-PPIProjection-Package /r`

### **Your Phone**

Powershell: `Get-AppxPackage -AllUsers *phone* | Remove-AppxPackage`

### **Hello Face**

Powershell:

`Get-WindowsPackage -Online | Where PackageName -like *Hello-Face* | Remove-WindowsPackage -Online -NoRestart`

Command Prompt:

`schtasks /Change /TN "\Microsoft\Windows\HelloFace\FODCleanupTask" /Disable`

### **Edit with 3D Paint/3D Print**

Command Prompt:

`for /f "tokens=1* delims=" %I in (' reg query "HKEY_CLASSES_ROOT\SystemFileAssociations" /s /k /f "3D Edit" ^| find /i "3D Edit" ') do (reg delete "%I" /f )`\
`for /f "tokens=1* delims=" %I in (' reg query "HKEY_CLASSES_ROOT\SystemFileAssociations" /s /k /f "3D Print" ^| find /i "3D Print" ') do (reg delete "%I" /f )`

### Disabling Cortana

With the Anniversary Update, Microsoft hid the option to disable Cortana.\
Warning: Do not attempt to remove the Cortana package using install\_wim\_tweak or the PowerShell, as it will break Windows Search and you will have to reinstall Windows!\
Command Prompt:

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 0 /f`\
`reg add "HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules" /v "{2765E0F4-2918-4A46-B9C9-43CDD8FCBA2B}" /t REG_SZ /d "BlockCortana|Action=Block|Active=TRUE|Dir=Out|App=C:\windows\systemapps\microsoft.windows.cortana_cw5n1h2txyewy\searchui.exe|Name=Search and Cortana application|AppPkgId=S-1-15-2-1861897761-1695161497-2927542615-642690995-327840285-2659745135-2630312742|" /f`\
`reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" /v BingSearchEnabled /t REG_DWORD /d 0 /f`
