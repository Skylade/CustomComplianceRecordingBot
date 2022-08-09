# Power Shell (Run as Administrtor)

## Install module
* Install-Module -Name MicrosoftTeams -Force -AllowClobber

## Import Module
* Import-Module MicrosoftTeams

## Sign in to Teams 
* Connect-Microsoft Teams

## Create an application access policy that contains a list of application IDs
* New-CsApplicationAccessPolicy -Identity {AccessPolicyName} -AppIds {"ApplicationID"} -Description {"Access Policy Description"}
>**Note:** The AppIds parameter is the Application (client) ID of AAD App registrations.

## Grant the strategy to users to allow the user granted by the application ID included in the strategy to access the online meeting on behalf of the granted user (it will take about 30minutes to take effect)
* Grant-CsApplicationAccessPolicy -PolicyName {AccessPolicyName} -Identity {"UserObjectId"}
>**Note:** The Identity parameter is the ObjectId of the AAD User.

## Create an application instance (here you will get the ObjectId of the Application Instance User)
* New-CsOnlineApplicationInstance -UserPrincipalName {UserUPN} -DisplayName {UserDisplayName} -ApplicationId {ApplicationID}
>**Note:** The ApplicationId parameter is the Application (client) ID of the AAD App registrations. 

## Synchronize the application instance from AAD to the proxy provisioning service
* Sync-CsOnlineApplicationInstance -ObjectId {UserObjectId}
>**Note:** The ObjectId parameter value will be obtained by the above establishment instruction.

## Create a compliance recording policy (wait about 1-2 minutes)
* New-CsTeamsComplianceRecordingPolicy -Enabled $true -Description {"Policy Description"} {ComplianceRecordingPolicyName}	

## Assign the User to apply the Policy
* Set-CsTeamsComplianceRecordingPolicy -Identity {ComplianceRecordingPolicyName}  -ComplianceRecordingApplications ` @(New-CsTeamsComplianceRecordingApplication -Parent {ComplianceRecordingPolicyName} -Id {UserObjectId})
>**Note:** The Id parameter is the ObjectId of the application instance.

## Give users a compliance recording policy
* Grant-CsTeamsComplianceRecordingPolicy -Identity {UserUPN} -PolicyName {ComplianceRecordingPolicyName}

## Switch to the source code folder path, execute the PS within the source code, and enter the settings project information
* cd {SourcePath}\CustomComplianceRecordingBot\Source
* .\configure_cloud.ps1
* Path: {SourcePath}\CustomComplianceRecordingBot\Source\BotService\LocalMedia\ComplianceRecordingBot
* Service DNS name: {DNS name}
* Service CName: {CName}
* Certificate thumbprint: {thumbprint}
* Bot Display Name: {Azure Bot Service Name}
* Bot's Microsoft application id: {Azure Bot Service application id}
* Bot's Microsoft application secret: {Azure Bot Service application secret}
