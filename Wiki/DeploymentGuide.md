# Deployment Guide

## Azure Portal (https://portal.azure.com/)

### Create Azure Bot
>**Note:** Record the bot name

![DG1.png](images/DG1.png)

### Create bot channel - Microsoft Teams

![DG2.png](images/DG2.png)

* Enable the calling and set webhook

![DG3.png](images/DG3.png)

### Create Key Vault

![DG4.png](images/DG4.png)

* Import the SSL certificate PFX file

![DG5.png](images/DG5.png)

![DG6.png](images/DG6.png)

* Copy certificate thumbprint
>**Note:** Record the certificate thumbprint

![DG7.png](images/DG7.png)

* Set access principles

![DG8.png](images/DG8.png)

### Create Storage Account
>**Note:** Record the storage account name

![DG9.png](images/DG9.png)

* Copy blob service endpoint
>**Note:** Record the blob service endpoint

![DG10.png](images/DG10.png)

* Copy access key
>**Note:** Record the access key

![DG11.png](images/DG11.png)

## Azure AD
### Application registration
>**Note:** Record the application name、app id

![DG12.png](images/DG12.png)

* Add AppSecret and copy value
>**Note:** Record the app secret

![DG13.png](images/DG13.png)

* Add API permissions - Microsoft Graph

![DG14.png](images/DG14.png)

![DG15.png](images/DG15.png)

![DG16.png](images/DG16.png)

* Grant administrator consent

![DG17.png](images/DG17.png)

### Create users
>**Note:** Record the MeetingOrganizer user object id

![DG18.png](images/DG18.png)

![DG19.png](images/DG19.png)

## M365 Admin Center (https://admin.microsoft.com/)
### Grant the user authorization

![DG20.png](images/DG20.png)

## Power Shell (executed as an administrator)
[Please refer to Power Shell](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/main/Wiki/PowerShell.md)

## Teams Admin (https://admin.teams.microsoft.com/)
### Add users to the teams channel

![DG21.png](images/DG21.png)

![DG22.png](images/DG22.png)

![DG23.png](images/DG23.png)

## Teams (https://teams.microsoft.com/)
### Get ChannelId
>**Note:** Record the ChannelId

![DG24.png](images/DG24.png)

## Share Point (https://xxxxxx.sharepoint.com/)
### Get the SiteId
>**Note:** Record the SiteId

![DG25.png](images/DG25.png)

### Create recordings folder (/General/Recordings)

![DG26.png](images/DG26.png)

## Source Code
### Modify \Source\BotService\LocalMedia\ComplianceRecordingBot\ServiceConfiguration.Cloud.cscfg
### Modify \Source\BotService\LocalMedia\ComplianceRecordingBot\ServiceConfiguration.Local.cscfg
### Modify \Source\BotService\LocalMedia\ComplianceRecordingBot\WorkerRole\ComplianceRecordingBot.WorkerRole.dll.config

![DG31.png](images/DG31.png)

## Visual Studio
### Release of Cloud Service (Extended Support)

![DG27.png](images/DG27.png)

![DG28.png](images/DG28.png)

![DG29.png](images/DG29.png)

![DG30.png](images/DG30.png)

## Copy the files into the VM and install them
* VC_redist.x64.exe
* PowerShell-7.2.2-win-x64.msi

### Execute PowerShell
* Install module: 
Install-Module -Name MicrosoftTeams -Force -AllowClobber

* Import Module: 
Import-Module MicrosoftTeams

* Get the module installation path: 
Get-Module -ListAvailable -Name MicrosoftTeams

* Add the module installation path to the system environment variable

* Restart the VM
