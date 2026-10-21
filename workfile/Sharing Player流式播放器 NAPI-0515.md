# @ohos.multimedia.khSharingPlayer接口

说明：本模块接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

投播服务为开发者提供一套简单且易于理解的接口，使得开发者能够方便接入系统并使用系统的媒体接入、预览和转分发能力。投播服务可支撑音视频对讲、投屏、拉流播放等业务。

相关模块：

- khSharingChat：用于音视频通话业务，可实现多路音视频对讲功能。
- khSharingWFD：用于投屏业务，可实现多路投屏功能。

## 导入模块

```javascript
import khsharingPlayer from '@ohos.multimedia.khSharingPlayer';
```

### createPlayer9+

createPlayer(): Player;



同步方式获取播放器实例对象。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值：**

| **类型** | **说明**       |
| -------- | -------------- |
| Player   | 播放器实例对象 |

**示例：**

```javascript
Player player = playerMgr.createPlayer();
```

## Player9+

播放管理类，用于管理和播放媒体资源。在调用player的方法前，需要先通过[createPlayer()](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#282d22ce)构建一个Player实例。



### 属性

**系统能力：** SystemCapability.Multimedia.khSharingPlayer.Core

| **名称**      | **类型**                                                     | **可读** | **可写** | **说明**                                                     |
| ------------- | ------------------------------------------------------------ | -------- | -------- | ------------------------------------------------------------ |
| playerId      | number                                                       | 是       | 否       | 播放器id，创建时生成，不可更改                               |
| url9+         | string                                                       | 是       | 是       | 媒体URL，只允许在**idle**状态下设置，静态属性。 支持的视频格式(mp4、mpeg-ts、webm、mkv)。 支持的音频格式(m4a、aac、mp3、ogg、wav)。 **支持路径示例**： 1. fd类型播放：fd://xx。 ![img](https://cdn.nlark.com/yuque/0/2023/png/32611479/1684239317221-b7d07a6e-9527-471f-9393-16871ccf1687.png) 2. http网络播放: [http://xx。](https://gitee.com/link?target=http%3A%2F%2Fxx%E3%80%82) 3. https网络播放: [https://xx。](https://gitee.com/link?target=https%3A%2F%2Fxx%E3%80%82) 4. hls网络播放路径：[http://xx或者https://xx。](https://gitee.com/link?target=http%3A%2F%2Fxx%E6%88%96%E8%80%85https%3A%2F%2Fxx%E3%80%82) |
| fdSrc9+       | [FileDescriptor](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-media.md#avfiledescriptor9) | 是       | 是       | 媒体文件描述，只允许在**idle**状态下设置，静态属性。 使用场景：应用中的媒体资源被连续存储在同一个文件中。 支持的视频格式(mp4、mpeg-ts、webm、mkv)。 支持的音频格式(m4a、aac、mp3、ogg、wav)。 **使用示例**： 假设一个连续存储的媒体文件: 视频1(地址偏移:0，字节长度:100)； 视频2(地址偏移:101，字节长度:50)； 视频3(地址偏移:151，字节长度:150)； 1. 播放视频1：AVFileDescriptor { fd = 资源句柄; offset = 0; length = 100; }。 2. 播放视频2：AVFileDescriptor { fd = 资源句柄; offset = 101; length = 50; }。 3. 播放视频3：AVFileDescriptor { fd = 资源句柄; offset = 151; length = 150; }。 假设是一个独立的媒体文件: 请使用src=fd://xx。 |
| surfaceId9+   | string                                                       | 是       | 是       | 视频窗口ID，默认无窗口，只允许在**initialized**状态下设置，静态属性。 使用场景：视频播放的窗口渲染，纯音频播放不用设置。 **使用示例**： [通过Xcomponent创建surfaceId](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-xcomponent.md#getxcomponentsurfaceid)。 |
| state9+       | [PlayerState](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-media.md#avplayerstate9) | 是       | 否       | 音视频播放的状态，全状态有效，可查询参数。                   |
| currentTime9+ | number                                                       | 是       | 否       | 视频的当前播放位置，单位为毫秒（ms），可查询参数。 返回为(-1)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。 直播场景默认返回(-1)。 |
| duration9+    | number                                                       | 是       | 否       | 视频时长，单位为毫秒（ms），可查询参数。 返回为(-1)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。 直播场景默认返回(-1)。 |
| width9+       | number                                                       | 是       | 否       | 视频宽，单位为像素（px），可查询参数。 返回为(0)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。 |
| height9+      | number                                                       | 是       | 否       | 视频高，单位为像素（px），可查询参数。 返回为(0)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。 |

**说明：**

将资源句柄（fd）传递给媒体播放器之后，请不要通过该资源句柄做其他读写操作，包括但不限于将同一个资源句柄传递给多个媒体播放器。同一时间通过同一个资源句柄读写文件时存在竞争关系，将导致播放异常。

### prepare9+

prepare(callback: AsyncCallback<void>): void;



异步方式设置播放器相关参数。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**           |
| ---------- | -------- | -------- | ------------------ |
| callback   | function | 是       | 回调，用于获取结果 |

**示例：**

```javascript
player.prepare((err) => {
    if (err == null) {
        console.info('prepare success');
    } else {
        console.error('prepare failed, error message is :' + err.message)
    }
})
```



### prepare9+

prepare(): Promise<void>;



异步方式设置播放器相关参数。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值**：

| **类型**      | **说明**                                |
| ------------- | --------------------------------------- |
| Promise<void> | 异步设置播放器相关参数的Promise返回值。 |



**示例：**

```javascript
player.prepare().then() => {
    console.info('prepare success');
}, (err) => {
    console.error('prepare failed, error message is :' + err.message)
})
```



### play9+

play(callback: AsyncCallback<void>): void;



异步方式启动指定音视频源数据的播放。play需要在prepare之后再调用，否则调用失败。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                 |
| ---------- | -------- | -------- | ------------------------ |
| callback   | function | 是       | 回调函数，用于获取结果。 |



**示例：**

```javascript
player.play((err) => {
    if (err == null) {
        console.info('play success');
    } else {
        console.error('play failed,error message is :' + err.message)
    }
})
```



### play9+

play(): Promise<void>;



异步方式启动指定音视频源数据的播放。play需要在prepare之后再调用，否则调用失败。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值**：

| **类型**      | **说明**                                      |
| ------------- | --------------------------------------------- |
| Promise<void> | 异步启动指定音视频源数据播放的Promise返回值。 |



**示例：**

```javascript
player.play().then(() => {
    console.info('play success');
}, (err) => {
    console.error('play filed, error message is :' + err.message)
})
```



### stop9+

stop(callback: AsyncCallback<void>): void;



异步方式停止指定音视频源数据的播放。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                 |
| ---------- | -------- | -------- | ------------------------ |
| callback   | function | 是       | 回调函数，用于获取结果。 |



**示例：**

```javascript
player.stop((err) => {
    if (err == null) {
        console.info('stop success');
    } else {
        console.error('stop failed, error message is :' + err.message)
    }
})
```

### stop9+

stop(): Promise<void>;



异步方式停止指定音视频源数据的播放。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值**：

| **类型**      | **说明**                              |
| ------------- | ------------------------------------- |
| Promise<void> | 异步停止播放音视频源的Promise返回值。 |



**示例：**

```javascript
player.stop().then(() => {
    console.info('stop success');
}, (err) => {
    console.error('stop failed, error message is :' + err.message)
})
```



### pause9+

pause(callback: AsyncCallback<void>): void;



异步方式暂停播放。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                 |
| ---------- | -------- | -------- | ------------------------ |
| callback   | function | 是       | 回调函数，用于获取结果。 |



**示例：**

```javascript
player.pause((err) => {
    if (err == null) {
        console.info('pause success');
    } else {
        console.error('pause failed, error message is :' + err.message)
    }
})
```



### pause9+

pause(): Promise<void>;



异步方式暂停播放。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值：**

| **类型**      | **说明**                  |
| ------------- | ------------------------- |
| Promise<void> | 暂停视频的Promise返回值。 |



**示例：**

```javascript
player.pause().then(() => {
    console.info('pause success');
}, (err) => {
    console.error('pause failed, error message is :' + err.message)
})
```



### resume9+

resume(callback: AsyncCallback<void>): void;



异步方式恢复播放。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                 |
| ---------- | -------- | -------- | ------------------------ |
| callback   | function | 是       | 回调函数，用于获取结果。 |



**示例：**

```javascript
player.resume((err) => {
    if (err == null) {
        console.info('resume success');
    } else {
        console.error('resume failed, error message is :' + err.message)
    }
})
```



### resume9+

resume(): Promise<void>;



异步方式恢复播放。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值：**

| **类型**      | **说明**                  |
| ------------- | ------------------------- |
| Promise<void> | 恢复播放的Promise返回值。 |



**示例：**

```javascript
player.resume().then(() => {
    console.info('resume success');
}, (err) => {
    console.error('resume failed, error message is :' + err.message)
})
```



### mute9+

mute(callback: AsyncCallback<void>): void;



异步方式开启静音。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                 |
| ---------- | -------- | -------- | ------------------------ |
| callback   | function | 是       | 回调函数，用于获取结果。 |



**示例：**

```javascript
player.mute((err) => {
    if (err == null) {
        console.info('mute success');
    } else {
        console.error('mute failed, error message is :' + err.message)
    }
})
```



### mute9+

mute(): Promise<void>;



异步方式开启静音。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值：**

| **类型**      | **说明**                  |
| ------------- | ------------------------- |
| Promise<void> | 开启静音的Promise返回值。 |



**示例：**

```javascript
player.mute().then(() => {
    console.info('mute success');
}, (err) => {
    console.error('mute failed, error message is :' + err.message)
})
```



### unMute9+

unMute(callback: AsyncCallback<void>): void;



异步方式对投屏流解除静音。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                 |
| ---------- | -------- | -------- | ------------------------ |
| callback   | function | 是       | 回调函数，用于获取结果。 |



**示例：**

```javascript
player.unMute((err) => {
    if (err == null) {
        console.info('unMute success');
    } else {
        console.error('unMute failed, error message is :' + err.message)
    }
})
```



### unMute9+

unMute(): Promise<void>;



异步方式对投屏流解除静音。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**返回值：**

| **类型**      | **说明**                  |
| ------------- | ------------------------- |
| Promise<void> | 解除静音的Promise返回值。 |



**示例：**

```javascript
player.unMute().then(() => {
    console.info('unMute success');
}, (err) => {
    console.error('unMute failed, error message is :' + err.message)
})
```



### seek9+

seek(timeMs: number, callback: AsyncCallback<void>): void;



异步方式跳转接口，直播时不支持跳转。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                                                    |
| ---------- | -------- | -------- | ----------------------------------------------------------- |
| timeMs     | number   | 是       | 指定的跳转时间节点，单位毫秒（ms），取值范围为[0, 视频时长] |
| callback   | function | 是       | 回调函数，用于获取结果。                                    |



**示例：**

```javascript
let seekTime = 1000;
player.seek(seekTime, (err)=>{
    if (err == null) {
        console.info('seek time success');
    } else {
        console.error('seek time failed, error message is :' + err.message)
    }
})
```



### seek9+

seek( seekTime: number): Promise<void>;



异步方式跳转接口，直播时不支持跳转。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                                                    |
| ---------- | -------- | -------- | ----------------------------------------------------------- |
| seekTime   | number   | 是       | 指定的跳转时间节点，单位毫秒（ms），取值范围为[0, 视频时长] |



**返回值：**

| **类型**      | **说明**                      |
| ------------- | ----------------------------- |
| Promise<void> | 异步跳转节点的Promise返回值。 |



**示例：**

```javascript
let seekTime = 1000;
player.seek(seekTime).then(() => {
    console.info('seek time success');
}, (err) => {
    console.error('seek time failed, error message is :' + err.message)
})
```



### setVolume9+

setVolume(volume: number, callback: AsyncCallback<void>): void;



异步方式设置媒体播放音量。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                                                     |
| ---------- | -------- | -------- | ------------------------------------------------------------ |
| volume     | number   | 是       | 指定的播放音量大小，取值范围为[0.00-1.00]，1表示最大音量，即100%。 |
| callback   | function | 是       | 回调函数，用于获取结果。                                     |



**示例：**

```javascript
let volume = 1000;
player.setVolume(volume, (err)=>{
    if (err == null) {
        console.info('set volume success');
    } else {
        console.error('set volume failed, error message is :' + err.message)
    }
})
```



### setVolume9+

setVolume(volume: number): Promise<void>;



异步方式设置媒体播放音量。通过Promise获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**                                                     |
| ---------- | -------- | -------- | ------------------------------------------------------------ |
| volume     | number   | 是       | 指定的播放音量大小，取值范围为[0.00-1.00]，1表示最大音量，即100%。 |



**返回值：**

| **类型**      | **说明**                          |
| ------------- | --------------------------------- |
| Promise<void> | 设置媒体播放音量的Promise返回值。 |



**示例：**

```javascript
let volume = 1000;
player.setVolume(volume).then(() => {
    console.info('set volume success');
}, (err) => {
    console.error('set volume failed, error message is :' + err.message)
})
```



### release9+

release(callback: AsyncCallback<void>): void;



异步方式释放播放器实例。通过注册回调函数方式获取返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型** | **必填** | **说明**             |
| ---------- | -------- | -------- | -------------------- |
| callback   | function | 是       | 释放播放的回调方法。 |



**示例：**

```javascript
player.release((err) => {
    if (err == null) {
        console.info('release success');
    } else {
        console.error('release failed, error message is :' + err.message)
    }
})
```

### release9+

release(): Promise<void>



异步方式释放播放器实例。通过Promise获取返回值。



**系统能力**: SystemCapability.Multimedia.khSharingPlayer.Core


 **返回值：**

| **类型**      | **说明**                            |
| ------------- | ----------------------------------- |
| Promise<void> | 异步释放播放器实例的Promise返回值。 |



**示例：**

```javascript
player.release().then(() => {
    console.info('release success');
}, (err) => {
    console.error('release failed, error message is :' + err.message)
})
```

### on('stateChange')9+

on(type: 'stateChange', callback: Callback<[MediaPlayerState](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#lonHr)>): void;



状态变化通知。



**系统能力**: SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型**                                                     | **必填** | **说明**                     |
| ---------- | ------------------------------------------------------------ | -------- | ---------------------------- |
| type       | string                                                       | 是       | 监听事件，固定为'error'。    |
| callback   | Callback<[MediaPlayerState](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#lonHr)> | 是       | 回调函数，用于返回播放器状态 |



**示例：**

```javascript
player.on('stateChange', (state: MediaPlayerState) => {
    switch (state) {
        case 'idle':
            console.info('state idle called')
            break;
        case 'prepared':
            console.info('state prepared called')
            break;
        case 'playing':
            console.info('state playing called')
            break;
        case 'paused':
            console.info('state paused called')
            break;
        case 'stopped':
            console.info('state stopped called')
            break;
        case 'released':
            console.info('state released called')
            break;
        case 'error':
            console.info('state error called')
            break;
        default:
            console.info('unkown state :' + state)
            break;
    }
})
```

### on('timeUpdate ')9+

on(type: 'timeUpdate', callback: Callback<[PlayerCurrentTime](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#DU5X5)>): void;



播放时间变化通知，监听资源播放当前时间，单位为毫秒（ms），用于刷新进度条当前位置，默认间隔1s时间上报，因用户操作(seek)产生的时间变化会立刻上报。 注：直播场景不支持timeUpdate上报



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型**                                                     | **必填** | **说明**                             |
| ---------- | ------------------------------------------------------------ | -------- | ------------------------------------ |
| type       | string                                                       | 是       | 监听事件，固定为'error'。            |
| callback   | Callback<[PlayerCurrentTime](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#DU5X5)> | 是       | 回调函数，用于返回当前播放进度时间。 |



**示例：**

```javascript
player.on('timeUpdate', (time:PlayerCurrentTime) => {
    console.info('timeUpdate success, player id is ' + time.playerId);
    console.info('timeUpdate success, current time is ' + time.currentTime);
})
```

### on('error')9+

on(type: 'error',  callback: ErrorCallback<[ErrorInfo](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#HsNEZ)>): void;



接口执行过程中出错，或内部运行出现错误时通知给应用，此时内部服务将停止，应用需要删除对应的资源后重新开始。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core



**参数：**

| **参数名** | **类型**                                                     | **必填** | **说明**                     |
| ---------- | ------------------------------------------------------------ | -------- | ---------------------------- |
| type       | string                                                       | 是       | 监听事件，固定为'error'。    |
| callback   | ErrorCallback<[ErrorInfo](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#HsNEZ)> | 是       | 回调函数，用于获取错误信息。 |



**示例：**

```javascript
on('error', (err)=>{
  console.log("error, player id  is" + err.playerId);
  console.log("error, code is" + err.code.code);
  console.log("error, message is" + err.code.message);
})
```

## MediaPlayerState9+

表示流式播放器当前状态。

 

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core

| **名称** | **值** | **说明** |
| -------- | ------ | -------- |
| IDLE     | 0      | 初始状态 |
| PREPARED | 1      | 已就绪   |
| PLAYING  | 2      | 播放中   |
| PAUSED   | 3      | 暂停     |
| STOPED   | 4      | 停止     |
| RELEASED | 5      | 已释放   |

## PlayerCurrentTime

`on('timeUpdate')`接口的返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core

| **成员**    | **类型** | **必填** | **说明**     |
| ----------- | -------- | -------- | ------------ |
| playerId    | number   | 是       | 播放器id。   |
| currentTime | number   | 是       | 当前播放时间 |

## ErrorInfo

`on('error')`接口的返回值。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core

| **成员** | **类型**                                                     | **必填** | **说明**   |
| -------- | ------------------------------------------------------------ | -------- | ---------- |
| playerId | number                                                       | 是       | 播放器id。 |
| code     | [ErrorCode](https://www.yuque.com/shifeilong-iyxip/kshg9i/fpvcn21gylei09gq#vxpOB) | 是       | 错误码。   |

## ErrorCode

枚举，投屏错误码。



**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core

| 名称                  | 值       | 说明           |
| --------------------- | -------- | -------------- |
| SUCCESS               | 0        | 无错误         |
| ERROR                 | 18800001 | 无类别通用错误 |
| ERROR_BAD_PARAMETER   | 18800002 | 参数不匹配     |
| ERROR_INVALID_ID      | 18800003 | 无效的ID       |
| ERROR_SERVICE_LIMIT   | 18800004 | 服务超限       |
| ERROR_STATE_EXCEPTION | 18800005 | 状态不匹配     |
| ERROR_PLAY_URL        | 18803101 | 播放地址错误   |