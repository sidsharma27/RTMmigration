This page juxtaposes the legacy Signaling APIs with the Real-time Messaging APIs. 

## Login & Logout

| Method                 | Signaling                              | Real-time Messaging                  |
| ---------------------- | -------------------------------------- | ------------------------------------ |
| Creates an instance.   | `getInstance`/`createAgoraSDKInstance` | `createInstance`<sup>1</sup>         |
| Sets a callback.       | `callbackSet`                          | N/A                                  |
| Login                  | `login`/`login2`                       | `login`<sup>2</sup>                  |
| Logout                 | `logout`                               | `logout`                             |
| Gets the login status. | `getStatus`                            | N/A. See `onConnectionStateChanged`. |
| Destroys the instance. | `destroy`                              | `release`                            |

| Event                | Signaling                              | Real-time Messaging            |
| --------------------- | -------------------------------------- | ------------------------------ |
| Login succeeds. | `onLoginSuccess` | `onSuccess`     |
| Login Fails. | `onLoginFailed`            | `onFailure`                 |
| Logout results.     | `onLogout`              | `onFailure`              |
| Connection state changes. | N/A. See `getStatus`.          | `onConnectionStateChanged` |
|                          |                       |                            |

> - Unless otherwise specified, most of the core APIs of the Agora RTM Android SDK should only be called after the `login` method call succeeds and after you receive the `onSuccess` callback.
> - <sup>1</sup> You can create multiple RtmClient instances with the `createInstance` method. The Agora RTM SDK does not put a limit to the number of RtmClient instances you can create, but it only allows you to join a maximum of 20 RtmChannels at the same time. 
> - <sup>2</sup> The generation of the token you use to log in the Agora RTM system differs from the generation of the signalingToken you use to log in the Agora Signaling system. Make sure you use the right token. 

## Sending a peer-to-peer message

| Method                        | Signaling            | Real-time Messaging         |
| ----------------------------- | -------------------- | --------------------------- |
| Creates a message instance.   | N/A                  | `createMessage`<sup>1</sup> |
| Sends a peer-to-peer message. | `messageInstantSend` | `sendMessageToPeer`         |


| Event                       | Signaling            | Real-time Messaging |
| ---------------------------- | -------------------- | ------------------- |
| Peer-to-peer message sending succeeds. | `onMessageSendSuccess` | `onSuccess` |
| Peer-to-peer message sending fails. | `onMessageSendError` | `onFailure` |
| Receives a peer-to-peer message | `onMessageInstantReceive` | `onMessageReceived` |

> <sup>1</sup> With the Agora RTM SDK, you must create a message instance before sending it. A message instance can be used either for a peer-to-peer or for a channel message. As of v0.9.3, the Agora RTM SDK allows you to send an offline message by configuring `sendMessageOptions`.

## Querying the online status of specified user(s)

| Method                                         | Signaling            | Real-time Messaging      |
| ---------------------------------------------- | -------------------- | ------------------------ |
| Queries the online status of a specified user. | `queryuserStatus`    | `queryPeersOnlineStatus` |
| Sends a peer-to-peer message.                  | `messageInstantSend` | `sendMessageToPeer`      |



| Event                            | Signaling                 | Real-time Messaging |
| -------------------------------- | ------------------------- | ------------------- |
| Returns the result of the query. | `OnQueryUserStatusResult` | `onSuccess`         |
|                                  |                           |                     |

> With the Agora RTM SDK,  you can query the online status of a list of peer users, not of just one peer user.

## user-Attribute Operations

| Method                                              | Signaling        | Real-time Messaging                   |
| --------------------------------------------------- | ---------------- | ------------------------------------- |
| Sets the local user's attribute                     | `setAttr`        | `addOrUpdateLocalUserAttributes`      |
| Gets an attribute of the local user.                | `getAttr`        | `getUserAttributesBykeys`<sup>1</sup> |
| Gets all attributes of the local user.              | `getAttrAll`     | `getUserAttributes`<sup>2</sup>       |
| Gets all attributes of the specified user.          | `getUserAttrAll` | `getUserAttributes`                   |
| Replaces the local user's attributes with new ones. | N/A              | `setLocalUserAttributes`              |
| Deletes the specified attributes of the local user. | N/A              | `deleteLocaluserAttributeByKeys`      |
| Clears the local user's attributes                  | N/A              | `clearLocalUserAttributes`            |
|                                                     |                  |                                       |

| Event                                                 | Signaling               | Real-time Messaging     |
| ----------------------------------------------------- | ----------------------- | ----------------------- |
| Returns the result of the user-attribute method call. | `onUserAttributeResult` | `onSuccess`/`onFailure` |

>- <sup>1</sup> The `getuserAttributesBykeys` method allows you to retrieve a list of attributes from the local user.
>- <sup>2</sup> The `getUserAttributes` method allows you to retrieve the attributes from either the local user or a specified peer user.  

