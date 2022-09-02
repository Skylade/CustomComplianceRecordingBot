## Solution Overview

Refer the following image for architecture.

![Overview](images/Architecture-1.png)

The **Custom Compliance Recording Bot** has the following main components:
* **Web API**: Create or Get Teams Online Meeting.
* **Azure Bot**: Call to recording bot.
* **Reaording Bot**: The Azure Cloud Services (extended support) for teams policy-based recording.(https://docs.microsoft.com/en-us/microsoftteams/teams-recording-policy)
* **Downloader App**: The Downloader App will download recording files to local from SharePoint.
* **Microsoft Graph API**: The app leverages Microsoft graph api's to [Create Online  Meeting](https://docs.microsoft.com/en-us/graph/api/application-post-onlinemeetings?view=graph-rest-1.0&tabs=csharp) , [Get Online Meeting](https://docs.microsoft.com/en-us/graph/api/onlinemeeting-get?view=graph-rest-1.0&tabs=http) , [Get User](https://docs.microsoft.com/en-us/graph/api/user-get?view=graph-rest-1.0&tabs=http)

---

## Flow chart

Following description of solution components workflows and how they interact with each other for core scenarios.

![Flow chart](images/Architecture-2.png)

### CreateMeeting Web API

User calls the CreateMeeting API to create a online meeting and set the primary user to the compliance recording policy.

### Azure Bot Service

Teams platform will dial the recording bot through the call settings of this azure bot service so that the bot can anser call to join the meeting.

### Recording Bot

After joining the meeting, bot will subscribe meeting event and automatically start recording, save the recording files after meeting end, and upload recording files to specified SharePoint location.

### Downloader App

Download meeting recording files from specified SharePoint location.

---

## The Graph API used
API Name | Method | Endpoint | Reference 
--- | --- | --- | --- 
Get a user | GET | https://graph.microsoft.com/v1.0/users/{id or userPrincipalName} | [Article](https://docs.microsoft.com/en-us/graph/api/user-get?view=graph-rest-1.0&tabs=http)
Get onlineMeeting | GET | https://graph.microsoft.com/v1.0/users/{userId}/onlineMeetings/{meetingId} | [Article](https://docs.microsoft.com/en-us/graph/api/onlinemeeting-get?view=graph-rest-1.0&tabs=http)
Create onlineMeeting | POST | https://graph.microsoft.com/v1.0/users/{userId}/onlineMeetings | [Article](https://docs.microsoft.com/en-us/graph/api/application-post-onlinemeetings?view=graph-rest-1.0&tabs=http)
Get a site | GET | https://graph.microsoft.com/v1.0/sites/{hostname}:/{server-relative-path} | [Article](https://docs.microsoft.com/en-us/graph/api/site-get?view=graph-rest-1.0&tabs=csharp)
Enumerate lists in a site | GET | https://graph.microsoft.com/v1.0/sites/{site-id}/lists | [Article](https://docs.microsoft.com/en-us/graph/api/list-list?view=graph-rest-1.0&tabs=http)
Enumerate items in a list | GET | https://graph.microsoft.com/v1.0/sites/{site-id}/lists/{list-id}/items | [Article](https://docs.microsoft.com/en-us/graph/api/listitem-list?view=graph-rest-1.0&tabs=http)
Get Drive | GET | https://graph.microsoft.com/v1.0/sites/{siteId}/drive | [Article](https://docs.microsoft.com/en-us/graph/api/drive-get?view=graph-rest-1.0&tabs=http)
Get a driveItem | GET | https://graph.microsoft.com/v1.0/sites/{site-id}/lists/{list-id}/items/{item-id}/driveItem | [Article](https://docs.microsoft.com/en-us/graph/api/driveitem-get?view=graph-rest-1.0&tabs=http)
List children of a driveItem | GET | https://graph.microsoft.com/v1.0/sites/{site-id}/drive/items/{item-id}/children | [Article](https://docs.microsoft.com/en-us/graph/api/driveitem-list-children?view=graph-rest-1.0&tabs=http)
Get a driveItem | GET | https://graph.microsoft.com/v1.0/drives/{drive-id}/items/{item-id} | [Article](https://docs.microsoft.com/en-us/graph/api/driveitem-get?view=graph-rest-1.0&tabs=http)
Delete a DriveItem | DELETE | https://graph.microsoft.com/v1.0/drives/{drive-id}/items/{item-id} | [Article](https://docs.microsoft.com/en-us/graph/api/driveitem-delete?view=graph-rest-1.0&tabs=http)
Delete a DriveItem | DELETE | https://graph.microsoft.com/v1.0/sites/{siteId}/drive/items/{itemId} | [Article](https://docs.microsoft.com/en-us/graph/api/driveitem-delete?view=graph-rest-1.0&tabs=http)