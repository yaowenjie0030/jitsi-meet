# Jitsi Meet API

You can use the Jitsi Meet API to embed Jitsi Meet in to your application. You are also welcome to use it for embedding the globally distributed and highly available deployment on meet.jit.si itself. The only thing we ask for in that case is that you please DO NOT remove the jitsi.org logo from the top left corner.
您可以使用Jitsi Meet API将Jitsi Meet嵌入到应用程序中。还欢迎您使用它在meet.jit上嵌入全局分布式的高可用性部署。如果本身。在这种情况下，我们只要求您不要从左上角删除jitsi.org徽标。
## Installation

To embed Jitsi Meet in your application you need to add the Jitsi Meet API library:
要在应用程序中嵌入Jitsi Meet，需要添加Jitsi Meet API库
```javascript
<script src='https://meet.jit.si/external_api.js'></script>
```
## API

### `api = new JitsiMeetExternalAPI(domain, options)`

The next step for embedding Jitsi Meet is to create the Jitsi Meet API object.
Its constructor gets a number of options:
嵌入Jitsi Meet的下一步是创建Jitsi Meet API对象。
它的构造函数有很多选项:

* **domain**: domain used to build the conference URL, 'meet.jit.si' for
  example.
  域用于构建会议URL 'meet.jit '。如果对
