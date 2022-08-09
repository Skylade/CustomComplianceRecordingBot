## Solution Overview

Refer the following image for architecture.

![Overview](images/Architecture-1.png)

The **Custom Compliance Recording Bot** has the following main components:
* **Web API**: Create or Get Teams Online Meeting.
* **Azure Bot**: Call to recording bot.
* **Reaording Bot**: The Azure Cloud Services (extended support)for teams policy-based recording.(https://docs.microsoft.com/en-us/microsoftteams/teams-recording-policy)
* **Downloader App**: The Azure Key vault stores the secrets, certificates and connection strings. For more information about the data stored, please check [this](Data-stores.md).
* **Microsoft Graph API**: The app leverages Microsoft graph api's to [Create Online  Meeting](https://docs.microsoft.com/en-us/graph/api/application-post-onlinemeetings?view=graph-rest-1.0&tabs=csharp), [Get Online Meeting](https://docs.microsoft.com/en-us/graph/api/onlinemeeting-get?view=graph-rest-1.0&tabs=http), [Get User](https://docs.microsoft.com/en-us/graph/api/user-get?view=graph-rest-1.0&tabs=http).

---

## Flow chart

Following description of solution components workflows and how they interact with each other for core scenarios.

![Flow chart](images/Architecture-2.png)

### CreateMeeting Web API

The user calls the CreateMeeting API to create a meeting and set the primary user to the Compliance Recording Policy.

### Azure Bot Service

The Teams platform will dial the Recording Bot through the call settings of this Azure Bot Service so that the Bot can Anser Call to join the meeting.

### Recording Bot

After joining the meeting, subscribe to the meeting event and automatically start the recording, save the recording files after the meeting end, and upload to the specified SharePoint location.

### Downloader App

Download meeting recording files from specified SharePoint location.

---