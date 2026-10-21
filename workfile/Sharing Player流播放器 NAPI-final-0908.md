<a name="NxExh"></a>
# @ohos.multimedia.khSharingPlayer接口
投播服务为开发者提供一套简单且易于理解的接口，使得开发者能够方便接入系统并使用系统的媒体接入、预览和转分发能力。投播服务可支撑音视频对讲、投屏、拉流播放等业务。<br />Sharing部件包含了流播放业务，提供以下常用功能：

- [AVPlayer](#O3P3v)：流播放器，支持多路并行播放。目标支持本地文件播放和拉流播放。当前仅实现RTSP协议拉流播放功能。

**相关模块：**

- khSharingChat：用于音视频通话业务，可实现多路音视频对讲功能。
- khSharingWFD：用于投屏业务，可实现多路投屏功能。

>**说明**：本模块接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
<a name="ca4ef8a7"></a>
## 导入模块
```javascript
import khSharingPlayer from '@ohos.multimedia.khSharingPlayer';
```
<a name="282d22ce"></a>
### khSharingPlayer.createAVPlayer
createAVPlayer(): [AVPlayer](#O3P3v);

同步方式获取播放器实例对象。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core

**返回值：**

| **类型** | **说明** |
| --- | --- |
| [AVPlayer](#O3P3v) | 返回AVPlayer类实例，失败时返回null。可用于媒体播放、暂停、停止等操作。 |

**错误码：**

以下错误码的详细介绍请参见[错误码](#ErrorCode)

| **错误码ID** | **错误信息** |
| --- | --- |
| 18800004 | Service limit. Return by callback. |

**示例：**
```javascript
let avPlayer = khSharingPlayer.createAVPlayer();
```
<a name="O3P3v"></a>
## AVPlayer
媒体播放管理类，用于管理和播放一路媒体资源。在调用player的方法前，需要先通过[createAVPlayer()](#282d22ce)构建一个AVPlayer实例。
<a name="Zlg0X"></a>
### 属性
**系统能力：** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| url | string | 是 | 是 | 媒体URL，静态属性。当前支持RTSP协议拉流播放。<br />支持路径示例：<br />1.fd类型播放：fd://xx。<br />    ![](https://cdn.nlark.com/yuque/0/2023/png/32611479/1684239317221-b7d07a6e-9527-471f-9393-16871ccf1687.png#averageHue=%23fdfcfb&clientId=uf0090c95-d687-4&from=paste&id=cPEJI&originHeight=89&originWidth=179&originalType=url&ratio=1.2000000476837158&rotation=0&showTitle=false&status=done&style=none&taskId=u2d5a5367-b2f6-4d56-aca3-36df8df24ae&title=)<br />2. rtsp网络播放路径：rtsp[://xx](https://gitee.com/link?target=http%3A%2F%2Fxx%25E6%2588%2596%25E8%2580%2585https%3A%2F%2Fxx%25E3%2580%2582)。 |
| fdSrc | [AVFileDescriptor](#UM9Iz) | 是 | 是 | 媒体文件描述，静态属性。<br />使用场景：应用中的媒体资源被连续存储在同一个文件中。<br />支持的视频格式(mp4、mpeg-ts、webm、mkv)。<br />支持的音频格式(m4a、aac、mp3、ogg、wav)。<br />**使用示例**：<br />假设一个连续存储的媒体文件:<br />视频1(地址偏移:0，字节长度:100)；<br />视频2(地址偏移:101，字节长度:50)；<br />视频3(地址偏移:151，字节长度:150)；<br />1. 播放视频1：AVFileDescriptor { fd = 资源句柄; offset = 0; length = 100; }。<br />2. 播放视频2：AVFileDescriptor { fd = 资源句柄; offset = 101; length = 50; }。<br />3. 播放视频3：AVFileDescriptor { fd = 资源句柄; offset = 151; length = 150; }。<br />假设是一个独立的媒体文件: 请使用src=fd://xx。 |
| surfaceId | string | 是 | 是 | 视频窗口的surface ID，静态属性。设置一次即可。用于视频播放的窗口渲染，音频播放无需设置。 |
| muteState | [AVMuteState](#Sa2SK) | 是 | 是 | 静音状态，静态属性。 |
| state | [AVPlayerState](#lonHr) | 是 | 否 | 音视频播放的状态，全状态有效，可查询参数。 |
| currentTime | number | 是 | 否 | 视频的当前播放位置，单位为毫秒（ms），可查询参数。<br />返回为(-1)表示无效值，PREPARED/PLAYING/PAUSED状态下有效。<br />直播场景默认返回(-1)。 |
| duration | number | 是 | 否 | 视频时长，单位为毫秒（ms），可查询参数。<br />返回为(-1)表示无效值，PREPARED/PLAYING/PAUSED状态下有效。<br />直播场景默认返回(-1)。 |
| width | number | 是 | 否 | 视频宽，单位为像素（px），可查询参数。<br />返回为(0)表示无效值，PREPARED/PLAYING/PAUSED状态下有效。 |
| height | number | 是 | 否 | 视频高，单位为像素（px），可查询参数。<br />返回为(0)表示无效值，PREPARED/PLAYING/PAUSED状态下有效。 |

**说明：**将资源句柄（fd）传递给媒体播放器之后，请不要通过该资源句柄做其他读写操作，包括但不限于将同一个资源句柄传递给多个媒体播放器。同一时间通过同一个资源句柄读写文件时存在竞争关系，将导致播放异常。另外surface ID仅需设置一次，重复设置可能会引起视频无法渲染，可尝试在下一次prepare之前调用。
<a name="nxJ7K"></a>
### prepare
prepare(callback: AsyncCallback<void>): void;

异步方式准备播放器资源。通过注册回调函数方式获取返回值。此函数调用之前需设置好url、fdSrc、surfaceId等静态属性。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.prepare((err) => {
    if (err == null) {
        console.info('prepare success');
    } else {
        console.error('prepare failed, error code is :' + err.code)
    }
})
```
<a name="fTtuZ"></a>
### prepare
prepare(): Promise<void>;

异步方式准备播放器资源。通过Promise获取返回值。此函数调用之前需设置好url、fdSrc、surfaceId等静态属性。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 准备播放器资源的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.prepare().then() => {
    console.info('prepare success');
}, (err) => {
    console.error('prepare failed, error code is :' + err.code)
})
```
<a name="f62cf29d"></a>
### play
play(callback: AsyncCallback<void>): void;

异步方式开始播放音视频资源。需要在prepare之后再调用，否则调用失败。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.play((err) => {
    if (err == null) {
        console.info('play success');
    } else {
        console.error('play failed, error code is :' + err.code)
    }
})
```
<a name="f62cf29d-1"></a>
### play
play(): Promise<void>;

异步方式开始播放音视频资源。需要在prepare之后再调用，否则调用失败。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 开始播放音视频资源的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.play().then(() => {
    console.info('play success');
}, (err) => {
    console.error('play filed, error code is :' + err.code)
})
```
<a name="c267e175"></a>
### stop
stop(callback: AsyncCallback<void>): void;

异步方式停止播放音视频资源。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.stop((err) => {
    if (err == null) {
        console.info('stop success');
    } else {
        console.error('stop failed, error code is :' + err.code)
    }
})
```
<a name="c267e175-1"></a>
### stop
stop(): Promise<void>;

异步方式停止播放音视频资源。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 停止播放音视频资源的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.stop().then(() => {
    console.info('stop success');
}, (err) => {
    console.error('stop failed, error code is :' + err.code)
})
```
<a name="59205e50"></a>
### pause
pause(callback: AsyncCallback<void>): void;

异步方式暂停播放音视频资源。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.pause((err) => {
    if (err == null) {
        console.info('pause success');
    } else {
        console.error('pause failed, error code is :' + err.code)
    }
})
```
<a name="59205e50-1"></a>
### pause
pause(): Promise<void>;

异步方式暂停播放音视频资源。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 暂停播放音视频资源的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.pause().then(() => {
    console.info('pause success');
}, (err) => {
    console.error('pause failed, error code is :' + err.code)
})
```
<a name="3b268a8b"></a>
### resume
resume(callback: AsyncCallback<void>): void;

异步方式恢复暂停播放的音视频资源。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.resume((err) => {
    if (err == null) {
        console.info('resume success');
    } else {
        console.error('resume failed, error code is :' + err.code)
    }
})
```
<a name="3b268a8b-1"></a>
### resume
resume(): Promise<void>;

异步方式恢复暂停播放的音视频资源。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 恢复暂停播放音视频资源的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.resume().then(() => {
    console.info('resume success');
}, (err) => {
    console.error('resume failed, error code is :' + err.code)
})
```
<a name="9babd5b0"></a>
### mute
mute(callback: AsyncCallback<void>): void;

异步方式开启静音。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.mute((err) => {
    if (err == null) {
        console.info('mute success');
    } else {
        console.error('mute failed, error code is :' + err.code)
    }
})
```
<a name="9babd5b0-1"></a>
### mute
mute(): Promise<void>;

异步方式开启静音。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 开启静音的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.mute().then(() => {
    console.info('mute success');
}, (err) => {
    console.error('mute failed, error code is :' + err.code)
})
```
<a name="dabee4c7"></a>
### unMute
unMute(callback: AsyncCallback<void>): void;

异步方式解除静音。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
avPlayer.unMute((err) => {
    if (err == null) {
        console.info('unMute success');
    } else {
        console.error('unMute failed, error code is :' + err.code)
    }
})
```
<a name="dabee4c7-1"></a>
### unMute
unMute(): Promise<void>;

异步方式解除静音。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 解除静音的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.unMute().then(() => {
    console.info('unMute success');
}, (err) => {
    console.error('unMute failed, error code is :' + err.code)
})
```
<a name="01f3089c"></a>
### seek
seek(timeMs: number, callback: AsyncCallback<void>): void;

异步方式跳转到指定播放位置，直播流时不支持跳转。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| timeMs | number | 是 | 指定的跳转时间节点，单位毫秒（ms），取值范围为[0, duration]。 |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
let seekTime = 1000;
avPlayer.seek(seekTime, (err)=>{
    if (err == null) {
        console.info('seek time success');
    } else {
        console.error('seek time failed, error code is :' + err.code)
    }
})
```
<a name="01f3089c-1"></a>
### seek
seek( timeMs: number): Promise<void>;

异步方式跳转到指定播放位置，直播流时不支持跳转。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| timeMs | number | 是 | 指定的跳转时间节点，单位毫秒（ms），取值范围为[0, duration]。  |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 跳转到指定播放位置的Promise返回值。 |

**示例：**
```javascript
// promise
let seekTime = 1000;
avPlayer.seek(seekTime).then(() => {
    console.info('seek time success');
}, (err) => {
    console.error('seek time failed, error code is :' + err.code)
})
```
<a name="803f8a1a"></a>
### setVolume(暂未实现)
setVolume(volume: number, callback: AsyncCallback<void>): void;

异步方式设置音视频资源的播放音量。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| volume | number | 是 | 指定的播放音量大小，取值范围为[0.00-1.00]，1表示最大音量，即100%。 |
| callback | function | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asyncallback
let volume = 1.0;
avPlayer.setVolume(volume, (err)=>{
    if (err == null) {
        console.info('set volume success');
    } else {
        console.error('set volume failed, error code is :' + err.code)
    }
})
```
<a name="803f8a1a-1"></a>
### setVolume(暂未实现)
setVolume(volume: number): Promise<void>;

异步方式设置音视频资源的播放音量。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| volume | number | 是 | 指定的播放音量大小，取值范围为[0.00-1.00]，1表示最大音量，即100%。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 设置音视频资源播放音量的Promise返回值。 |

**示例：**
```javascript
// promise
let volume = 1.0;
avPlayer.setVolume(volume).then(() => {
    console.info('set volume success');
}, (err) => {
    console.error('set volume failed, error code is :' + err.code)
})
```
<a name="q2jc1"></a>
### release
release(callback: AsyncCallback<void>): void;

异步方式释放播放器实例。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | function | 是 | 释放播放器的回调方法。 |

**示例：**
```javascript
// asyncallback
avPlayer.release((err) => {
    if (err == null) {
        console.info('release success');
    } else {
        console.error('release failed, error code is :' + err.code)
    }
})
```
<a name="pLUlZ"></a>
### release
release(): Promise<void>

异步方式释放播放器实例。通过Promise获取返回值。

**系统能力**: SystemCapability.Multimedia.khSharingPlayer.AVPlayer

 **返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 释放播放器实例的Promise返回值。 |

**示例：**
```javascript
// promise
avPlayer.release().then(() => {
    console.info('release success');
}, (err) => {
    console.error('release failed, error code is :' + err.code)
})
```
<a name="faf8796a"></a>
### on('stateChange')
on(type: 'stateChange', callback: Callback<[AVPlayerState](#lonHr)>): void;

监听流播放器状态变化事件，通过回调获取当前播放器状态。

**系统能力**: SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'error'。 |
| callback | Callback<[AVPlayerState](#lonHr)> | 是 | 回调函数，用于返回播放器状态。 |

**示例：**
```javascript
avPlayer.on('stateChange', (state) => {
    switch (state) {
        case khSharingPlayer.AVPlayerState.IDLE:
            console.info('state idle called');
            break;
        case khSharingPlayer.AVPlayerState.PREPARED:
            console.info('state prepared called');
            break;
        case khSharingPlayer.AVPlayerState.PLAYING:
            console.info('state playing called');
            break;
        case khSharingPlayer.AVPlayerState.PAUSED:
            console.info('state paused called');
            break;
        case khSharingPlayer.AVPlayerState.STOPPED:
            console.info('state stopped called');
            break;
        case khSharingPlayer.AVPlayerState.RELEASED:
            console.info('state released called');
            break;
        default:
            console.info('unkown state :' + state);
            break;
    }
})
```
<a name="cbc8853b"></a>
### on('timeUpdate ')
on(type: 'timeUpdate', callback: Callback<number>): void;

播放时间变化通知，监听音视频资源播放当前时间，单位为毫秒（ms），处于播放状态时，用于刷新进度条当前位置，默认间隔1s时间上报，因用户操作([seek](#01f3089c))产生的时间变化会立刻上报。 注：直播场景不支持timeUpdate上报。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监播放事件回调类型，支持的事件包括：'timeUpdate'。<br />- 'timeUpdate'：音频播放时间戳更新，开始播放后自动触发该事件。 |
| callback | Callback<number> | 是 | 播放事件回调方法。回调方法入参为更新后的时间戳。 |

**示例：**
```javascript
avPlayer.on('timeUpdate', (newTime) => {    //设置'timeUpdate'事件回调
  if (newTime == null) {
    console.info('audio timeUpadate fail');
    return;
  }
  console.log('audio timeUpadate success. seekDoneTime: ' + newTime);
});
avPlayer.play();    //开始播放后，自动触发时间戳更新事件
```
<a name="iV6Ah"></a>
### on('durationUpdate')
on(type: 'durationUpdate', callback: Callback<number>): void

监听音视频资源的时长，单位为毫秒（ms），用于刷新进度条长度，默认只在prepared上报一次，同时允许一些特殊码流刷新多次时长。 注：直播场景不支持durationUpdate上报。

**系统能力：** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 时长更新的回调类型，支持的事件：'durationUpdate'。 |
| callback | function | 是 | 资源时长。 |

**示例：**
```javascript
avPlayer.on('durationUpdate', (duration) => {
  console.info('durationUpdate success, new duration is :' + duration)
})
```
<a name="iPHa5"></a>
### on('videoSizeChange')
on(type: 'videoSizeChange', callback: (width: number, height: number) => void): void

监听视频播放宽高变化事件，仅视频播放支持该订阅事件，默认只在prepread状态上报一次，但码流分辨率切换时也会上报；

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 视频播放宽高变化事件回调类型，支持的事件：'videoSizeChange'。 |
| callback | function | 是 | 视频播放宽高变化事件回调方法，width表示宽，height表示高。 |

**示例：**
```javascript
avPlayer.on('videoSizeChange', (width: number, height: number) => {
  console.info('videoSizeChange success,and width is:' + width + ', height is :' + height)
})
```
<a name="65823c28"></a>
### on('error')
on(type: 'error',  callback: ErrorCallback): void;

接口执行过程中出错，或内部运行出现错误时通知给应用，此时内部服务将停止，应用需要删除对应的资源后重新开始。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'error'。 |
| callback | ErrorCallback | 是 | 回调函数，用于获取错误信息。 |

**示例：**
```javascript
on('error', (err)=>{
  console.log("error, code is" + err.code);// ErrorCode
})
```
<a name="UM9Iz"></a>
## AVFileDescriptor
音视频文件资源描述，一种特殊资源的播放方式，使用场景：应用中的音频资源被连续存储在同一个文件中，需要根据偏移量和长度进行播放。

**系统能力：** SystemCapability.Multimedia.khSharingPlayer.Core

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| fd | number | 是 | 资源句柄，通过resourceManager.getRawFileDescriptor获取。 |
| offset | number | 是 | 资源偏移量，需要基于预置资源的信息输入，非法值会造成音视频资源解析错误。 |
| length | number | 是 | 资源长度，需要基于预置资源的信息输入，非法值会造成音视频资源解析错误。 |

<a name="Sa2SK"></a>
## AVMuteState
表示流播放器当前静音状态的枚举。可通过mute属性主动获取当前静音状态，也可通过[mute()](#9babd5b0)方法变更静音状态。<br /> <br />**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

| **名称** | **值** | **说明** |
| --- | --- | --- |
| UNMUTE | 0 | 初始状态，非静音状态， |
| MUTE | 1 | 静音状态。 |

<a name="lonHr"></a>
## AVPlayerState
表示流播放器当前状态的枚举。可通过state属性主动获取当前状态，也可通过监听stateChange事件上报当前状态，状态机之间的切换规则。<br /> <br />**系统能力:** SystemCapability.Multimedia.khSharingPlayer.AVPlayer

| **名称** | **值** | **说明** |
| --- | --- | --- |
| IDLE | 0 | 初始状态，闲置状态，AVPlayer刚被创建createAVPlayer()或者调用了reset()方法之后，进入Idle状态。<br />首次创建createAVPlayer()，所有属性都为默认值。<br />调用stop()方法，url 、 fdSrc，mute、surface ID属性将会被保存，其他用户设置的属性将被重置。 |
| PREPARED | 1 | 已准备状态。此时不可以再配置 url、fdSrc、surfaceId等静态属性。 |
| PLAYING | 2 | 正在播放状态，在PREPARED/PAUSED状态调用play()方法，AVPlayer会进入PLAYING状态。 |
| PAUSED | 3 | 暂停状态，在PLAYING状态调用pause方法，AVPlayer会进入PAUSED状态。 |
| STOPPED | 4 | 停止状态，在PREPARED/PLAYING/PAUSED状态调用stop()方法，AVPlayer会进入STOPPED状态，此时播放引擎只会保留属性，但会释放内存资源，可以调用prepare()重新准备，或者调用release()彻底销毁。 |
| RELEASED | 5 | 销毁状态，销毁与当前AVPlayer关联的播放引擎，无法再进行状态转换，调用release()方法后，会进入RELEASED状态，结束流程。 |

<a name="ErrorCode"></a>
## ErrorCode
流播放器错误码枚举。

**系统能力:** SystemCapability.Multimedia.khSharingPlayer.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| ERR_OK | 0 | 无错误 |
| ERR_GENERAL_ERROR | 18800001 | 无类别通用错误 |
| ERR_BAD_PARAMETER | 18800002 | 参数不匹配 |
| ERR_INVALID_ID | 18800003 | 无效的ID |
| ERR_SERVICE_LIMIT | 18800004 | 服务超限 |
| ERR_STATE_EXCEPTION | 18800005 | 状态不匹配 |
| ERR_CONNECTION_FAILURE | 18800006 | 连接失败 |
| ERR_INTERACTION_FAILURE | 18800007 | 交互失败 |
| ERR_CONNECTION_TIMEOUT | 18800008 | 连接超时 |
| ERR_CONNECTION_REFUSED | 18800009 | 连接拒绝 |
| ERR_BAD_PROTOCOL | 18800010 | 协议不支持 |
| ERR_NETWORK_ERROR | 18800011 | 网络异常 |
| ERR_UNAUTHORIZED | 18800012 | 认证错误 |
| ERR_INVALID_URL | 18800013 | 无效URL |
| ERR_INVALID_SURFACE_ID | 18800101 | 无效surface ID |
| ERR_SURFACE_FAILURE | 18800102 | surface失效 |
| ERR_DECODE_ERROR | 18800601 | 解码错误 |
| ERR_DECODE_FORMAT | 18800602 | 解码格式不支持 |
| ERR_DECODE_SCARCE_CAPACITY | 18800603 | 解码能力不足 |
| ERR_PLAY_START | 18800608 | 播放启动错误 |
| ERR_PLAY_STOP | 18800609 | 播放停止错误 |