的例子。
* **options**: object with properties - the optional arguments:对象与属性-可选参数:
    * **roomName**: (optional) name of the room to join.(可选)要加入的房间名称
    * **width**: (optional) width for the iframe which will be created. If a number is specified it's treated as pixel units. If a string is specified the format is number followed by 'px', 'em', 'pt' or '%'.
    (可选)将要创建的iframe的宽度。如果指定了一个数字，它将被视为像素单位。如果指定了一个字符串，则格式为后跟'px'、'em'、'pt'或'%'的数字。
    * **height**: (optional) height for the iframe which will be created. If a number is specified it's treated as pixel units. If a string is specified the format is number followed by 'px', 'em', 'pt' or '%'.
    * **parentNode**: (optional) HTML DOM Element where the iframe will be added as a child.
    * **configOverwrite**: (optional) JS object with overrides for options defined in [config.js].
    * **interfaceConfigOverwrite**: (optional) JS object with overrides for options defined in [interface_config.js].
    * **noSSL**: (optional, defaults to true) Boolean indicating if the server should be contacted using HTTP or HTTPS.
    * **jwt**: (optional) [JWT](https://jwt.io/) token.
    * **onload**: (optional) handler for the iframe onload event.
    * **invitees**: (optional) Array of objects containing information about new participants that will be invited in the call.
    * **devices**: (optional) A map containing information about the initial devices that will be used in the call.

Example:

```javascript
const domain = 'meet.jit.si';
const options = {
    roomName: 'JitsiMeetAPIExample',
    width: 700,
    height: 700,
    parentNode: document.querySelector('#meet')
};
const api = new JitsiMeetExternalAPI(domain, options);
```

You can set the initial media devices for the call:
您可以为呼叫设置初始媒体设备:
```javascript
const domain = 'meet.jit.si';
const options = {
    ...
    devices: {
        audioInput: '<deviceLabel>',
        audioOutput: '<deviceLabel>',
        videoInput: '<deviceLabel>'
    },
    ...
};
const api = new JitsiMeetExternalAPI(domain, options);
```

You can overwrite options set in [config.js] and [interface_config.js].
For example, to enable the filmstrip-only interface mode, you can use:
您可以覆盖[config中设置的选项。js]和[interface_config.js]。
例如，要启用只使用胶片的界面模式，可以使用:
```javascript
const options = {
    ...
    interfaceConfigOverwrite: { filmStripOnly: true },
    ...
};
const api = new JitsiMeetExternalAPI(domain, options);
```

You can also pass a jwt token to Jitsi Meet:
您还可以传递jwt令牌给Jitsi Meet:
 ```javascript
const options = {
    ...
    jwt: '<jwt_token>',
    noSsl: false,
    ...
};
const api = new JitsiMeetExternalAPI(domain, options);
 ```

### Controlling the embedded Jitsi Meet Conference
控制嵌入式Jitsi会议
Device management `JitsiMeetExternalAPI` methods:设备管理“JitsiMeetExternalAPI”方法:
* **getAvailableDevices** - Retrieve a list of available devices.
** *getAvailableDevices** -检索可用设备列表。
```javascript
api.getAvailableDevices().then(devices => {
    // devices = {
    //     audioInput: [{
    //         deviceId: 'ID'
    //         groupId: 'grpID'
    //         kind: 'audioinput'
    //         label: 'label'
    //     },....],
    //     audioOutput: [{
    //         deviceId: 'ID'
    //         groupId: 'grpID'
    //         kind: 'audioOutput'
    //         label: 'label'
    //     },....],
    //     videoInput: [{
    //         deviceId: 'ID'
    //         groupId: 'grpID'
    //         kind: 'videoInput'
    //         label: 'label'
    //     },....]
    // }
    ...
});
```
* **getCurrentDevices** - Retrieve a list with the devices that are currently selected.
** *getCurrentDevices** -检索当前选择的设备的列表。
```javascript
api.getCurrentDevices().then(devices => {
    // devices = {
    //     audioInput: {
    //         deviceId: 'ID'
    //         groupId: 'grpID'
    //         kind: 'videoInput'
    //         label: 'label'
    //     },
    //     audioOutput: {
    //         deviceId: 'ID'
    //         groupId: 'grpID'
    //         kind: 'videoInput'
    //         label: 'label'
    //     },
    //     videoInput: {
    //         deviceId: 'ID'
    //         groupId: 'grpID'
    //         kind: 'videoInput'
    //         label: 'label'
    //     }
    // }
    ...
});
```
* **isDeviceChangeAvailable** - Resolves with true if the device change is available and with false if not.
**isDeviceChangeAvailable** -如果设备更改可用，则解析为true;如果不可用，则解析为false
```javascript
// The accepted deviceType values are - 'output', 'input' or undefined.
api.isDeviceChangeAvailable(deviceType).then(isDeviceChangeAvailable => {
    ...
});
```
* **isDeviceListAvailable** - Resolves with true if the device list is available and with false if not.
** *isDeviceListAvailable** -如果设备列表可用，则解析为true;如果不可用，解析为false
```javascript
api.isDeviceListAvailable().then(isDeviceListAvailable => {
    ...
});
```
* **isMultipleAudioInputSupported** - Resolves with true if multiple audio input is supported and with false if not.

```javascript
api.isMultipleAudioInputSupported().then(isMultipleAudioInputSupported => {
    ...
});
```
* **setAudioInputDevice** - Sets the audio input device to the one with the label or id that is passed.

```javascript
api.setAudioInputDevice(deviceLabel, deviceId);
```
* **setAudioOutputDevice** - Sets the audio output device to the one with the label or id that is passed.

```javascript
api.setAudioOutputDevice(deviceLabel, deviceId);
```
* **setVideoInputDevice** - Sets the video input device to the one with the label or id that is passed.

```javascript
api.setVideoInputDevice(deviceLabel, deviceId);
```

You can control the embedded Jitsi Meet conference using the `JitsiMeetExternalAPI` object by using `executeCommand`:

```javascript
api.executeCommand(command, ...arguments);
```

The `command` parameter is String object with the name of the command. The following commands are currently supported:

* **displayName** - Sets the display name of the local participant. This command requires one argument - the new display name to be set.
```javascript
api.executeCommand('displayName', 'New Nickname');
```

* **subject** - Sets the subject of the conference. This command requires one argument - the new subject to be set.
```javascript
api.executeCommand('subject', 'New Conference Subject');
```

* **toggleAudio** - Mutes / unmutes the audio for the local participant. No arguments are required.
```javascript
api.executeCommand('toggleAudio');
```

* **toggleVideo** - Mutes / unmutes the video for the local participant. No arguments are required.
```javascript
api.executeCommand('toggleVideo');
```

* **toggleFilmStrip** - Hides / shows the filmstrip. No arguments are required.
```javascript
api.executeCommand('toggleFilmStrip');
```

* **toggleChat** - Hides / shows the chat. No arguments are required.
```javascript
api.executeCommand('toggleChat');
```

* **toggleShareScreen** - Starts / stops screen sharing. No arguments are required.
```javascript
api.executeCommand('toggleShareScreen');
```

* **toggleTileView** - Enter / exit tile view layout mode. No arguments are required.
```javascript
api.executeCommand('toggleTileView');
```

* **hangup** - Hangups the call. No arguments are required.
```javascript
api.executeCommand('hangup');
```

* **email** - Changes the local email address. This command requires one argument - the new email address to be set.
```javascript
api.executeCommand('email', 'example@example.com');
```

* **avatarUrl** - Changes the local avatar URL. This command requires one argument - the new avatar URL to be set.
```javascript
api.executeCommand('avatarUrl', 'https://avatars0.githubusercontent.com/u/3671647');
```

You can also execute multiple commands using the `executeCommands` method:
```javascript
api.executeCommands(commands);
```
The `commands` parameter is an object with the names of the commands as keys and the arguments for the commands as values:
```javascript
api.executeCommands({
    displayName: [ 'nickname' ],
    toggleAudio: []
});
```

You can add event listeners to the embedded Jitsi Meet using the `addEventListener` method.
**NOTE: This method still exists but it is deprecated. JitsiMeetExternalAPI class extends [EventEmitter]. Use [EventEmitter] methods (`addListener` or `on`).**
```javascript
api.addEventListener(event, listener);
```

The `event` parameter is a String object with the name of the event.
The `listener` parameter is a Function object with one argument that will be notified when the event occurs with data related to the event.

The following events are currently supported:
* **cameraError** - event notifications about Jitsi-Meet having failed to access the camera. The listener will receive an object with the following structure:
```javascript
{
    type: string, // A constant representing the overall type of the error.
    message: string // Additional information about the error.
}
```

* **avatarChanged** - event notifications about avatar
changes. The listener will receive an object with the following structure:
```javascript
{
    id: string, // the id of the participant that changed his avatar.
    avatarURL: string // the new avatar URL.
}
```

* **audioAvailabilityChanged** - event notifications about audio availability status changes. The listener will receive an object with the following structure:
```javascript
{
    available: boolean // new available status - boolean
}
```

* **audioMuteStatusChanged** - event notifications about audio mute status changes. The listener will receive an object with the following structure:
```javascript
{
    muted: boolean // new muted status - boolean
}
```

* **micError** - event notifications about Jitsi-Meet having failed to access the mic. The listener will receive an object with the following structure:
```javascript
{
    type: string, // A constant representing the overall type of the error.
    message: string // Additional information about the error.
}
```

* **screenSharingStatusChanged** - receives event notifications about turning on/off the local user screen sharing. The listener will receive object with the following structure:
```javascript
{
    on: boolean, //whether screen sharing is on
    details: {

        // From where the screen sharing is capturing, if known. Values which are
        // passed include 'window', 'screen', 'proxy', 'device'. The value undefined
        // will be passed if the source type is unknown or screen share is off.
        sourceType: string|undefined
    }
}
```

* **tileViewChanged** - event notifications about tile view layout mode being entered or exited. The listener will receive object with the following structure:
```javascript
{
    enabled: boolean, // whether tile view is not displayed or not
}
```

* **incomingMessage** - Event notifications about incoming
messages. The listener will receive an object with the following structure:
```javascript
{
    from: string, // The id of the user that sent the message
    nick: string, // the nickname of the user that sent the message
    message: string // the text of the message
}
```

* **outgoingMessage** - Event notifications about outgoing
messages. The listener will receive an object with the following structure:
```javascript
{
    message: string // the text of the message
}
```

* **displayNameChange** - event notifications about display name
changes. The listener will receive an object with the following structure:
```javascript
{
    id: string, // the id of the participant that changed his display name
    displayname: string // the new display name
}
```

* **deviceListChanged** - event notifications about device list changes. The listener will receive an object with the following structure:
```javascript
{
    devices: Object // the new list of available devices.
}
```
NOTE: The devices object has the same format as the getAvailableDevices result format.

* **emailChange** - event notifications about email
changes. The listener will receive an object with the following structure:
```javascript
{
    id: string, // the id of the participant that changed his email
    email: string // the new email
}
```
* **filmstripDisplayChanged** - event notifications about the visibility of the filmstrip being updated.
```javascript
{
    visible: boolean // Whether or not the filmstrip is displayed or hidden.
}
```

* **participantJoined** - event notifications about new participants who join the room. The listener will receive an object with the following structure:
```javascript
{
    id: string, // the id of the participant
    displayName: string // the display name of the participant
}
```

* **participantLeft** - event notifications about participants that leave the room. The listener will receive an object with the following structure:
```javascript
{
    id: string // the id of the participant
}
```

* **videoConferenceJoined** - event notifications fired when the local user has joined the video conference. The listener will receive an object with the following structure:
```javascript
{
    roomName: string, // the room name of the conference
    id: string, // the id of the local participant
    displayName: string, // the display name of the local participant
    avatarURL: string // the avatar URL of the local participant
}
```

* **videoConferenceLeft** - event notifications fired when the local user has left the video conference. The listener will receive an object with the following structure:
```javascript
{
    roomName: string // the room name of the conference
}
```

* **videoAvailabilityChanged** - event notifications about video availability status changes. The listener will receive an object with the following structure:
```javascript
{
    available: boolean // new available status - boolean
}
```

* **videoMuteStatusChanged** - event notifications about video mute status changes. The listener will receive an object with the following structure:
```javascript
{
    muted: boolean // new muted status - boolean
}
```

* **readyToClose** - event notification fired when Jitsi Meet is ready to be closed (hangup operations are completed).

* **subjectChange** - event notifications about subject of conference changes.
The listener will receive an object with the following structure:
```javascript
{
    subject: string // the new subject
}
```

You can also add multiple event listeners by using `addEventListeners`.
This method requires one argument of type Object. The object argument must
have the names of the events as keys and the listeners of the events as values.
**NOTE: This method still exists but it is deprecated. JitsiMeetExternalAPI class extends [EventEmitter]. Use [EventEmitter] methods.**

```javascript
function incomingMessageListener(object)
{
// ...
}

function outgoingMessageListener(object)
{
// ...
}

api.addEventListeners({
    incomingMessage: incomingMessageListener,
    outgoingMessage: outgoingMessageListener
});
```

If you want to remove a listener you can use `removeEventListener` method with argument the name of the event.
**NOTE: This method still exists but it is deprecated. JitsiMeetExternalAPI class extends [EventEmitter]. Use [EventEmitter] methods( `removeListener`).**
```javascript
api.removeEventListener('incomingMessage');
```

If you want to remove more than one event you can use `removeEventListeners` method with an Array with the names of the events as an argument.
**NOTE: This method still exists but it is deprecated. JitsiMeetExternalAPI class extends [EventEmitter]. Use [EventEmitter] methods.**
```javascript
api.removeEventListeners([ 'incomingMessage', 'outgoingMessageListener' ]);
```

You can get the number of participants in the conference with the following API function:
```javascript
const numberOfParticipants = api.getNumberOfParticipants();
```

You can get the avatar URL of a participant in the conference with the following API function:
```javascript
const avatarURL = api.getAvatarURL(participantId);
```

You can get the display name of a participant in the conference with the following API function:
```javascript
const displayName = api.getDisplayName(participantId);
```

You can get the email of a participant in the conference with the following API function:
```javascript
const email = api.getEmail(participantId);
```

You can get the iframe HTML element where Jitsi Meet is loaded with the following API function:
```javascript
const iframe = api.getIFrame();
```

You can check whether the audio is muted with the following API function:
```javascript
api.isAudioMuted().then(muted => {
    ...
});
```

You can check whether the video is muted with the following API function:
```javascript
api.isVideoMuted().then(muted => {
    ...
});
```

You can check whether the audio is available with the following API function:
```javascript
api.isAudioAvailable().then(available => {
    ...
});
```

You can check whether the video is available with the following API function:
```javascript
api.isVideoAvailable().then(available => {
    ...
});
```

You can invite new participants to the call with the following API function:
```javascript
api.invite([ {...}, {...}, {...} ]).then(() => {
    // success
}).catch(() => {
    // failure
});
```
**NOTE: The format of the invitees in the array depends on the invite service used for the deployment.**
**注意:数组中受邀者的格式取决于用于部署的invite服务
You can remove the embedded Jitsi Meet Conference with the following API function:
您可以使用以下API函数删除嵌入式Jitsi meeting会议:
```javascript
api.dispose();
```

NOTE: It's a good practice to remove the conference before the page is unloaded.
注意:在卸载页面之前删除会议是一个很好的实践。
[config.js]: https://github.com/jitsi/jitsi-meet/blob/master/config.js
[interface_config.js]: https://github.com/jitsi/jitsi-meet/blob/master/interface_config.js
[EventEmitter]: https://nodejs.org/api/events.html