## Joining or Leaving a Channel

| Method                                | Signaling          | Real-time Messaging         |
| ------------------------------------ | ------------------ | --------------------------- |
| Creates a channel instance.          | N/A                | `createChannel`<sup>1</sup> |
| Joins a specified channel.           | `channelJoin`      | `join`<sup>2</sup>    |
| Deletes all attributes of a channel. | `channelLeave` |  `leave`                           |

| Event                                               | Signaling             | Real-time Messaging |
| --------------------------------------------------- | --------------------- | ------------------- |
| Successfully joins the specified channel.           | `onChannelJoined`     | `onSuccess`         |
| Fails to join the specified channel                 | `onChannelJoinFailed` | `onFailure`         |
| A remote user joins the current channel.            | `onChannelUserJoined` | `onMemberJoined`    |
| Successfully leaves the current channel.            | `onChannelLeaved`     | `onSuccess`         |
| A remote channel member leaves the current channel. | `onChannelUserLeaved` | `onMemberLeft`      |
|                                                     |                       |                     |


> - <sup>1</sup> The Agora RTM SDK requires you to create a channel instance before joining it. 
>
> - <sup>2</sup> The Agora RTM SDK allows you to join a maximum of 20 channels at the same time. 


## Sending a channel message

| Method                                          | Signaling                 | Real-time Messaging         |
| ----------------------------------------------- | ------------------------- | --------------------------- |
| Creates a message instance.                     | N/A                       | `createMessage`<sup>1</sup> |
| Sends a channel message from within a channel.  | `messageChannelSend`      | `sendMessage`<sup>2</sup>   |
| Sends a channel message from outside a channel. | `messageChannelSendForce` | N/A                         |



| Event                                     | Signaling                 | Real-time Messaging |
| ----------------------------------------- | ------------------------- | ------------------- |
| Successfully sends out a channel message. | `onMessageSendSuccess`    | `onSuccess`         |
| Fails to send out a channel message.      | `onMessageSendError`      | `onFailure`         |
| Receives a channel message.               | `onMessageChannelReceive` | `onMessageReceived` |



> - <sup>1</sup> With the Agora RTM SDK, you must create a message instance before sending out a peer-to-peer or channel message. 
> - The Agora RTM SDK does not support sending channel messages from outside a channel. That said, you must join a channel before being able to send out a channel message. 

## CHANNEL-ATTRIBUTE OPERATIONS

| Method                               | Signaling          | Real-time Messaging      |
| ------------------------------------ | ------------------ | ------------------------ |
| Sets a channel attribute.            | `channelSetAttr`   | N/A （Available in 1.1） |
| Deletes a channel attribute.         | `channelDelAttr`   | N/A （Available in 1.1） |
| Deletes all attributes of a channel. | `channelClearAttr` | N/A（Available in 1.1）  |



| Event                           | Signaling              | Real-time Messaging      |
| ------------------------------- | ---------------------- | ------------------------ |
| A channel attribute is updated. | `onChannelAttrUpdated` | N/A （Available in 1.1） |



## Retrieving a member list of the current channel



| Method                                                 | Signaling | Real-time Messaging      |
| ------------------------------------------------------ | --------- | ------------------------ |
| Retrieves the latest user list of a specified channel. | `invoke`  | `getMembers`<sup>1</sup> |



| Event                                         | Signaling                         | Real-time Messaging     |
| --------------------------------------------- | --------------------------------- | ----------------------- |
| Returns the user list in a specified channel. | `onInvokeRet`/`onChannelUserList` | `onSuccess`/`onFailure` |

> <sup>1</sup> You must join an RtmChannel before retrieving a member list of it. When the number of the channel members exceeds 512, the Agora RTM SDK only returns a list of randomly selected 512 channel members. 



## Retrieving the Number of Users in a Specified Channel



| Method                                                | Signaling             | Real-time Messaging |
| ----------------------------------------------------- | --------------------- | ------------------- |
| Retrieves the number of users in a specified channel. | `channelQueryUserNum` | N/A                 |



| Event                                               | Signaling                     | Real-time Messaging |
| --------------------------------------------------- | ----------------------------- | ------------------- |
| Returns the number of users in a specified channel. | `onChannelQueryUserNumResult` | N/A                 |



## Call Invitation



