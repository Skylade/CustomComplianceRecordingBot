# CustomComplianceRecordingBot

| [Documentation](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/main/Wiki) | [Architecture](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/main/Wiki/Architecture.md) | [Deployment guide](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/main/Wiki/DeploymentGuide.md) | [Test guide](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/main/Wiki/TestGuide.md) | [Power Shell](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/main/Wiki/PowerShell.md) |
| ---- | ---- | ---- | ---- | ---- |

# Introduction

Custom compliance recording bot for Teams online meeting.

## About

The compliance recording bot sample guides you through building, deploying and testing a bot. This sample demonstrates how a bot can receive media streams for recording.

## Disclaimer

THIS CODE IS PROVIDED AS IS WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING ANY IMPLIED WARRANTIES OF FITNESS FOR A PARTICULAR PURPOSE, MERCHANTABILITY, OR NON-INFRINGEMENT.

## Legal Notice

This compliance recording bot sample is provided under the [MIT License](https://github.com/shawnlien/CustomComplianceRecordingBot/blob/master/LICENSE) terms.  In addition to these terms, by using this compliance recording bot sample you agree to the following:

- You, not Microsoft, will license the use of your app to users or organization. 

- This compliance recording bot sample is not intended to substitute your own regulatory due diligence or make you or your app compliant with respect to any applicable regulations, including but not limited to privacy, healthcare, employment, or financial regulations.

- You are responsible for complying with all applicable privacy and security regulations including those related to use, collection and handling of any personal data by your app. This includes complying with all internal privacy and security policies of your organization if your app is developed to be sideloaded internally within your organization. Where applicable, you may be responsible for data related incidents or data subject requests for data collected through your app.

- Any trademarks or registered trademarks of Microsoft in the United States and/or other countries and logos included in this repository are the property of Microsoft, and the license for this project does not grant you rights to use any Microsoft names, logos or trademarks outside of this repository. Microsoft’s general trademark guidelines can be found [here](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general.aspx).

- If the compliance recording bot sample enables access to any Microsoft Internet-based services (e.g., Office365), use of those services will be subject to the separately-provided terms of use. In such cases, Microsoft may collect telemetry data related to compliance recording bot sample usage and operation. Use and handling of telemetry data will be performed in accordance with such terms of use.

- Use of this compliance recording bot sample does not guarantee acceptance of your app to the Teams app store. To make this app available in the Teams app store, you will have to comply with the [submission and validation process](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/deploy-and-publish/appsource/publish), and all associated requirements such as including your own privacy statement and terms of use for your app.

# Getting Started

This section walks you through the process of deploying and testing the sample bot.

### Bot Registration

1. Follow the steps in [Register Calling Bot](https://microsoftgraph.github.io/microsoft-graph-comms-samples/docs/articles/calls/register-calling-bot.html). Save the bot name, bot app id and bot secret for configuration.

1. Add the following Application Permissions to the bot:

    * Calls.AccessMedia.All
    * Calls.JoinGroupCall.All
   
1. The permission needs to be consented by tenant admin. Go to "https://login.microsoftonline.com/common/adminconsent?client_id=<app_id>&state=<any_number>&redirect_uri=<any_callback_url>" using tenant admin to sign-in, then consent for the whole tenant.

### Create an Application Instance

Open powershell (in admin mode) and run the following commands. When prompted for authentication, login with the tenant admin.
* `> Import-Module SkypeOnlineConnector`
* `> $userCredential = Get-Credential`
* `> $sfbSession = New-CsOnlineSession -Credential $userCredential -Verbose`
* `> Import-PSSession $sfbSession`
* `> New-CsOnlineApplicationInstance -UserPrincipalName <upn@contoso.com> -DisplayName <displayName> -ApplicationId <your_botappId>`
* `> Sync-CsOnlineApplicationInstance -ObjectId <objectId>`

### Create a Compliance Recording Policy
Requires the application instance ID created above. Continue your powershell session and run the following commands.
  * `> New-CsTeamsComplianceRecordingPolicy -Tenant <tenantId> -Enabled $true -Description "Test policy created by <yourName>" <policyIdentity>`
  * ```> Set-CsTeamsComplianceRecordingPolicy -Tenant <tenantId> -Identity <policyIdentity> -ComplianceRecordingApplications ` @(New-CsTeamsComplianceRecordingApplication -Tenant <tenantId> -Parent <policyIdentity> -Id <objectId>)```

After 30-60 seconds, the policy should show up. To verify your policy was created correctly:
  * `> Get-CsTeamsComplianceRecordingPolicy <policyIdentity>`

### Assign the Compliance Recording Policy
Requries the policy identity created above. Contine your powershell session and run the following commands.
  * `> Grant-CsTeamsComplianceRecordingPolicy -Identity <userUnderCompliance@contoso.com> -PolicyName <policyIdentity> -Tenant <tenantId>`

To verify your policy was assigned correctly:
  * `> Get-CsOnlineUser <userUnderCompliance@contoso.com> | ft sipaddress, tenantid, TeamsComplianceRecordingPolicy`

### Prerequisites

* Install the prerequisites:
    * [Visual Studio 2017+](https://visualstudio.microsoft.com/downloads/)
    * [PostMan](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)

### Deploy

1. Create a cloud service (classic) in Azure. Get your "Site URL" from Azure portal, this will be your DNS name and CN name for later configuration, for example: `bot.contoso.com`.

1. Set up SSL certificate and upload to the cloud service
    1. Create a wildcard certificate for your service. This certificate should not be a self-signed certificate. For instance, if your bot is hosted at `bot.contoso.com`, create the certificate for `*.contoso.com`.
    2. Upload the certificate to the cloud service.
    3. Copy the thumbprint for later.

1. Set up cloud service configuration
    1. Open powershell, go to the folder that contains file `configure_cloud.ps1`. The file is in the `Samples` directory.
    2. Run the powershell script with parameters:
        * `> .\configure_cloud.ps1 -p .\Source\LocalMedia\ComplianceRecordingBot\ -dns {your DNS name} -cn {your CN name, should be the same as your DNS name} -thumb {your certificate thumbprint} -bid {your bot name} -aid {your bot app id} -as {your bot secret}`, for example `.\configure_cloud.ps1 -p .\Source\LocalMedia\ComplianceRecordingBot\ -dns bot.contoso.com -cn bot.contoso.com -thumb ABC0000000000000000000000000000000000CBA -bid bot -aid 3853f935-2c6f-43d7-859d-6e8f83b519ae -as 123456!@#$%^`

1. Publish the bot from VS:
    1. Right click ComplianceRecordingBot, then click `Publish...`. Publish it to the cloud service you created earlier.

### Test

1. Set up the test meeting and test clients:
   1. Sign in to Teams client with a non-recorded test tenant user.
   1. Use another Teams client to sign in with the recorded user. (You could use an private browser window at https://teams.microsoft.com)

1. Place a call from the Teams client with the non-recorded user to the recorded user.

1. Your bot should now receive an incoming call, and join the call (See next step for retrieving logs). Use the recorded user's Teams client to accept the call.

1. Interact with your service, _adjusting the service URL appropriately_.
    1. Get diagnostics data from the bot. Open the links in a browser for auto-refresh. Replace the call id 311a0a00-53d9-4a42-aa78-c10a9ae95213 below with your call id from the first response.
       * Active calls: https://bot.contoso.com/calls
       * Service logs: https://bot.contoso.com/logs

    1. By default, the call will be terminated when the recording status has failed. You can terminate the call through `DELETE`, as needed for testing. Replace the call id `311a0a00-53d9-4a42-aa78-c10a9ae95213` below with your call id from the first response.

        ##### Request
        ```json
            DELETE https://bot.contoso.com/calls/311a0a00-53d9-4a42-aa78-c10a9ae95213
        ```
