# [Sendbird](https://sendbird.com) Chat SDK for Windows

![Platform](https://img.shields.io/badge/platform-Windows-blue.svg)
![Language](https://img.shields.io/badge/language-C%2B%2B-red.svg)
[![Commercial License](https://img.shields.io/badge/license-Commercial-brightgreen.svg)](https://github.com/sendbird/SendBird-Windows/blob/master/LICENSE.md)
![Status](https://img.shields.io/badge/status-beta-yellowgreen.svg)

## Table of contents

  1. [Introduction](#introduction)
  1. [Before getting started](#before-getting-started)
  1. [Getting started](#getting-started)
  1. [Send your first message](#send-your-first-message)
  1. [Appendix](#appendix)
  1. [Changelogs](#changelogs)
  
<br />

## Introduction

Through Chat SDK for Windows, you can efficiently integrate real-time chat into your client app. On the client-side implementation, you can initialize, configure and build the chat with minimal effort. On the server-side, Sendbird ensures reliable infra-management services for your chat within the app. This read.me provides the Chat SDK’s structure, supplementary features, and the installation steps. 

### How it works

It is simple to implement chat in your client app with the Chat SDK: a **user** logs in, sees a **list of channels**, selects or creates an **open channel** or a **group channel**, and, through the use of the **channel event handlers**, sends **messages** to the channel, while also receiving **them from other users** within the channel. 

<br />

## Before getting started

This section shows you the prerequisites you need to know for using Sendbird Chat SDK for Windows.

### Requirements

- Windows 7 (32-bit/64-bit) or higher
- C++03 or higher

<br />

## Getting started

### Try the sample app

The fastest way to test the Chat SDK is to build your chat app on top of our sample app. To create a project for the sample app, download the app from our GitHub repository. The link is down below. 

-  https://github.com/sendbird/SendBird-Windows


### Step 1: Create a Sendbird application from your dashboard

The first thing you need to do is to log in to the [Sendbird Dashboard](https://dashboard.sendbird.com/auth/signin) and create a Sendbird application. If you do not yet have an account, you can log in with Google, GitHub, or create a new account.

You should create one application per service, regardless of the platform. For example, an app released in Android, iOS, and Windows would require only one application to be created on the Sendbird Dashboard.

All users within the same Sendbird application are able to communicate with each other, across all platforms. This means users using iOS, Android, web clients, Windows, etc. can all chat with one another. However, users in different Sendbird applications cannot talk to each other.


### Step 2: Build a new app or integrate Sendbird with an existing app

#### 2-1. Dependencies

This library doesn't have any dependencies. However, if you use boost and `OpenSSL` and there is a symbol conflict with them, contact [Technical Support
](https://help.sendbird.com/hc/en-us/requests/new).

#### 2-2. Set Visual Studio Project

- Set the directory of header files for Sendbird to **Project Properties** > **Configuration Properties** > **VC++ Directories** > **Include Directories**.
- Set the directory of `Sendbird.lib` to **Project Properties** > **Configuration Properties** > **VC++ Directories** > **Library Directories**.
- Set `Sendbird.lib` to **Project Properties** > **Configuration Properties** > **Linker** > **Input** > **Additional Dependencies**.
- ***Every string parameter and variable is `wstring` type and its value is unicode***.

<br /> 

## Send your first message

### Step 1: Initialize with APP_ID

To use the chat features, you must initialize using the `APP_ID` assigned to your Sendbird application. It is recommended that code for initialization be implemented in the user login view controller.

```cpp
#include <Sendbird.h>

SBDMain::Init(APP_ID);
```

### Step 2: Connect to Sendbird server

#### A. Connect with a user ID

Connect a user to Sendbird server by using a unique user ID or with a user ID and an access token. To connect to Sendbird server, a user is required to log in with a unique ID. A new user can authenticate with any untaken user ID, which gets automatically registered to Sendbird system. An existing ID can log in directly. The ID must be unique within a Sendbird application to be distinguished from others, such as an email address or a UID from your database.

This simple authentication procedure might be useful when you are in development or if your service does not require additional security.

```cpp
#include <SendBird.h>

class SendBirdConnectHandler : public SBDConnectInterface {
public:
    SendBirdConnectHandler() {
    }
    
    ~SendBirdConnectHandler() {
    }
    
    void CompletionHandler(SBDUser user, SBDError *error) {
        if (error != NULL) {
            // Error Handlling.
            
            // Deallocate error.
            delete error;
            
            return;
        }
        
        // Connected to Sendbird server.
    }
};

void Connect() {
    SendBirdConnectHandler *handler = new SendBirdConnectHandler(); // `handler` has to be deallocated later.
    SBDMain::Connect(USER_ID, SBD_NULL_WSTRING, handler);
}
```

#### B. Connect with a user ID and access token

With Sendbird [Chat Platform API](https://sendbird.com/docs/chat/v3/platform-api/guides/user#2-create-a-user), you can create a new user with an access token, or you can issue an access token for an existing user. Once an access token is issued, you are required to provide the user's token in the log in method.

```cpp
#include <SendBird.h>

class SendBirdConnectHandler : public SBDConnectInterface {
public:
    SendBirdConnectHandler() {
    }
    
    ~SendBirdConnectHandler() {
    }
    
    void CompletionHandler(SBDUser user, SBDError *error) {
        if (error != NULL) {
            // Error Handlling.
            
            // Deallocate error.
            delete error;
            
            return;
        }
        
        // Connected to SendBird.
    }
};

void Connect() {
    SendBirdConnectHandler *handler = new SendBirdConnectHandler(); // `handler` has to be deallocated later.
    SBDMain::Connect(USER_ID, ACCESS_TOKEN, handler);
}
```

**- Tips for user account security**

From **Settings** > **Application** > **Security** > **Access token permission** setting in your dashboard, you can prevent users without an access token from logging in to your Sendbird application or restrict their access to **read** and **write** messages.

### Step 3: Create a new open channel

### Step 4: Enter the channel


### Step 5: Send a message to the channel


<br />



## Authentication

### Initializing with APP_ID

To use its chat features, you must initialize SendBird using the **APP_ID** assigned to your SendBird application.
Typically, initialization would be implemented in the user login view controller.

```cpp
#include <SendBird.h>

SBDMain::Init(APP_ID);
```

### Connecting with UserID
By default, SendBird requires only a **USER_ID** to join a channel. Upon requesting connection, SendBird will query its user database for a matching USER_ID. If it finds that the UserID has not been registered yet, a new user account will be created. The USER_ID can be any unique string id, such as an email address or a UID from your database.

This simple authentication procedure might be useful when you are in development or if your service does not require additional security.

> Explanation on SendBird's usage of Delegates and callbacks can be found under the **[Event Handler](#event_handler)** section.

```cpp
#include <SendBird.h>

class SendBirdConnectHandler : public SBDConnectInterface {
public:
  SendBirdConnectHandler() {
  }

  ~SendBirdConnectHandler() {
  }

  void CompletionHandler(SBDUser user, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // Connected to SendBird.
  }
};

void Connect() {
  SendBirdConnectHandler *handler = new SendBirdConnectHandler(); // `handler` has to be deallocated later.
  SBDMain::Connect(USER_ID, SBD_NULL_WSTRING, handler);
}
```

### Connecting with UserID and Access Token
With the SendBird [Platform API](/platform), you can create a user with an access token, or you can issue an access token for an existing user. Once an access token is issued, you are required to provide the user's token in the login method.

1. Create a SendBird user account via the Platform API when your user signs up on your service.
2. Save the access token to your secured persistent store.
3. Load the access token in your client and pass it to the SendBird login method.
4. For security reasons, we recommend that you periodically update your access token by issuing a new token to replace the previous one.


> You can set restrictions for users without access tokens in your Dashboard settings.
These settings can be found under **Security - Access Token Policy**.

```cpp
#include <SendBird.h>

class SendBirdConnectHandler : public SBDConnectInterface {
public:
  SendBirdConnectHandler() {
  }

  ~SendBirdConnectHandler() {
  }

  void CompletionHandler(SBDUser user, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // Connected to SendBird.
  }
};

void Connect() {
  SendBirdConnectHandler *handler = new SendBirdConnectHandler(); // `handler` has to be deallocated later.
  SBDMain::Connect(USER_ID, ACCESS_TOKEN, handler);
}
```

### Disconnecting

You should disconnect from SendBird when your user no longer needs to receive messages from an online state.

Disconnecting removes all registered handlers and callbacks. That is, it removes all [Event Handlers](#event-handler) added through `AddReconnectionHandler()` or `AddChannelHandler()` of `SBDMain`. It also flushes all internally cached data, such as the channels that are cached when `GetChannel()` of `SBDOpenChannel` or `GetChannel()` of `SBDGroupChannel` is called.

```cpp
#include <SendBird.h>

class SendBirdDisconnectHandler : public SBDDisconnectInterface {
public:
  SendBirdDisconnectHandler() {

  }

  ~SendBirdDisconnectHandler() {

  }

  void CompletionHandler() {
    
  }
};

void Disconnect() {
  SendBirdDisconnectHandler *handler = new SendBirdDisconnectHandler(); // `handler` has to be deallocated later.
  SBDMain::Disconnect(handler);
}
```

### Updating a User Profile and Profile Image

You can update a user's nickname and profile image.

Call `UpdateCurrentUserInfo()` to update a user's nickname, as well as their profile picture with a URL.

```cpp
#include <SendBird.h>

class SendBirdUpdateUserInfoHandler : public SBDUpdateUserInfoInterface {
public:
  SendBirdUpdateUserInfoHandler() {

  }

  ~SendBirdUpdateUserInfoHandler() {

  }

  void CompletionHandler(SBDError *error) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void UpdateCurrentUserInfo() {
  SendBirdUpdateUserInfoHandler *handler = new SendBirdUpdateUserInfoHandler(); // `handler` has to be deallocated later.
  SBDMain::UpdateCurrentUserInfo(NEW_NICKNAME, NEW_PROFILE_IMAGE_URL, handler);
}
```

Or, you can pass in an image file directly.

```cpp
#include <SendBird.h>

class SendBirdUpdateUserInfoHandler : public SBDUpdateUserInfoInterface {
public:
  SendBirdUpdateUserInfoHandler() {

  }

  ~SendBirdUpdateUserInfoHandler() {

  }

  void CompletionHandler(SBDError *error) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void UpdateCurrentUserInfo() {
  SendBirdUpdateUserInfoHandler *handler = new SendBirdUpdateUserInfoHandler(); // `handler` has to be deallocated later.
  SBDMain::UpdateCurrentUserInfoWithBinaryProfileImage(NEW_NICKNAME, NEW_PROFILE_IMAGE_FILE_PATH, FILE_MIME_TYPE, handler);
}
```

## Channel Types
You should understand the following terminology before proceeding with the rest of this guide.


### Open Channel
An **Open Channel** is a public chat. In this channel type, anyone can enter and participate in the chat without permission. A single channel can handle thousands of simultaneous users. i.e.) **a Twitch-style public chat**.


### Group Channel
A **Group Channel** is a private chat. A user may join the chat only through an invitation by another user who is already a member of the chatroom.

>* ** Distinct property ** : A channel with the **Distinct** property enabled will always be reused for the same members. If a new member is invited, or if a member leaves the channel, then the Distinct property is disabled automatically.

>* **1-on-1 messaging** : 1-on-1 messaging is a private channel between two users. You can enable the Distinct property for the channel in order to reuse a channel for the same members. i.e.) **Twitter Direct Messages-style 1-on-1 chatting**

>* **Group messaging**  : Group messaging is a private channel among multiple users. You can invite up to hundreds of members into a group channel. i.e.) **a WhatsApp-style closed group chat**

#### Open vs. Group Channels
|Type|Open Channel|Group Channel|
| --- | --- | --- |
| Access Control | Public | Invitation required |
| Class Name | OpenChannel | GroupChannel |
| Number of Members | Over a few thousand | Less than a few hundred |
| How to Create | SendBird Dashboard / Platform API / Client SDK | Client SDK / Platform API|
| Operators | Supported | N/A |
| User Ban | Supported | N/A |
| User Mute | Supported | N/A |
| Freeze Channel | Supported | N/A |
| Push Notifications| N/A | Supported |
| Unread Counts | N/A | Supported |
| Read Receipts | N/A | Supported |
| Typing Indicators | N/A | Supported |


## Open Channel
An **Open Channel** is a public chat. In this channel type, anyone can enter and participate in the chat without permission. A single channel can handle thousands of simultaneous users. i.e.) **a Twitch-style public chat**


### Creating an Open Channel
You can create an Open Channel from the **[SendBird Dashboard](https://dashboard.sendbird.com)**.

To create a channel, you must specify a **Channel URL**, which is a unique identifier. Additionally, you can set a **Channel Topic**, the name of the channel.

An open channel is ideal for use cases that require a small and static number of channels - for example, when you have a central "Lobby Chat" within a game.

You can also create a channel via the SDK or the SendBird [Platform API](/platform). You should do so when your channel needs to be created on demand or dynamically.

```cpp
#include <SendBird.h>

class SendBirdCreateOpenChannelHandler : public SBDCreateOpenChannelInterface {
public:
  SendBirdCreateOpenChannelHandler() {
  }

  ~SendBirdCreateOpenChannelHandler() {
  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The open channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateOpenChannel() {
  SendBirdCreateOpenChannelHandler *handler = new SendBirdCreateOpenChannelHandler(); // `handler` has to be deallocated later.

  // Every wstring type parameter is an option. If you don't have to set them, set `SBD_NULL_WSTRING`.
  // OPERATOR_USER_IDS is vector<wstring> type. If you don't have to set it, set vector<wstring>().
  SBDOpenChannel::CreateChannel(CHANNEL_NAME, CHANNEL_URL, COVER_URL, DATA, OPERATOR_USER_IDS, CUSTOM_TYPE, handler);
}
```

### Getting a list of Open Channels
You can obtain a list of Open Channels by creating a `SBDOpenChannelListQuery`.
`LoadNextPage()` returns a list of `SBDOpenChannel` objects.

> You must be connected to SendBird before requesting an Open Channel List.

```cpp
#include <SendBird.h>

SBDOpenChannelListQuery *query;

class SendBirdOpenChannelListQueryHandler : public SBDLoadNextOpenChannelListInterface {
public:
  SendBirdOpenChannelListQueryHandler() {

  }

  ~SendBirdOpenChannelListQueryHandler() {

  }

  void CompletionHandler(vector<SBDOpenChannel *> channels, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel items in channels vector.
  }
}

void InitQueryInstance() {
  query = SBDOpenChannel::CreateOpenChannelListQuery();
  query->limit = 30;
  query->SetChannelUrlFilter(CHANNEL_URL);
  query->SetChannelNameFilter(CHANNEL_NAME);
  query->SetCustomTypeFilter(CUSTOM_TYPE);
}

void GetOpenChannels() {
  SendBirdOpenChannelListQueryHandler *handler = new SendBirdOpenChannelListQueryHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

### Getting an Open Channel instance with a URL

Since a **channel URL** is a unique identifier of an Open Channel, you can use a URL to retrieve a channel instance. It is important to remember that a user must [enter the channel](#entering-an-open-channel) before being able to send or receive messages within it.

Store channel URLs to handle lifecycle or state changes in your app. For example, if a user disconnects from SendBird by temporarily switching to another app, you can provide a smooth restoration of the user's state using a stored URL to fetch the appropriate channel instance, then re-entering the user into the channel.

```cpp
#include <SendBird.h>

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

### Entering an Open Channel
A user must enter an Open Channel in order to receive messages.

You can enter up to 10 Open Channels at once.

Entered Open Channels are valid only within the current connection. If you disconnect or reconnect to SendBird, you must re-enter channels in order to continue receiving messages.

```cpp
#include <SendBird.h>

class SendBirdEnterOpenChannelHandler : public SBDEnterOpenChannelInterface {
public:
  SendBirdEnterOpenChannelHandler() {

  }

  ~SendBirdEnterOpenChannelHandler() {

  }

  void CompletionHandler(SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void EnterOpenChannel() {
  SendBirdEnterOpenChannelHandler *handler = new SendBirdEnterOpenChannelHandler(); // `handler` has to be deallocated later.
  open_channel->Enter(handler);
}
```

### Exiting an Open Channel
To stop receiving messages from an Open Channel, you must exit the channel.

```cpp
#include <SendBird.h>

class SendBirdExitOpenChannelHandler : public SBDExitOpenChannelInterface {
public:
  SendBirdExitOpenChannelHandler() {

  }

  ~SendBirdExitOpenChannelHandler() {

  }

  void CompletionHandler(SBDError *error) {
    // Error Handlling.

    // Deallocate error.
    delete error;

    return;
  }
};

void ExitOpenChannel() {
  SendBirdExitOpenChannelHandler *handler = new SendBirdExitOpenChannelHandler(); // `handler` has to be deallocated later.
  open_channel->Exit(handler);
}
```

### Sending messages
Upon entering a channel, a user will be able to send messages of the following types:
* **UserMessage** : a User text message.
* **FileMessage** : a User binary message.

You can additionally specify a `CUSTOM_TYPE` to further subclassify a message.

When you send a text message, you can additionally attach arbitrary strings via a `DATA` field. You can utilize this field to send structured data such as font sizes, font types, or custom JSON objects.

Delivery failures (e.g., due to network issues) will return an exception. By implementing the virtual method, `CompletionHandler()`, it is possible to display only the messages that are successfully sent.

```cpp
#include <SendBird.h>

class SendBirdSendUserMessageHandler : public SBDSendUserMessageInterface {
public:
  SendBirdSendUserMessageHandler() {
  }

  ~SendBirdSendUserMessageHandler() {
  }

  void CompletionHandler(SBDUserMessage *user_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `user_message`.
  }
};


void SendUserMessage() {
  SendBirdSendUserMessageHandler *handler = new SendBirdSendUserMessageHandler(); // `handler` has to be deallocated later.

  // `SendUserMessage()` belongs to `SBDBaseChannel` class, so it can be used by `SBDOpenChannel` and `SBDGroupChannel` instance.
  // TARGET_LANGUAGES is vector<wstring> type. If you don't have to set it, set vector<wstring>().
  channel->SendUserMessage(MESSAGE_TEXT, DATA, CUSTOM_TYPE, TARGET_LANGUAGES, handler);
}
```

A user can also send any binary file through SendBird. There are two ways in which you can send a binary file: by sending a **URL**.

By sending a raw file, you can choose to send a file hosted in your own servers by passing in a URL that points to the file. In this case, your file will not be hosted in the SendBird servers, and downloads of the file will occur through your own servers instead.


```cpp
#include <SendBird.h>

class SendBirdSendFileMessageHandler : public SBDSendFileMessageInterface {
public:
  SendBirdSendFileMessageHandler() {

  }

  ~SendBirdSendFileMessageHandler() {

  }

  void CompletionHandler(SBDFileMessage *file_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `file_message`.
  }
};

void SendFileMessage() {
  SendBirdSendFileMessageHandler *handler = new SendBirdSendFileMessageHandler(); // `handler` has to be deallocated later.
  channel->SendFileMessage(FILE_URL, FILE_NAME, FILE_SIZE, FILE_TYPE, CUSTOM_DATA, CUSTOM_TYPE, handler);
}
```

### Receiving messages
Messages can be received by adding a **[SBDChannelInterface](#event-handler)**.
A received `SBDBaseMessage` object can be of one of three different types of messages.

* **SBDUserMessage** : a User text message.
* **SBDFileMessage** : a User binary message.
* **SBDAdminMessage** : an Admin message which can be sent by an admin through the Platform API.

`UNIQUE_HANDLER_ID` is a unique identifier to register multiple concurrent handlers.


```cpp
#include <SendBird.h>

class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void MessageReceived(SBDBaseChannel *channel, SBDBaseMessage *message) {

  }

  // ...
};

void InitChannelEventHandler() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_HANDLER_ID);
}
```

You should remove the channel delegate where the UI is no longer valid.

```cpp
#include <SendBird.h>

void RemoveChannelHandler() {
  SBDMain::RemoveChannelHandler(UNIQUE_HANDLER_ID);
}
```

### Loading previous messages
You can load previous messages by creating a `SBDPreviousMessageListQuery` instance.
You will be able to display past messages in your UI once they have loaded.

```cpp
#include <SendBird.h>

SBDPreviousMessageListQuery *query;

class SendBirdLoadPreviousMessageListHandler : public SBDLoadPreviousMessageListInterface {
public:
  SendBirdLoadPreviousMessageListHandler() {

  }

  ~SendBirdLoadPreviousMessageListHandler() {

  }

  void CompletionHandler(vector<SBDBaseMessage *> messages, SBDError *error) {
    // Error Handlling.

    // Deallocate error.
    delete error;

    return;
  }
};

void InitQuery() {
  query = channel->CreatePreviousMessageQuery();
}

void GetPreviousMessages() {
  SendBirdLoadPreviousMessageListHandler *handler = new SendBirdLoadPreviousMessageListHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(30, false, handler);
}
```

Past messages are queried in fixed numbers (30 in the above code). A new `SBDPreviousMessageListQuery` instance will load the most recent `n` messages. Calling `LoadNextPage()` on the same query instance will load `n` messages before that. Therefore, you should store your query instance as a member variable in order to traverse through your entire message history.

> An important note is that you must receive your first `CompletionHandler()` callback before invoking `LoadNextPage()` again.

### Loading messages by timestamp
You can retrieve a set number of messages starting from a specific timestamp.

To load messages sent prior to a specifed timestamp, use `GetPreviousMessagesByTimestamp()` of the channel instance.

```cpp
#include <SendBird.h>

int64_t timestamp = INT64_MAX;

class SendBirdGetPreviousMessagesHandler : public SBDGetMessagesInterface {
public:
  SendBirdGetPreviousMessagesHandler() {

  }

  ~SendBirdGetPreviousMessagesHandler() {

  }
  
  void CompletionHandler(vector<SBDBaseMessage *> messages, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void GetPreviousMessage() {
  SendBirdGetPreviousMessagesHandler *handler = new SendBirdGetPreviousMessagesHandler(); // `handler` has to be deallocated later.
  channel->GetPreviousMessagesByTimestamp(timestamp, limit, reverse, messageType, customType, handler);
}
```

> * `timestamp` : The reference timestamp.
> * `limit` : The number of messages to load. Note that the actual number of results may be larger than the set value when there are multiple messages with the same timestamp as the earliest message.
> * `reverse` : Whether to reverse the results.
> * `messageType` : A [`SBDMessageTypeFilter`] enum type. Should be one of `SBDMessageTypeFilterUser`, `SBDMessageTypeFilterFile`, `SBDMessageTypeFilterAdmin`, or `SBDMessageTypeFilterAll`.
> * `customType` : The Custom Type of the messages to be returned.

To load messages sent after a specified timestamp, call [`GetNextMessagesByTimestamp()`] in a similar fashion. To load results on either side of the reference timestamp, use [`GetMessagesByTimestamp()`].

### Getting a list of participants in a channel
Participants are online users who are currently receiving all messages from the Open Channel.

```cpp
#include <SendBird.h>

SBDUserListQuery *query;

class SendBirdLoadNextParticipantsHandler : public SBDLoadNextUserListInterface {
public:
  SendBirdLoadNextParticipantsHandler() {

  }

  ~SendBirdLoadNextParticipantsHandler() {

  }

  void CompletionHandler(vector<SBDUser> users, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void InitQuery() {
  query = open_channel->CreateParticipantListQuery();
  query->limit = 30;
}

void GetParticipants() {
  SendBirdLoadNextParticipantsHandler *handler = new SendBirdLoadNextParticipantsHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

### Getting participants' online statuses
To stay updated on each participant's connection status, you must obtain a new `SBDUserListQuery`, which contains the latest information on each user. To get a `SBDUserListQuery` for a specific channel, call `CreateOpenChannelListQuery()` of `SBDOpenChannel`. If you wish to get the list of all users of your service (application), call `CreateUserListQuery(USER_IDS)` of `SBDMain`.

You can then check each of the users' connection statuses by referencing `connection_status` of `SBDUser`.

> If your application needs to keep track of users' connection statuses in real time, we recommend that you receive a new `SBDUserListQuery` periodically, perhaps in intervals of one minute or more.

`connection_status` can return one of three values:

* `SBDUserConnectionStatusNotAvailable` : User's status information cannot be reached.
* `SBDUserConnectionStatusOffline` : User is disconnected from SendBird.
* `SBDUserConnectionStatusOnline` : User is connected to SendBird.

### Getting a list of banned or muted users in a channel
You can also create a query to get a list of muted or banned users in an Open Channel.

> This query is only available for users who are registered as operators of the Open Channel.

```cpp
#include <SendBird.h>

SBDUserListQuery *query;

class SendBirdLoadNextBannedUserList : public SBDLoadNextUserListInterface {
public:
  SendBirdLoadNextBannedUserList() {

  }

  ~SendBirdLoadNextBannedUserList() {

  }

  void CompletionHandler(vector<SBDUser> users, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void InitQuery() {
  query = open_channel->CreateMutedUserListQuery();
  query->limit = 30;
}

void GetBannedUsers() {
  SendBirdLoadNextBannedUserList *handler = new SendBirdLoadNextBannedUserList(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

```cpp
#include <SendBird.h>

SBDUserListQuery *query;

class SendBirdLoadNextMutedUserList : public SBDLoadNextUserListInterface {
public:
  SendBirdLoadNextMutedUserList() {

  }

  ~SendBirdLoadNextMutedUserList() {

  }

  void CompletionHandler(vector<SBDUser> users, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void InitQuery() {
  query = open_channel->CreateMutedUserListQuery();
  query->limit = 30;
}

void GetBannedUsers() {
  SendBirdLoadNextMutedUserList *handler = new SendBirdLoadNextMutedUserList(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

### Deleting messages
Users are able to delete messages. An error is returned if a user tries to delete messages sent by someone else.
Channel Operators are able to delete any message in the channel, including those by other users.

Deleting a message fires a `MessageDeleted` event to all other users in the channel.

```cpp
#include <SendBird.h>

class SendBirdDeleteMessageHandler : public SBDDeleteMessageInterface {
public:
  SendBirdDeleteMessageHandler() {

  }

  ~SendBirdDeleteMessageHandler() {

  }

  void CompletionHandler(SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void DeleteMessage() {
  SendBirdDeleteMessageHandler *handler = new SendBirdDeleteMessageHandler(); // `handler` has to be deallocated later.
  channel->DeleteMessage(USER_MESSAGE, handler);
}
```

You can receive a `MessageDeleted()` event using a Channel Interface.

```cpp
class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void MessageDeleted(SBDBaseChannel *channel, uint64_t message_id) {

  }

  // ...
};

void InitSendBird() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_CHANNEL_HANDLER_IDENTIFIER);
}
```

## Open Channel - Advanced

### Admin messages

You can send Admin messages to users in a channel using the [SendBird Dashboard](https://dashboard.sendbird.com) or the [Platform API](/platform).

To do so using the Dashboard, navigate to the **Open Channels** tab. Inside the message box, you should see an option to send an Admin message. Admin messages should not be longer than 1000 characters.

> If you are currently developing under the **Free Plan** and therefore cannot access the **Moderation Tools** from the Dashboard, you must send Admin messages through the Platform API.


### Channel cover images

When creating a channel, you can add a cover image by specifying an image URL.

```cpp
#include <SendBird.h>

class SendBirdCreateOpenChannelHandler : public SBDCreateOpenChannelInterface {
public:
  SendBirdCreateOpenChannelHandler() {
  }

  ~SendBirdCreateOpenChannelHandler() {
  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The open channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateOpenChannel() {
  SendBirdCreateOpenChannelHandler *handler = new SendBirdCreateOpenChannelHandler(); // `handler` has to be deallocated later.

  SBDOpenChannel::CreateChannel(SBD_NULL_WSTRING, SBD_NULL_WSTRING, COVER_URL, SBD_NULL_WSTRING, vector<wstring>(), SBD_NULL_WSTRING, handler);
}
```

You can get the cover image URL using `cover_url`. You can also update a channel's cover image by calling `UpdateChannel()`.

### Custom channel types

When creating a channel, you can additionally specify a **Custom Type** to further subclassify your channels. This custom type takes on the form of a `wstring`, and can be handy in searching or filtering channels.

> `DATA` and `CUSTOM_TYPE` are both String fields that allow you to append information to your channels. The intended use case is for `CUSTOM_TYPE` to contain information that can subclassify the channel (e.g., distinguishing "School" and "Work" channels). However, both these fields can be flexibly utilized.

```cpp
#include <SendBird.h>

class SendBirdCreateOpenChannelHandler : public SBDCreateOpenChannelInterface {
public:
  SendBirdCreateOpenChannelHandler() {
  }

  ~SendBirdCreateOpenChannelHandler() {
  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The open channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateOpenChannel() {
  SendBirdCreateOpenChannelHandler *handler = new SendBirdCreateOpenChannelHandler(); // `handler` has to be deallocated later.

  SBDOpenChannel::CreateChannel(SBD_NULL_WSTRING, SBD_NULL_WSTRING, SBD_NULL_WSTRING, DATA, vector<wstring>(), CUSTOM_TYPE, handler);
}
```

To get a channel's Custom Type, read `channel->custom_type`.

### Custom message types

Likewise, you can specify a **Custom Type** for messages in order to categorize them into more specific groups. This custom type takes on the form of a `wstring`, and can be useful in searching or filtering messages.

> `DATA` and `CUSTOM_TYPE` are both String fields that allow you to append information to your messages. The intended use case is for `CUSTOM_TYPE` to contain information that can subclassify the message (e.g., distinguishing "FILE_IMAGE" and "FILE_AUDIO" type messages). However, both these fields can be flexibly utilized.

To embed a Custom Type into a message, simply pass a String parameter to `SendUserMessage()` or `SendFileMessage()`.

```cpp
#include <SendBird.h>

class SendBirdSendUserMessageHandler : public SBDSendUserMessageInterface {
public:
  SendBirdSendUserMessageHandler() {
  }

  ~SendBirdSendUserMessageHandler() {
  }

  void CompletionHandler(SBDUserMessage *user_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `user_message`.
  }
};


void SendUserMessage() {
  SendBirdSendUserMessageHandler *handler = new SendBirdSendUserMessageHandler(); // `handler` has to be deallocated later.

  // `SendUserMessage()` belongs to `SBDBaseChannel` class, so it can be used by `SBDOpenChannel` and `SBDGroupChannel` instance.
  // TARGET_LANGUAGES is vector<wstring> type. If you don't have to set it, set vector<wstring>().
  channel->SendUserMessage(MESSAGE_TEXT, DATA, CUSTOM_TYPE, vector<wstring>(), handler);
}
```

To get a message's Custom Type, read `message->custom_type`.

### Message auto-translation

> This feature is not available under the Free plan. Contact [sales@sendbird.com](mailto:sales@sendbird.com) if you wish to implement this functionality.

SendBird makes it possible for messages to be sent in different languages through its auto-translation feature.
Pass in a `vector` of language codes to SendUserMessage()` to request translated messages in the corresponding languages.

```cpp
#include <SendBird.h>

class SendBirdSendUserMessageHandler : public SBDSendUserMessageInterface {
public:
  SendBirdSendUserMessageHandler() {
  }

  ~SendBirdSendUserMessageHandler() {
  }

  void CompletionHandler(SBDUserMessage *user_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `user_message`.
  }
};


void SendUserMessage() {
  SendBirdSendUserMessageHandler *handler = new SendBirdSendUserMessageHandler(); // `handler` has to be deallocated later.

  // `SendUserMessage()` belongs to `SBDBaseChannel` class, so it can be used by `SBDOpenChannel` and `SBDGroupChannel` instance.

  vector<wstring> target_langs;

  target_langs.push_back(L"es");
  target_langs.push_back(L"ko");

  channel->SendUserMessage(MESSAGE_TEXT, DATA, CUSTOM_TYPE, target_langs, handler);
}
```

You can obtain translations of a message using `user_message->translations`. This method returns a `map<wstring, wstring>` containing the language codes and translations.

```cpp
class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void MessageReceived(SBDBaseChannel *channel, SBDBaseMessage *message) {
    vector<wstring> translations = (SBDUserMessage *)message;
    wstring es_translation = translations[L"es"];

    // Display translation in UI.
  }

  // ...
};
```

> Reference the table below for supported languages and their language codes.

| Language Code | English Name        | Language Code | English Name       |
|---------------|---------------------|---------------|--------------------|
| af            | Afrikaans           | tlh-Qaak      | Klingon (pIqaD)    |
| ar            | Arabic              | ko            | Korean             |
| bs-Latn       | Bosnian (Latin)     | lv            | Latvian            |
| bg            | Bulgarian           | lt            | Lithuanian         |
| ca            | Catalan             | ms            | Malay              |
| zh-CHS        | Chinese Simplified  | mt            | Maltese            |
| zh-CHT        | Chinese Traditional | no            | Norwegian          |
| hr            | Croatian            | fa            | Persian            |
| cs            | Czech               | pl            | Polish             |
| da            | Danish              | pt            | Portuguese         |
| nl            | Dutch               | otq           | Querétaro Otomi    |
| en            | English             | ro            | Romanian           |
| et            | Estonian            | ru            | Russian            |
| fi            | Finnish             | sr-Cyrl       | Serbian (Cyrillic) |
| fr            | French              | sr-Latn       | Serbian (Latin)    |
| de            | German              | sk            | Slovak             |
| el            | Greek               | sl            | Slovenian          |
| ht            | Haitian Creole      | es            | Spanish            |
| he            | Hebrew              | sv            | Swedish            |
| hi            | Hindi               | th            | Thai               |
| mww           | Hmong Daw           | tr            | Turkish            |
| hu            | Hungarian           | uk            | Ukrainian          |
| id            | Indonesian          | ur            | Urdu               |
| it            | Italian             | vi            | Vietnamese         |
| ja            | Japanese            | cy            | Welsh              |
| sw            | Kiswahili           | yua           | Yucatec Maya       |
| tlh           | Klingon             |     -         |          -         |


### Keyword search

You can search for specific channels by adding a keyword to your `SBDOpenChannelListQuery`.
There are two types of keywords: a **Name Keyword** and a **URL Keyword**.

Adding a Name Keyword to a query will return the list of Open Channels that have the keyword included in their names.

```cpp
#include <SendBird.h>

SBDOpenChannelListQuery *query;

class SendBirdOpenChannelListQueryHandler : public SBDLoadNextOpenChannelListInterface {
public:
  SendBirdOpenChannelListQueryHandler() {

  }

  ~SendBirdOpenChannelListQueryHandler() {

  }

  void CompletionHandler(vector<SBDOpenChannel *> channels, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel items in channels vector.
    // Returns a List of channels that have "NameKeyword" in their names.
  }
}

void InitQueryInstance() {
  query = SBDOpenChannel::CreateOpenChannelListQuery();
  query->limit = 30;
  query->SetChannelNameFilter(CHANNEL_NAME);
}

void GetOpenChannels() {
  SendBirdOpenChannelListQueryHandler *handler = new SendBirdOpenChannelListQueryHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

Adding a URL Keyword to a query will return the Open Channel whose URL **exactly** matches the given keyword.

```cpp
#include <SendBird.h>

SBDOpenChannelListQuery *query;

class SendBirdOpenChannelListQueryHandler : public SBDLoadNextOpenChannelListInterface {
public:
  SendBirdOpenChannelListQueryHandler() {

  }

  ~SendBirdOpenChannelListQueryHandler() {

  }

  void CompletionHandler(vector<SBDOpenChannel *> channels, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel items in channels vector.
    // Returns a List containing a single channel with the URL that matches the URL Keyword.
  }
}

void InitQueryInstance() {
  query = SBDOpenChannel::CreateOpenChannelListQuery();
  query->limit = 30;
  query->SetChannelUrlFilter(CHANNEL_URL);
}

void GetOpenChannels() {
  SendBirdOpenChannelListQueryHandler *handler = new SendBirdOpenChannelListQueryHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

### File Message thumbnails

> This feature is not available under the Free plan. Contact [sales@sendbird.com](mailto:sales@sendbird.com) if you wish to implement this functionality.

When sending an image file, you can choose to create thumbnails of the image, which you can fetch and render into your UI. You can specify up to **3** different dimensions to generate thumbnail images in, which can be convenient for supporting various display densities.

> Supported file types are files whose **MIME type** is `image/*` or `video/*`.

> The SDK does not support creating thumbnails when [sending a File Message via a file URL](#open_channel_3_sending_messages).

Create a `vector` of `SBDThumbnailSize` objects to pass to `SendFileMessageWithPath()`. A `SBDThumbnailSize` can be created with the constructor `SBDThumbnailSize()`, where the values specify **pixels**. The `completion_handler` callback of `SBDSendFileMessageInterface` will subsequently return a `vector` of `SBDThumbnail` objects in the file message object that each contain the URL of the generated thumbnail image file.

```cpp
#include <SendBird.h>

class SendBirdSendFileMessageHandler : public SBDSendFileMessageInterface {
public:
  SendBirdSendFileMessageHandler() {

  }

  ~SendBirdSendFileMessageHandler() {

  }

  void CompletionHandler(SBDFileMessage *file_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // file_message->thumbnails has the thumbnails' information.
  }
};

void SendFileMessage() {
  vector<SBDThumbnailSize> thumbnail_sizes;
  thumbnail_sizes.push_back(SBDThumbnailSize(320, 320));
  thumbnail_sizes.push_back(SBDThumbnailSize(160, 160));

  SendBirdSendFileMessageHandler *handler = new SendBirdSendFileMessageHandler(); // `handler` has to be deallocated later.
  channel->SendFileMessageWithPath(FILE_PATH, FILE_MIME_TYPE, thumbnail_sizes, DATA, CUSTOM_TYPE, handler));		
}
```

`max_width` and `max_height` specify the maximum dimensions of the thumbnail. Your image will be scaled down evenly to fit within the bounds of (`max_width`, `max_height`). Note that if the original image is smaller than the specified dimensions, the thumbnail will not be scaled. `GetUrl()` returns the location of the generated thumbnail file within the SendBird servers.


## Group Channel

A **Group Channel** is a private chat. A user may join the chat only through an invitation by another user who is already a member of the chatroom. A Group Channel can consist of one to hundreds of members. Creating a channel with two members allows 1-to-1 messaging.

A user will automatically receive all messages from the group channels that they are a member of.

### Creating a Group Channel

A Group Channel can be created on demand by a user through the **SendBird SDK**.

> ** Distinct property ** : A channel with the Distinct property enabled will always be reused for the same members. If a new member is invited, or if a member leaves the channel, then the distinct property is disabled automatically. For example, in the case that a Group Channel with 3 members, **A**, **B**, and **C**, already exists, attempting to create a new channel with the same members will just return a reference to the existing channel.

> Consequently, we recommend that you enable the Distinct property in 1-to-1 messaging channels in order to reuse the same channel when a user chooses to directly message a friend. If the property is disabled, the user will create a new channel even if they have had previous conversations with the friend, and therefore will not be able to see or access previously sent messages or data.

```cpp
#include <SendBird.h>

class SendBirdCreateGroupChannelHandler : public SBDCreateGroupChannelInterface {
public:
  SendBirdCreateGroupChannelHandler() {
  }

  ~SendBirdCreateGroupChannelHandler() {
  }

  void CompletionHandler(SBDGroupChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The group channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateGroupChannel() {
  SendBirdCreateGroupChannelHandler *handler = new SendBirdCreateGroupChannelHandler(); // `handler` has to be deallocated later.

  // Every wstring type parameter is an option. If you don't have to set them, set `SBD_NULL_WSTRING`.
  // INVITEE_IDS is vector<wstring> type. If you don't have to set it, set vector<wstring>().
  // IS_DISTINCT is bool type.
  SBDGroupChannel::CreateChannel(INVITEE_IDS, NAME, IS_DISTINCT, COVER_IMAGE_URL, DATA, CUSTOM_TYPE, handler);
}
```

* `NAME` : the name of the channel, or the Channel Topic.
* `COVER_IMAGE_URL` : the URL of the cover image, which you can fetch to render into the UI.
* `DATA` : a `wstring` field to store structured information, such as a JSON String.
* `CUSTOM_TYPE` : a `wstring` field that allows you to subclassify your channel.

> See the [Advanced](#group_channel_advanced) section for more information on cover images and Custom Types.

You can also create channels via the SendBird [Platform API](/platform).
You should utilize the Platform API when you wish to control channel creations and member invitations on the server-side.

### Getting a Group Channel instance with a URL

Since a **channel URL** is a unique identifier of a Group Channel, you can use a URL to retrieve a channel instance.

Store channel URLs to handle lifecycle or state changes in your app. For example, if a user disconnects from SendBird by temporarily switching to another app, you can provide a smooth restoration of the user's state using a stored URL to fetch the appropriate channel instance, then re-entering the user into the channel.

```cpp
class SendBirdGetGroupChannelHandler : public SBDGetGroupChannelInterface {
public:
  SendBirdGetGroupChannelHandler() {

  }

  ~SendBirdGetGroupChannelHandler() {

  }

  void CompletionHandler(SBDGroupChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void CreateGroupChannel() {
  SendBirdGetGroupChannelHandler *handler = new SendBirdGetGroupChannelHandler(); // `handler` has to be deallocated later.
  SBDGroupChannel::GetChannel(CHANNEL_URL, handler);
}
```

### Inviting users to an existing channel
Only members of the channel are able to invite new users into the channel.

You can choose whether the newly invited user is able to access past messages in the channel. In your **Dashboard Settings - Messages** section, there is an option to **show channel history**. If this option is enabled, new users will be able to view all messages sent before they have joined the channel. If not, new users will only be able to see messages sent after they had been invited.

> **Show channel history** is enabled by default

```cpp
class SendBirdInviteUsersHandler : public SBDInviteUsersInterface {
public:
  SendBirdInviteUsersHandler() {

  }

  ~SendBirdInviteUsersHandler() {

  }

  void CompletionHandler(SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void InviteUsers() {
  vector<SBDUser> invitees;

  invitees.push_back(user_a);
  invitees.push_back(user_b);

  SendBirdInviteUsersHandler *handler = new SendBirdInviteUsersHandler(); // `handler` has to be deallocated later.
  group_channel->InviteUsers(invitees, handler);
}
```

### Leaving a Group Channel
Users will no longer receive messages from channels they have left.

```cpp
class SendBirdLeaveGroupChannelHandler : public SBDLeaveGroupChannelInterface {
public:
  SendBirdLeaveGroupChannelHandler() {

  }

  ~SendBirdLeaveGroupChannelHandler() {

  }

  void CompletionHandler(SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void LeaveGroupChannel() {
  SendBirdLeaveGroupChannelHandler *handler = new SendBirdLeaveGroupChannelHandler(); // `handler` has to be deallocated later.
  group_channel->LeaveChannel();
}
```

### Getting a list of my Group Channels
You can obtain a list of Group Channels by creating a query with `CreateMyGroupChannelListQuery()` of `SBDGroupChannel`.
`LoadNextPage` returns a list of `SBDGroupChannel` objects.

> You can also set an option to include empty channels with `include_empty_channel`. **Empty channels** are channels that have been created but contain no sent messages. By default, empty channels are not shown.

```cpp
SBDGroupChannelListQuery *query;

class SendBirdMyGroupChannelListQueryHandler : public SBDLoadNextGroupChannelListInterface {
public:
  SendBirdMyGroupChannelListQueryHandler() {

  }

  ~SendBirdMyGroupChannelListQueryHandler() {

  }

  void CompletionHandler(vector<SBDGroupChannel *> channels, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel items in channels vector.
  }
}

void InitQueryInstance() {
  query = SBDGroupChannel::CreateMyGroupChannelListQuery();
  query->limit = 30;
  query->include_empty_channel = true;
}

void GetMyGroupChannels() {
  SendBirdMyGroupChannelListQueryHandler *handler = new SendBirdMyGroupChannelListQueryHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

### Querying Group Channels by User IDs

It is possible to filter a channel search by user IDs. This can be done by calling `sSetUsersExactFilter()` or `SetUsersIncludeFilter()` of `SBDGroupChannelListQuery`.

Given an example where a user (with the ID "User") is part of two Group Channels:
* channelA: { "User", "John", "Jay" }
* channelB: { "User", "John", "Jay", "Jin" }

An **ExactFilter** returns the list of channels containing exactly the queried userIDs

```cpp
SBDGroupChannelListQuery *query;

class SendBirdMyGroupChannelListQueryHandler : public SBDLoadNextGroupChannelListInterface {
public:
  SendBirdMyGroupChannelListQueryHandler() {

  }

  ~SendBirdMyGroupChannelListQueryHandler() {

  }

  void CompletionHandler(vector<SBDGroupChannel *> channels, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel items in channels vector.
  }
}

void InitQueryInstance() {
  query = SBDGroupChannel::CreateMyGroupChannelListQuery();
  query->limit = 30;

  query->SetUsersExactFilter(USERS);
}

void GetMyGroupChannels() {
  SendBirdMyGroupChannelListQueryHandler *handler = new SendBirdMyGroupChannelListQueryHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```

An **IncludeFilter** returns channels where the userIDs are included. This method can return one of two different results, based on the parameter `query_type`.

```cpp
SBDGroupChannelListQuery *query;

class SendBirdMyGroupChannelListQueryHandler : public SBDLoadNextGroupChannelListInterface {
public:
  SendBirdMyGroupChannelListQueryHandler() {

  }

  ~SendBirdMyGroupChannelListQueryHandler() {

  }

  void CompletionHandler(vector<SBDGroupChannel *> channels, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Do not deallocate the channel items in channels vector.
  }
}

void InitQueryInstance() {
  query = SBDGroupChannel::CreateMyGroupChannelListQuery();
  query->limit = 30;

  query->SetUsersIncludeFilter(USERS, SBDGroupChannelListQueryTypeAnd);
}

void GetMyGroupChannels() {
  SendBirdMyGroupChannelListQueryHandler *handler = new SendBirdMyGroupChannelListQueryHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(handler);
}
```


### Sending messages
Upon entering a channel, a user will be able to send messages of the following types:
* **UserMessage** : a User text message.
* **FileMessage** : a User binary message.

You can additionally specify a `CUSTOM_TYPE` to further subclassify a message.

When you send a text message, you can additionally attach arbitrary strings via a `DATA` field. You can utilize this field to send structured data such as font sizes, font types, or custom JSON objects.

Delivery failures (e.g., due to network issues) will return an exception. By implementing the virtual method, `CompletionHandler()`, it is possible to display only the messages that are successfully sent.

```cpp
#include <SendBird.h>

class SendBirdSendUserMessageHandler : public SBDSendUserMessageInterface {
public:
  SendBirdSendUserMessageHandler() {
  }

  ~SendBirdSendUserMessageHandler() {
  }

  void CompletionHandler(SBDUserMessage *user_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `user_message`.
  }
};


void SendUserMessage() {
  SendBirdSendUserMessageHandler *handler = new SendBirdSendUserMessageHandler(); // `handler` has to be deallocated later.

  // `SendUserMessage()` belongs to `SBDBaseChannel` class, so it can be used by `SBDOpenChannel` and `SBDGroupChannel` instance.
  // TARGET_LANGUAGES is vector<wstring> type. If you don't have to set it, set vector<wstring>().
  channel->SendUserMessage(MESSAGE_TEXT, DATA, CUSTOM_TYPE, TARGET_LANGUAGES, handler);
}
```

A user can also send any binary file through SendBird. There are two ways in which you can send a binary file: by sending a **URL**.

By sending a raw file, you can choose to send a file hosted in your own servers by passing in a URL that points to the file. In this case, your file will not be hosted in the SendBird servers, and downloads of the file will occur through your own servers instead.


```cpp
#include <SendBird.h>

class SendBirdSendFileMessageHandler : public SBDSendFileMessageInterface {
public:
  SendBirdSendFileMessageHandler() {

  }

  ~SendBirdSendFileMessageHandler() {

  }

  void CompletionHandler(SBDFileMessage *file_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `file_message`.
  }
};

void SendFileMessage() {
  SendBirdSendFileMessageHandler *handler = new SendBirdSendFileMessageHandler(); // `handler` has to be deallocated later.
  channel->SendFileMessage(FILE_URL, FILE_NAME, FILE_SIZE, FILE_TYPE, CUSTOM_DATA, CUSTOM_TYPE, handler);
}
```

### Receiving messages
Messages can be received by adding a **[SBDChannelInterface](#event_handler)**.
A received `SBDBaseMessage` object can be of one of three different types of messages.

* **SBDUserMessage** : a User text message.
* **SBDFileMessage** : a User binary message.
* [SBDAdminMessage](#open_channel_3_admin_messages) : an Admin message which can be sent by an admin through the Platform API.

`UNIQUE_HANDLER_ID` is a unique identifier to register multiple concurrent handlers.


```cpp
#include <SendBird.h>

class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void MessageReceived(SBDBaseChannel *channel, SBDBaseMessage *message) {

  }

  // ...
};

void InitChannelEventHandler() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_HANDLER_ID);
}
```

You should remove the channel delegate where the UI is no longer valid.

```cpp
#include <SendBird.h>

void RemoveChannelHandler() {
  SBDMain::RemoveChannelHandler(UNIQUE_HANDLER_ID);
}
```

### Loading previous messages
You can load previous messages by creating a `SBDPreviousMessageListQuery` instance.
You will be able to display past messages in your UI once they have loaded.

```cpp
#include <SendBird.h>

SBDPreviousMessageListQuery *query;

class SendBirdLoadPreviousMessageListHandler : public SBDLoadPreviousMessageListInterface {
public:
  SendBirdLoadPreviousMessageListHandler() {

  }

  ~SendBirdLoadPreviousMessageListHandler() {

  }

  void CompletionHandler(vector<SBDBaseMessage *> messages, SBDError *error) {
    // Error Handlling.

    // Deallocate error.
    delete error;

    return;
  }
};

void InitQuery() {
  query = channel->CreatePreviousMessageQuery();
}

void GetPreviousMessages() {
  SendBirdLoadPreviousMessageListHandler *handler = new SendBirdLoadPreviousMessageListHandler(); // `handler` has to be deallocated later.
  query->LoadNextPage(30, false, handler);
}
```

Past messages are queried in fixed numbers (30 in the above code). A new `SBDPreviousMessageListQuery` instance will load the most recent `n` messages. Calling `LoadNextPage()` on the same query instance will load `n` messages before that. Therefore, you should store your query instance as a member variable in order to traverse through your entire message history.

> An important note is that you must receive your first `CompletionHandler()` callback before invoking `LoadNextPage()` again.

### Loading messages by timestamp
You can retrieve a set number of messages starting from a specific timestamp.

To load messages sent prior to a specifed timestamp, use [`GetPreviousMessagesByTimestamp()`] of the channel instance.

```cpp
#include <SendBird.h>

int64_t timestamp = INT64_MAX;

class SendBirdGetPreviousMessagesHandler : public SBDGetMessagesInterface {
public:
  SendBirdGetPreviousMessagesHandler() {

  }

  ~SendBirdGetPreviousMessagesHandler() {

  }
  
  void CompletionHandler(vector<SBDBaseMessage *> messages, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void GetPreviousMessage() {
  SendBirdGetPreviousMessagesHandler *handler = new SendBirdGetPreviousMessagesHandler(); // `handler` has to be deallocated later.
  channel->GetPreviousMessagesByTimestamp(timestamp, limit, reverse, messageType, customType, handler);
}
```

> * `timestamp` : The reference timestamp.
* `limit` : The number of messages to load. Note that the actual number of results may be larger than the set value when there are multiple messages with the same timestamp as the earliest message.
* `reverse` : Whether to reverse the results.
* `messageType` : A [`SBDMessageTypeFilter`] enum type. Should be one of `SBDMessageTypeFilterUser`, `SBDMessageTypeFilterFile`, `SBDMessageTypeFilterAdmin`, or `SBDMessageTypeFilterAll`.
* `customType` : The Custom Type of the messages to be returned.

To load messages sent after a specified timestamp, call [`GetNextMessagesByTimestamp()`] in a similar fashion. To load results on either side of the reference timestamp, use [`GetMessagesByTimestamp()`].


### Deleting messages
Users are able to delete messages. An error is returned if a user tries to delete messages sent by someone else.
Channel Operators are able to delete any message in the channel, including those by other users.

Deleting a message fires a `MessageDeleted` event to all other users in the channel.

```cpp
#include <SendBird.h>

class SendBirdDeleteMessageHandler : public SBDDeleteMessageInterface {
public:
  SendBirdDeleteMessageHandler() {

  }

  ~SendBirdDeleteMessageHandler() {

  }

  void CompletionHandler(SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

void DeleteMessage() {
  SendBirdDeleteMessageHandler *handler = new SendBirdDeleteMessageHandler(); // `handler` has to be deallocated later.
  channel->DeleteMessage(USER_MESSAGE, handler);
}
```

You can receive a `MessageDeleted()` event using a Channel Interface.

```cpp
class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void MessageDeleted(SBDBaseChannel *channel, uint64_t message_id) {

  }

  // ...
};

void InitSendBird() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_CHANNEL_HANDLER_IDENTIFIER);
}
```

## 1-to-1 Chat

A 1-to-1 chat is just a Group Channel with two members.

### Creating a 1-to-1 chat

A Group Channel can be created on demand by a user through the client SDK. Pass in two user IDs to create a 1-to-1 chat between two users.

You would typically want a 1-to-1 chat to be **Distinct**. If the Distinct property is not enabled, the user will be able to create a new channel with the same opponent, even if they have had previous conversations. In this case, multiple 1-to-1 chats between the same two users might exist, each with its own chat history and data.

```cpp
#include <SendBird.h>

class SendBirdCreateGroupChannelHandler : public SBDCreateGroupChannelInterface {
public:
  SendBirdCreateGroupChannelHandler() {
  }

  ~SendBirdCreateGroupChannelHandler() {
  }

  void CompletionHandler(SBDGroupChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The group channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateGroupChannel() {
  SendBirdCreateGroupChannelHandler *handler = new SendBirdCreateGroupChannelHandler(); // `handler` has to be deallocated later.
  SBDGroupChannel::CreateChannel(vector<wstring>(), SBD_NULL_WSTRING, IS_DISTINCT, SBD_NULL_WSTRING, SBD_NULL_WSTRING, SBD_NULL_WSTRING, handler);
}
```

You can also append information by passing additional arguments

```cpp
#include <SendBird.h>

class SendBirdCreateGroupChannelHandler : public SBDCreateGroupChannelInterface {
public:
  SendBirdCreateGroupChannelHandler() {
  }

  ~SendBirdCreateGroupChannelHandler() {
  }

  void CompletionHandler(SBDGroupChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The group channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateGroupChannel() {
  SendBirdCreateGroupChannelHandler *handler = new SendBirdCreateGroupChannelHandler(); // `handler` has to be deallocated later.
  SBDGroupChannel::CreateChannel(vector<wstring>(), NAME, IS_DISTINCT, COVER_IMAGE_URL, DATA, CUSTOM_TYPE, handler);
}
```

* `NAME` : the name of the channel, or the Channel Topic.
* `COVER_IMAGE_URL` : the URL of the cover image, which you can fetch to render into the UI.
* `DATA` : a `wstring` field to store structured information, such as a JSON String.
* `CUSTOM_TYPE` : a `wstring` field that allows you to subclassify your channel.

> See the [Advanced](##group-channel---advanced) section for more information on cover images and Custom Types.

You can also create channels via the SendBird [Platform API](/platform). You should utilize the Platform API when you wish to control channel creations and member invitations on the server-side.

## Group Channel - Advanced

### Getting a list of all channel members

You can obtain a list of members in a Group Channel by referencing the `members` attribute within `SBDGroupChannel`.

```cpp
vector<SBDMember> members = group_channel->members;
```

### Getting members' online statuses
To stay updated on each participant's connection status, you must obtain a new `SBDUserListQuery`, which contains the latest information on each user. To get a `SBDUserListQuery` for a specific channel, call `CreateOpenChannelListQuery()` of `SBDOpenChannel`. If you wish to get the list of all users of your service (application), call `CreateUserListQuery(USER_IDS)` of `SBDMain`.

You can then check each of the users' connection statuses by referencing `connection_status` of `SBDUser`.

> If your application needs to keep track of users' connection statuses in real time, we recommend that you receive a new `SBDUserListQuery` periodically, perhaps in intervals of one minute or more.

`connection_status` can return one of three values:

* `SBDUserConnectionStatusNotAvailable` : User's status information cannot be reached.
* `SBDUserConnectionStatusOffline` : User is disconnected from SendBird.
* `SBDUserConnectionStatusOnline` : User is connected to SendBird.


### Typing indicators
You can send typing events by invoking `StartTyping()` and `EndTyping()`.

```cpp
group_channel->StartTyping();
group_channel->EndTyping();
```

You can receive a `TypingStatusUpdated` event with a Channel Interface.

```cpp
#include <SendBird.h>

class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void TypingStatusUpdated(SBDGroupChannel *channel) {

  }

  // ...
};

void InitChannelEventHandler() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_HANDLER_ID);
}
```

### Read Receipts
A user can indicate that they have read a message by calling `MarkAsRead()`.

```cpp
group_channel->MarkAsRead();
```

This broadcasts a `ReadReceiptUpdated` event, which can be handled with a channel interface.

```cpp
#include <SendBird.h>

class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void ReadReceiptUpdated(SBDGroupChannel *channel) {

  }

  // ...
};

void InitChannelEventHandler() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_HANDLER_ID);
}
```

`GetReadReceipt()` returns the number of members in the channel who have not read the message

```cpp
int unread_count = channel->GetReadReceipt(message);
```

### Viewing who has read a message

You can view who has read a message with [`GetReadMembers()`]. This list is updated when the message's read receipt is updated. Therefore, you should replace your previous message instance with the newly received message in [`ReadReceiptUpdated()`] for real-time updates.

```cpp
vector<SBDMember> members = group_channel->GetReadMembers(message);
```

Similarly, you can also view who has **not** read the message with [`GetUnreadMembers():`].

### Admin messages

You can send Admin messages to users in a channel using the [SendBird Dashboard](https://dashboard.sendbird.com) or the [Platform API](/platform).

To do so using the Dashboard, navigate to the **Group Channels** tab. Inside the message box, you should see an option to send an Admin message. Admin messages should not be longer than 1000 characters.

> If you are currently developing under the **Free Plan** and therefore cannot access the **Moderation Tools** from the Dashboard, you must send Admin messages through the Platform API.


### Channel cover images

When creating a channel, you can add a cover image by specifying an image URL.

```cpp
#include <SendBird.h>

class SendBirdCreateGroupChannelHandler : public SBDCreateGroupChannelInterface {
public:
  SendBirdCreateGroupChannelHandler() {

  }

  ~SendBirdCreateGroupChannelHandler() {

  }

  void CompletionHandler(SBDGroupChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The group channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateGroupChannel() {
  SendBirdCreateGroupChannelHandler *handler = new SendBirdCreateGroupChannelHandler(); // `handler` has to be deallocated later.

  SBDGroupChannel::CreateChannel(vector<wstring>(), NAME, IS_DISTINCT, COVER_URL, SBD_NULL_WSTRING, SBD_NULL_WSTRING, handler);
}
```

You can get the cover image URL referencing `cover_url`. You can also update a channel's cover image by calling `UpdateChannel()`.

### Custom channel types

When creating a channel, you can additionally specify a **Custom Type** to further subclassify your channels. This custom type takes on the form of a `NSString`, and can be handy in searching or filtering channels.

> `DATA` and `CUSTOM_TYPE` are both String fields that allow you to append information to your channels. The intended use case is for `CUSTOM_TYPE` to contain information that can subclassify the channel (e.g., distinguishing "School" and "Work" channels). However, both these fields can be flexibly utilized.

```cpp
#include <SendBird.h>

class SendBirdCreateGroupChannelHandler : public SBDCreateGroupChannelInterface {
public:
  SendBirdCreateGroupChannelHandler() {

  }

  ~SendBirdCreateGroupChannelHandler() {

  }

  void CompletionHandler(SBDGroupChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
    
    // The group channel is created.
    // Do not deallocate `channel` pointer.
  }
};

void CreateGroupChannel() {
  SendBirdCreateGroupChannelHandler *handler = new SendBirdCreateGroupChannelHandler(); // `handler` has to be deallocated later.

  SBDGroupChannel::CreateChannel(vector<wstring>(), NAME, IS_DISTINCT, COVER_URL, DATA, CUSTOM_TYPE, handler);
}
```

To get a channel's Custom Type, read `channel->custom_type`.

### Custom message types

Likewise, you can specify a **Custom Type** for messages in order to categorize them into more specific groups. This custom type takes on the form of a `wstring`, and can be useful in searching or filtering messages.

> `DATA` and `CUSTOM_TYPE` are both String fields that allow you to append information to your messages. The intended use case is for `CUSTOM_TYPE` to contain information that can subclassify the message (e.g., distinguishing "FILE_IMAGE" and "FILE_AUDIO" type messages). However, both these fields can be flexibly utilized.

To embed a Custom Type into a message, simply pass a String parameter to `SendUserMessage()` or `SendFileMessage()`.

```cpp
#include <SendBird.h>

class SendBirdSendUserMessageHandler : public SBDSendUserMessageInterface {
public:
  SendBirdSendUserMessageHandler() {
  }

  ~SendBirdSendUserMessageHandler() {
  }

  void CompletionHandler(SBDUserMessage *user_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `user_message`.
  }
};


void SendUserMessage() {
  SendBirdSendUserMessageHandler *handler = new SendBirdSendUserMessageHandler(); // `handler` has to be deallocated later.

  // `SendUserMessage()` belongs to `SBDBaseChannel` class, so it can be used by `SBDOpenChannel` and `SBDGroupChannel` instance.
  // TARGET_LANGUAGES is vector<wstring> type. If you don't have to set it, set vector<wstring>().
  channel->SendUserMessage(MESSAGE_TEXT, DATA, CUSTOM_TYPE, vector<wstring>(), handler);
}
```

To get a message's Custom Type, read `message->custom_type`.

### Message auto-translation

> This feature is not available under the Free plan. Contact [sales@sendbird.com](mailto:sales@sendbird.com) if you wish to implement this functionality.

SendBird makes it possible for messages to be sent in different languages through its auto-translation feature.
Pass in a `vector` of language codes to SendUserMessage()` to request translated messages in the corresponding languages.

```cpp
#include <SendBird.h>

class SendBirdSendUserMessageHandler : public SBDSendUserMessageInterface {
public:
  SendBirdSendUserMessageHandler() {
  }

  ~SendBirdSendUserMessageHandler() {
  }

  void CompletionHandler(SBDUserMessage *user_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // Handle `user_message`.
  }
};


void SendUserMessage() {
  SendBirdSendUserMessageHandler *handler = new SendBirdSendUserMessageHandler(); // `handler` has to be deallocated later.

  // `SendUserMessage()` belongs to `SBDBaseChannel` class, so it can be used by `SBDOpenChannel` and `SBDGroupChannel` instance.

  vector<wstring> target_langs;

  target_langs.push_back(L"es");
  target_langs.push_back(L"ko");

  channel->SendUserMessage(MESSAGE_TEXT, DATA, CUSTOM_TYPE, target_langs, handler);
}
```

You can obtain translations of a message using `user_message->translations`. This method returns a `map<wstring, wstring>` containing the language codes and translations.

```cpp
class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  // ...

  void MessageReceived(SBDBaseChannel *channel, SBDBaseMessage *message) {
    vector<wstring> translations = (SBDUserMessage *)message;
    wstring es_translation = translations[L"es"];

    // Display translation in UI.
  }

  // ...
};
```

> Reference the table below for supported languages and their language codes.

| Language Code | English Name        | Language Code | English Name       |
|---------------|---------------------|---------------|--------------------|
| af            | Afrikaans           | tlh-Qaak      | Klingon (pIqaD)    |
| ar            | Arabic              | ko            | Korean             |
| bs-Latn       | Bosnian (Latin)     | lv            | Latvian            |
| bg            | Bulgarian           | lt            | Lithuanian         |
| ca            | Catalan             | ms            | Malay              |
| zh-CHS        | Chinese Simplified  | mt            | Maltese            |
| zh-CHT        | Chinese Traditional | no            | Norwegian          |
| hr            | Croatian            | fa            | Persian            |
| cs            | Czech               | pl            | Polish             |
| da            | Danish              | pt            | Portuguese         |
| nl            | Dutch               | otq           | Querétaro Otomi    |
| en            | English             | ro            | Romanian           |
| et            | Estonian            | ru            | Russian            |
| fi            | Finnish             | sr-Cyrl       | Serbian (Cyrillic) |
| fr            | French              | sr-Latn       | Serbian (Latin)    |
| de            | German              | sk            | Slovak             |
| el            | Greek               | sl            | Slovenian          |
| ht            | Haitian Creole      | es            | Spanish            |
| he            | Hebrew              | sv            | Swedish            |
| hi            | Hindi               | th            | Thai               |
| mww           | Hmong Daw           | tr            | Turkish            |
| hu            | Hungarian           | uk            | Ukrainian          |
| id            | Indonesian          | ur            | Urdu               |
| it            | Italian             | vi            | Vietnamese         |
| ja            | Japanese            | cy            | Welsh              |
| sw            | Kiswahili           | yua           | Yucatec Maya       |
| tlh           | Klingon             |     -         |          -         |

### File Message thumbnails

> This feature is not available under the Free plan. Contact [sales@sendbird.com](mailto:sales@sendbird.com) if you wish to implement this functionality.

When sending an image file, you can choose to create thumbnails of the image, which you can fetch and render into your UI. You can specify up to **3** different dimensions to generate thumbnail images in, which can be convenient for supporting various display densities.

> Supported file types are files whose **MIME type** is `image/*` or `video/*`.

> The SDK does not support creating thumbnails when [sending a File Message via a file URL](#open_channel_3_sending_messages).

Create a `vector` of `SBDThumbnailSize` objects to pass to `SendFileMessageWithPath()`. A `SBDThumbnailSize` can be created with the constructor `SBDThumbnailSize()`, where the values specify **pixels**. The `completion_handler` callback of `SBDSendFileMessageInterface` will subsequently return a `vector` of `SBDThumbnail` objects in the file message object that each contain the URL of the generated thumbnail image file.

```cpp
class SendBirdSendFileMessageHandler : public SBDSendFileMessageInterface {
public:
  SendBirdSendFileMessageHandler() {

  }

  ~SendBirdSendFileMessageHandler() {

  }

  void CompletionHandler(SBDFileMessage *file_message, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    // file_message->thumbnails has the thumbnails' information.
  }
};

void SendFileMessage() {
  vector<SBDThumbnailSize> thumbnail_sizes;
  thumbnail_sizes.push_back(SBDThumbnailSize(320, 320));
  thumbnail_sizes.push_back(SBDThumbnailSize(160, 160));

  SendBirdSendFileMessageHandler *handler = new SendBirdSendFileMessageHandler(); // `handler` has to be deallocated later.
  channel->SendFileMessageWithPath(FILE_PATH, FILE_MIME_TYPE, thumbnail_sizes, DATA, CUSTOM_TYPE, handler));	
}
```

`max_width` and `max_height` specify the maximum dimensions of the thumbnail. Your image will be scaled down evenly to fit within the bounds of (`max_width`, `max_height`). Note that if the original image is smaller than the specified dimensions, the thumbnail will not be scaled. `GetUrl()` returns the location of the generated thumbnail file within the SendBird servers.

## Channel Metadata

With **MetaData** and **MetaCounters**, you can store additional information within a channel.


**MetaData** allows you to store a `map` of wstring key-value pairs in a channel instance.
If your aim is to store an integer with atomic increasing/decreasing operations, you should use a **MetaCounters** instead.

Use cases for MetaData/Counters could include tracking the number of likes, the background color, or a long description of the channel, which can each be fetched and rendered into the UI.

### MetaData

MetaData is a `map<wstring, wstring>` that is stored within a channel. Its uses are very flexible, allowing you to customize a channel to fit you and your users' needs.

#### Create
Storing MetaData into a channel simply requires creation of a `map<wstring, wstring>`, then passing it as an argument when calling `CreateMetaData()`. You can store multiple key-value pairs in the dictionary.

```cpp
#include <SendBird.h>

class SendBirdCreateChannelMetaDataHandler : public SBDCreateChannelMetaDataInterface {
public:
  SendBirdCreateChannelMetaDataHandler() {

  }

  ~SendBirdCreateChannelMetaDataHandler() {

  }

  void CompletionHandler(map<wstring, wstring> meta_data, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

  }
};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    map<wstring, wstring> meta_data;

    meta_data[KEY_A] = VALUE_A;
    meta_data[KEY_B] = VALUE_B;

    SendBirdCreateChannelMetaDataHandler *handler = new SendBirdCreateChannelMetaDataHandler(); // `handler` has to be deallocated later.
    channel->CreateMetaData(meta_data, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

#### Update
The process for updating MetaData is identical to creation. Values will be updated for existing keys, while new key-value pairs will be added.

```cpp
class SendBirdUpdateChannelMetaDataHandler : public SBDUpdateChannelMetaDataInterface {
public:
  SendBirdUpdateChannelMetaDataHandler() {

  }

  ~SendBirdUpdateChannelMetaDataHandler() {

  }

  void CompletionHandler(map<wstring, wstring> meta_data, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

  }
};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    map<wstring, wstring> meta_data;

    meta_data[KEY_B] = VALUE_B;
    meta_data[KEY_C] = VALUE_C;

    SendBirdUpdateChannelMetaDataHandler *handler = new SendBirdUpdateChannelMetaDataHandler(); // `handler` has to be deallocated later.
    channel->UpdateMetaData(meta_data, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

#### Get
Getting stored MetaData requires creating a `vector` of keys to pass as an argument to `GetMetaData()`. The callback `CompletionHandler` of `SBDGetChannelMetaDataInterface` returns a `map<wstring, wstring>` containing the corresponding key-value pairs.

```cpp
class SendBirdGetChannelMetaDataHandler : public SBDGetChannelMetaDataInterface {
public:
  SendBirdGetChannelMetaDataHandler() {

  }

  ~SendBirdGetChannelMetaDataHandler() {

  }

  void CompletionHandler(map<wstring, wstring> meta_data, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    vector<wstring> keys;
    keys.push_back(KEY_A);
    keys.push_back(KEY_C);

    SendBirdGetChannelMetaDataHandler *handler = new SendBirdGetChannelMetaDataHandler(); // `handler` has to be deallocated later.
    channel->GetMetaData(keys, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

### MetaCounter

A MetaCounter is a `map<wstring, int64_t>` that is stored within a channel instance. Its primary uses are to track and update discrete indicators within a channel.

#### Create
Storing a MetaCounter into a channel simply requires creation of a `map<wstring, int64_t>`, then passing it as an argument when calling `CreateMetaCounters()`. You can store multiple key-value pairs in the dictionary.

```cpp
class SendBirdCreateChannelMetaCountersHandler : public SBDCreateChannelMetaCountersInterface {
public:
  SendBirdCreateChannelMetaCountersHandler() {

  }

  ~SendBirdCreateChannelMetaCountersHandler() {

  }

  void CompletionHandler(map<wstring, int64_t> meta_counters, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    map<wstring, int64_t> meta_counters;

    meta_counters[KEY_A] = 10;
    meta_counters[KEY_B] = 10;

    SendBirdCreateChannelMetaCountersHandler *handler = new SendBirdCreateChannelMetaCountersHandler(); // `handler` has to be deallocated later.
    channel->CreateMetaCounters(meta_counters, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

#### Get
Retrieving stored MetaCounters requires creating a `vector` of keys to pass as an argument to `GetMetaCounters()`. The callback `CompletionHandler` `SBDGetChannelMetaDataInterface` returns a `map<wstring, int64_t>` containing the corresponding key-value pairs.

```cpp
class SendBirdGetChannelMetaCountersHandler : public SBDGetChannelMetaCountersInterface {
public:
  SendBirdGetChannelMetaCountersHandler() {

  }

  ~SendBirdGetChannelMetaCountersHandler() {

  }

  void CompletionHandler(map<wstring, int64_t> meta_counters, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    vector<wstring> keys;
    keys.push_back(KEY_A);
    keys.push_back(KEY_B);

    SendBirdGetChannelMetaCountersHandler *handler = new SendBirdGetChannelMetaCountersHandler(); // `handler` has to be deallocated later.
    channel->GetMetaCounters(keys, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

#### Increase
The increase and decrease operations work similarly to getting MetaCounters, as described above. Create a `vector` of keys to pass to `IncreaseMetaCounters()`, which increments the corresponding MetaCounters by 1.

```cpp
class SendBirdIncreaseMetaCountersHandler : public SBDIncreaseMetaCountersInterface {
public:
  SendBirdIncreaseMetaCountersHandler() {

  }

  ~SendBirdIncreaseMetaCountersHandler() {

  }

  void CompletionHandler(map<wstring, int64_t> meta_counters, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    map<wstring, int64_t> meta_counters;

    meta_counters[KEY_A] = 5;

    SendBirdCreateChannelMetaCountersHandler *handler = new SendBirdCreateChannelMetaCountersHandler(); // `handler` has to be deallocated later.
    channel->IncreaseMetaCounters(meta_counters, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

#### Decrease
Likewise, pass a `vector` of keys to `DecreaseMetaCounters()`, which decrements the MetaCounters by 1.

```cpp
class SendBirdDecreaseMetaCountersHandler : public SBDDecreaseMetaCountersInterface {
public:
  SendBirdDecreaseMetaCountersHandler() {

  }

  ~SendBirdDecreaseMetaCountersHandler() {

  }

  void CompletionHandler(map<wstring, int64_t> meta_counters, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }
  }
};

class SendBirdGetOpenChannelHandler : public SBDGetOpenChannelInterface {
public:
  SendBirdGetOpenChannelHandler() {

  }

  ~SendBirdGetOpenChannelHandler() {

  }

  void CompletionHandler(SBDOpenChannel *channel, SBDError *error) {
    if (error != NULL) {
      // Error Handlling.

      // Deallocate error.
      delete error;

      return;
    }

    map<wstring, int64_t> meta_counters;

    meta_counters[KEY_A] = 5;

    SendBirdDecreaseMetaCountersHandler *handler = new SendBirdDecreaseMetaCountersHandler(); // `handler` has to be deallocated later.
    channel->DecreaseMetaCounters(meta_counters, handler);
    // Do not deallocate the channel instance.
  }
};

void GetOpenChannel() {
  SendBirdGetOpenChannelHandler *handler = new SendBirdGetOpenChannelHandler(); // `handler` has to be deallocated later.
  SBDOpenChannel::GetChannel(CHANNEL_URL, handler);
}
```

## Event Handler

Event Handlers are crucial components of the SendBird SDK that allow a client to react to server-side events. These handlers contain callback methods that can be overridden to respond to specific chat-related events passed from the server. For example, `channel:didReceiveMessage:` of `SBDChannelDelegate` is triggered whenever a message is received. The specifics of each received message is contained within the `SBDBaseChannel` and `SBDBaseMessage` arguments passed back from the triggering callback.

By providing its own Event Handlers, the SendBird SDK allows a client to respond to asynchronous events without worrying about the plethora of issues surrounding client-server communication and multithreading. A chat application especially involves rapid exchanges of data that must take place in near real-time across potentially thousands of users. Therefore, the SDK optimizes communication and threading to ensure data integrity between users and servers. Add Event Handlers and implement the necessary callback methods to track events occurring within channels or a user's own device.

### Channel Delegate

Register a **SBDChannelDelegate** to receive information whenever events occur within a channel.

You can register multiple Channel Delegates. `UNIQUE_HANDLER_ID` is a unique identifier that should be given to each delegate. Typically, Event Handlers would be registered in each view controller in order to stay up to date with changes in the channel, as well as notify the channel of the user's own view controller.

```cpp
class SendBirdChannelEventHandler : public SBDChannelInterface {
public:
  SendBirdChannelEventHandler() {
  }

  ~SendBirdChannelEventHandler() {
  }

  void MessageReceived(SBDBaseChannel *channel, SBDBaseMessage *message) {
    // Received a chat message
  }

  void MessageUpdated(SBDBaseChannel *channel, SBDBaseMessage *message) {

  }

  void ReadReceiptUpdated(SBDGroupChannel *channel) {
    // When read receipt has been updated 
  }

  void TypingStatusUpdated(SBDGroupChannel *channel) {
    // When typing status has been updated
  }

  void InvitationReceived(SBDGroupChannel *channel, vector<SBDUser> invitees, SBDUser inviter) {

  }

  void InvitationDeclined(SBDGroupChannel *channel, SBDUser invitee, SBDUser inviter) {

  }

  void UserJoined(SBDGroupChannel *channel, SBDUser user) {
    // When a new member joined the group channel
  }

  void UserLeft(SBDGroupChannel *channel, SBDUser user) {
    // When a member left the group channel
  }

  void UserEntered(SBDOpenChannel *channel, SBDUser user) {
    // When a new user entered the open channe
  }

  void UserExited(SBDOpenChannel *channel, SBDUser user) {
    // When a new user left the open channel
  }

  void UserMuted(SBDOpenChannel *channel, SBDUser user) {
    // When a user is muted on the open channel
  }

  void UserUnmuted(SBDOpenChannel *channel, SBDUser user) {
    // When a user is unmuted on the open channel
  }

  void UserBanned(SBDOpenChannel *channel, SBDUser user) {
    // When a user is banned on the open channel
  }

  void UserUnbanned(SBDOpenChannel *channel, SBDUser user) {
    // When a user is unbanned on the open channel
  }

  void ChannelFrozen(SBDOpenChannel *channel) {
    // When the open channel is frozen
  }

  void ChannelUnfrozen(SBDOpenChannel *channel) {
    // When the open channel is unfrozen
  }

  void ChannelChanged(SBDBaseChannel *channel) {
    // When a channel property has been changed
  }

  void ChannelDeleted(wstring channel_url, SBDChannelType channel_type) {
    // When a channel has been deleted
  }

  void MessageDeleted(SBDBaseChannel *channel, uint64_t message_id) {
    // When a message has been deleted
  }

  void ChannelMetaDataCreated(SBDBaseChannel *channel, map<wstring, wstring> created_meta_data) {

  }

  void ChannelMetaDataUpdated(SBDBaseChannel *channel, map<wstring, wstring> updated_meta_data) {

  }

  void ChannelMetaDataDeleted(SBDBaseChannel *channel, vector<wstring> deleted_meta_data) {

  }

  void ChannelMetaCountersCreated(SBDBaseChannel *channel, map<wstring, int64_t> created_meta_counters) {

  }

  void ChannelMetaCountersUpdated(SBDBaseChannel *channel, map<wstring, int64_t> updated_meta_counters) {

  }

  void ChannelMetaCountersDeleted(SBDBaseChannel *channel, vector<wstring> deleted_meta_counters) {

  }
};

void InitSendBird() {
  SBDMain::AddChannelHandler(new SendBirdChannelEventHandler(), UNIQUE_CHANNEL_HANDLER_IDENTIFIER);
}
```

> `ChannelChanged()` is called whenever a one of the following channel properties have been changed :
* **Push preference** (It can be changed on iOS/Android devices)
* **Last message** (except in cases where the message is a silent Admin message)
* **Unread message count**
* **Name**, **cover image**, **data**, **Custom Type**
* **Operators** (only applicable to Open Channels)
* **Distinct** property (only applicable to Group Channels)

You should remove the `SBDChannelInterface` if the handler is no longer valid.

```cpp
SBDMain::RemoveChannelHandler(UNIQUE_CHANNEL_HANDLER_IDENTIFIER);
```

### Reconnection Delegate

Register a **SBDReconnectionInterface** to detect changes in the user's own connection status.

You can register multiple Connection Delegates. `UNIQUE_HANDLER_ID` is a unique identifier that should be given to each delegate. Typically, Connection Delegates would be registered in each view controller in order to monitor the state of the user's connection with the SendBird servers.

```cpp
class SendBirdReconnectEventHandler : public SBDReconnectionInterface {
public:
  SendBirdReconnectEventHandler() {

  }

  void Started() {
    // Network has been disconnected. Auto reconnecting starts.
  }

  void Succeeded() {
    // Auto reconnecting succeeded.
  }

  void Failed() {
    // Auto reconnecting failed. You should call `connect` to reconnect to SendBird.);
  }

  void Cancelled() {
    // Auto reconnecting is cancelled by explicit reconnection.
  }
};
```

You should remove the `SBDReconnectionInterface` if the handler is no longer valid.

```cpp
SBDMain::RemoveReconnectionHandler(UNIQUE_CHANNEL_HANDLER_IDENTIFIER);
```