| Method                                                       | Signaling                                | Real-time Messaging                 |
| ------------------------------------------------------------ | ---------------------------------------- | ----------------------------------- |
| Gets an RTM call manager.                                    | N/A                                      | `getRtmCallManager`<sup>1</sup>     |
| Allows the caller to create and manage a `LocalInvitation.`  | N/A                                      | `createLocalInvitation`<sup>2</sup> |
| Allows the caller to send a call invite to a specified user (callee). | `channelInviteUser`/`channelInviteUser2` | `sendLocalInvitation`               |
| Allows the caller to send a call invite to land-line user.   | `channelInviteDTMF`                      | N/A                                 |
| Allows the caller to cancel a sent call invite.              | `channelInviteEnd`                       | `cancelLocalInvitation`             |
| Allows the callee to accept an incoming call invite.         | `channelInviteAccept`                    | `acceptRemoteInvitation`            |
| Allows the callee to decline an incoming call invite.        | `channelInviteRefuse`                    | `refuseRemoteInvitation`            |
|                                                              |                                          |                                     |

> - <sup>1</sup> With the Agora RTM SDK, either a caller or a callee needs to get an `RtmCallManager` instance before using its object methods to send, cancel, accept, or decline a call invite. 
> - <sup>2</sup> The Agora RTM SDK introduces the `LocalInvitation` object and the `RemoteInvitation` object. The former is created by the caller using the `createLocalInvitation` method, the latter is created automatically by the SDK when the callee receives the call invitation from the caller. You can take the two objects as the same call invitation taking in two different forms. The caller uses the `LocalInvitation` object to specify the callee, set the content, or check the `LocalInvitationState`; the callee uses the `RemoteInvitation` object to set a response, check the caller ID, or check the `RemoteInvitationState`. 



| Synchronous Callback      | Signaling | Real-time Messaging                                          |
| ------------------------- | --------- | ------------------------------------------------------------ |
| The method call succeeds. | N/A       | `onSuccess`                                                  |
| The method call fails.    | N/A       | `onFailure`. See `InvitationApiCallError` for the error codes.<sup>3</sup> |

> <sup>3</sup> If a user calls `sendLocalInvitation`, `cancelLocalInvitation`, `acceptRemoteInvitation` or `refuseRemoteInvitation` before the life cycle of a `LocalInvitation` starts or after the life cycle of a `RemoteInvitation` ends (either in failure or in success), the SDK returns the `onFailure` callback with the `InvitationApiCallError` error code. 




| Event                                                        | Signaling                | Real-time Messaging                                          |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| Returns to the caller: the callee receives the call invite.  | `oninviteReceivedByPeer` | `onLocalInvitationReceivedByPeer`                            |
| Returns to the caller: the call invite successfully cancelled. | `onInviteEndByMyself`    | `onLocalInvitationCanceled`                                  |
| Returns to the caller: call invite accepted.                 | `onInviteAcceptedByPeer` | `onLocalInvitationAccepted`                                  |
| Returns to the caller: call invite declined.                 | `onInviteRefusedByPeer`  | `onLocalInvitationRefused`                                   |
| Returns to the caller: the outgoing call invite ends in failure. | `onInviteFailed`         | `onLocalInvitationFailure`. See `LocalInvitationError` for the error codes. <sup>4</sup> |
|                                                              |                          |                                                              |

> <sup>4</sup>: The SDK returns the `onLocalInvitationFailure` to the caller if the call invitation process has started but ends in failure. Scenarios include: the callee is offline, the `LocalInvitation` object times out, the `LocalInvitation` object expires, and the callee receives the call invitation but fails to respond in time. 

| Event                                                   | Signaling           | Real-time Messaging                                          |
| ------------------------------------------------------- | ------------------- | ------------------------------------------------------------ |
| Returns to the land-line user: receives a call invite.  | `onInviteMsg`       | N/A                                                          |
| Returns to the callee: receives a call invite.          | `oninviteReceived`  | `onRemoteInvitationReceived`                                 |
| Returns to the callee: the caller cancels the invite.   | `onInviteEndByPeer` | `onRemoteInvitationCanceled`                                 |
| Returns to the callee: call invite accepted.            | N/A                 | `onRemoteInvitationAccepted`                                 |
| Returns to the callee: call invite declined.            | N/A                 | `onRemoteInvitationRefused`                                  |
| Returns to the callee: the call invite ends in failure. | N/A                 | `onRemoteInvitationFailure`. See `RemoteInvitationError` for the error codes. <sup>5</sup> |
|                                                         |                     |                                                              |

> <sup>5</sup> The SDK returns the `onRemoteInvitationFailure` to the callee if the call invitation process has started but ends in failure. Scenarios include: the caller is offline, the `RemoteInvitation` object times out, and the `RemoteInvitation` ojbect expires. 

## Renewing a Token



| Method                    | Signaling | Real-time Messaging |
| ------------------------- | --------- | ------------------- |
| Renews the current Token. | N/A       | `onSuccess`         |



| Event                                  | Signaling | Real-time Messaging     |
| -------------------------------------- | --------- | ----------------------- |
| Returns the result of the method call. | N/A       | `onSuccess`/`onFailure` |
| The token has expired.                 | N/A       | `onTokenExpired`        |



