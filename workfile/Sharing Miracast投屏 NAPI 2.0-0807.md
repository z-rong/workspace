<a name="30e02040"></a>
# @ohos.multimedia.khSharingWfd接口
投播服务为开发者提供一套简单且易于理解的接口，使得开发者能够方便接入系统并使用系统的媒体接入、预览和转分发能力。投播服务可支撑音视频对讲、投屏、拉流播放、推流等业务。<br />Miracast镜像投屏模块，支持多台Wi-Fi直连（Wi-Fi Direct)的设备，不需要无线热点AP，直接使用Wi-Fi直连(即wifip2p技术)进行连接。连接成功之后将其中一台(或者多台)设备的画面和声音，直接分享到另外一台设备进行展示。设备规格分为两类，一类为传送端Source，另一类为接收端Sink。该模块提供以下常用功能：

- [WfdSink](#WfdSink)：被投端接收器用于Sink端业务，可同时接收多路Miracast投屏流。

后续提供以下功能：WfdSource、反控、录像等功能。<br />**相关模块**：

- khSharingChat：用于音视频通话业务，可实现多路音视频通话功能。
- khSharingPlayer：用于流播放业务，可实现RTSP协议拉流播放功能。

>**说明**：本模块接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。<br />文档版本：2.0
<a name="ca4ef8a7"></a>
## 导入模块
```javascript
import khsharingWfd from '@ohos.multimedia.khSharingWfd';
```
<a name="sharing.createWfdSink"></a>
## khsharingWfd.createSink
createSink(): WfdSink

创建Miracast投屏接收器。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

**返回值：**

| **类型** | **说明** |
| --- | --- |
| [WfdSink](#WfdSink) | 返回一个Miracast投屏接收器实例。 |

**示例：**
```javascript
// synccallback
let wfdSink = khsharingWfd.createSink();
```
<a name="WfdSurfaceDisplayType"></a>
## SceneType
表示播放窗口渲染场景类型的枚举，渲染场景区分为前景和后景方式。

- 前景渲染模式：正常帧率解码送显；
- 后景渲染模式：仅关键帧解码送显，节省系统资源，同等硬件资源可接入更多的投屏设备。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| FOREGROUND | 0 | 前景模式 |
| BACKGROUND | 1 | 后景模式 |

<a name="dZVPp"></a>
## CodecType
表示媒体编解码类型的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| CODEC_H264	   | 1 | 视频编码格式 |
| CODEC_H265 | 2 | 视频编码格式 |
| CODEC_AAC | 3 | 音频编码格式 |

<a name="mr91u"></a>
## VideoFormat
表示视频协商参数的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| VIDEO_640x480_60 | 2 | 视频编码格式480P，60帧 |
| VIDEO_1280x720_25 | 3 | 视频编码格式720P，25帧 |
| VIDEO_1280x720_30 | 4 | 音频编码格式720P，30帧 |
| VIDEO_1920x1080_25 | 6 | 音频编码格式1080P，25帧 |
| VIDEO_1920x1080_30 | 7 | 视频编码格式1080P，30帧 |

<a name="JG81D"></a>
## AudioFormat
表示音频协商参数的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| AUDIO_48000_16_2 | 43 | 采样率48000 ，16位，双通道 |
| AUDIO_48000_16_4 | 44 | 采样率48000 ，16位，四通道 |

<a name="WfdVideoFormat"></a>
## CodecAttr
表示媒体协商的音视频属性。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
|  codecType | [CodecType](#dZVPp) | 是 | 编码类型。 |
|  format | number | 是 | 音视频协商参数枚举值。 |

<a name="WfdConnectionState"></a>
## DeviceState
表示WFD设备状态的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| CONNECTED | 0 | 已连接 |
| DISCONNECTED | 1 | 断开连接 |
| INIT | 2 | 初始状态 |
| READY | 3 | 就绪状态 |
| PLAYING | 4 | 播放状态 |
| PAUSEED | 5 | 暂停播放状态 |
| STOPED | 6 | 停止播放状态 |

<a name="WfdConnectionInfo"></a>
## ConnectionInfo
表示 WFD设备连接信息。

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| ipAddr | string | 是 | 设备的ip地址。 |
| macAddr | string | 是 | 设备的mac地址。 |
| state | [DeviceState](#WfdConnectionState) | 是 | 设备状态。 |
| surfaceId | string | 否 | surface ID。 |
| deviceName | string | 否 | 设备名称。 |
| primaryDeviceType | string | 否 | 主设备类型。 |
| secondaryDeviceType | string | 否 | 从设备类型。 |

<a name="OQ9Cb"></a>
## SceneConfig
场景配置。<br />**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| foregroundMaximum | number | 是 | 最大前景数。 |
| maximum | number | 是 | 最大前后景数。 |

<a name="WfdSink"></a>
## WfdSink
Miracast投屏接收器管理类，用于Sink端业务，在调用WfdSink的方法前，需要先通过createSink()构建一个WfdSink实例。
<a name="Zzgbd"></a>
### getConfig
getConfig(callback: AsyncCallback<[SceneConfig](#OQ9Cb)>): void

异步方式获取投屏接收器的配置信息。通过注册回调函数获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<[SceneConfig](#OQ9Cb)> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asynccallback
wfdSink.getConfig(err, config) => {
    if (err == null) {
        console.info('getConfig success.');
    } else {
        console.error('getConfig failed, error code is :' + err.code); //ErrorCode
    }
})

```
<a name="c3gCq"></a>
### getConfig
getConfig(): : Promise<[SceneConfig](#OQ9Cb)>;

异步方式获取投屏接收器的配置信息。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise<[SceneConfig](#OQ9Cb)> | 使用Promise的方式获取结果，返回配置信息。 |

**示例：**
```javascript
// promise
wfdSink.getConfig().then((sinkConfig) => {
    console.info('getConfig success.');
}).catch((err) => {
    console.error('getConfig filed, error code is :' + err.code); //ErrorCode
});
```
<a name="start"></a>
### start
start(callback: AsyncCallback\<void>): void;

异步方式启用Miracast投屏接收器服务。通过注册回调函数获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asynccallback
wfdSink.start((err) => {
    if (err == null) {
        console.info('start success.');
    } else {
        console.info('start failed, error code is :' + err.code); //ErrorCode
    }
});
```
<a name="cTWRx"></a>
### start
start(): Promise\<void>;

异步方式启用Miracast投屏接收器服务。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 异步启动投屏接收器服务的Promise返回值。 |

**示例：**
```javascript
// promise
wfdSink.start().then(() => {
    console.info('start success.');
}).catch((err) => {
    console.error('start filed, error code is :' + err.code); //ErrorCode
});
```
<a name="stop"></a>
### stop
stop(callback: AsyncCallback\<void>): void;

异步方式停止Miracast投屏接收器服务。通过注册回调函数获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
// asynccallback
wfdSink.stop((err) => {
    if (err == null) {
        console.info('stop success.');
    } else {
        console.error('stop failed, error code is :' + err.code); //ErrorCode
    }
});
```
<a name="emtIq"></a>
### stop
stop(): Promise\<void>;

异步方式停止Miracast投屏接收器服务。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 异步停止投屏接收端服务的Promise返回值。 |

**示例：**
```javascript
// promise
wfdSink.stop().then(() => {
    console.info('stop success.');
}).catch((err) => {
    console.error('stop filed, error code is :' + err.code); //ErrorCode
});
```
<a name="hcPr1"></a>
### appendSurface
appendSurface(deviceId: string, surfaceId: string, callback: AsyncCallback\<void>): void;

异步方式为设备添加一个预览窗口Surface ID，同一个设备可以添加多个预览窗口Surface ID。通过注册回调函数方式获取返回值。首次添加Surface ID，必须通过on('connectionChanged')的CONNECTED事件通知被动调用。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| surfaceId | string | 是 | surface ID。 |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**错误码：**

以下错误码的详细介绍请参见[投屏错误码](#RiDvJ)

| **错误码ID** | **错误信息** |
| --- | --- |
| 18800004 | Service limit. Return by callback. |
| 18800005 | State exception. Return by callback. |

**示例：**
```javascript
let wfdSink = khsharing.createSink();
let deviceId = '00-FF-E8-94-94-8A'
let surfaceId = '20001'

// asynccallback
wfdSink.appendSurface(deviceId, surfaceId, (err)=>{
    if (err == null) {
        console.info('appendSurface success.');
    } else {
        console.error('appendSurface failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="iZGPl"></a>
### appendSurface
appendSurface(deviceId: string, surfaceId: string): Promise\<void>;

异步方式为设备添加一个预览窗口Surface ID，同一个设备可以添加多个预览窗口Surface ID。通过Promise获取返回值。首次添加Surface ID，必须通过on('connectionChanged')的CONNECTED事件通知被动调用。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| surfaceId | string | 是 | surface ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 添加预览窗口surface ID的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[投屏错误码](#RiDvJ)

| **错误码ID** | **错误信息** |
| --- | --- |
| 18800004 | Service limit. Return by promise. |
| 18800005 | State exception. Return by promise. |

**示例：**
```javascript
let wfdSink = khsharing.createWfdSink();
let deviceId = '00-FF-E8-94-94-8A'
let surfaceId = '20001'

// promise
wfdSink.appendSurface(deviceId, surfaceId).then(() => {
    console.info('appendSurface success.');
}).catch((err) => {
    console.error('appendSurface filed, error code is :' + err.code); //ErrorCode
});
```
<a name="wh1oM"></a>
### removeSurface
removeSurface(deviceId: string, surfaceId: string, callback: AsyncCallback\<void>): void;

异步方式移除设备的预览窗口Surface ID。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| surfaceId | string | 是 | surfacd ID。 |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
let wfdSink = khsharing.createSink();
let deviceId = '00-FF-E8-94-94-8A'
let surfaceId = '20001'

// asynccallback
wfdSink.removeSurface(deviceId, surfaceId, (err)=>{
    if (err == null) {
        console.info('removeSurface success.');
    } else {
        console.error('removeSurface failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="o1gc7"></a>
### removeSurface
removeSurface(deviceId: string, surfaceId: string): Promise\<void>;

异步方式移除设备的预览窗口Surface ID。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| surfaceId | string | 是 | surface ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 移除预览窗口surface ID的Promise返回值。 |

**示例：**
```javascript
let wfdSink = khsharing.createWfdSink();
let deviceId = '00-FF-E8-94-94-8A'
let surfaceId = '20001'

// promise
wfdSink.removeSurface(deviceId, surfaceId).then(() => {
    console.info('removeSurface success.');
}).catch((err) => {
    console.error('removeSurface filed, error code is :' + err.code); //ErrorCode
});
```
<a name="N2Vka"></a>
### setMediaFormat
setMediaFormat(deviceId: string, videoFormat: CodecAttr, audioFormat: CodecAttr, callback: AsyncCallback\<void>): void;

异步方式设置投屏媒体协商格式。媒体协商格式是对投屏source端设备单独设置的，投屏接收器内有默认设置值，如需设置必须通过on('connectionChanged')的CONNECTED事件通知被动调用。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| videoFormat | [CodecAttr](#WfdVideoFormat) | 是 | 视频编码属性。 |
| audioFormat | [CodecAttr](#WfdVideoFormat) | 是 | 音频编码属性。 |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**错误码：**

以下错误码的详细介绍请参见[投屏错误码](#RiDvJ)

| **错误码ID** | **错误信息** |
| --- | --- |
| 18800004 | Service limit. Return by callback. |
| 18800005 | State exception. Return by callback. |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let videoFormat = {codecType: 1, format: 7}
let audioFormat = {codecType: 3, format: 43}

// asynccallback
wfdSink.setMediaFormat(deviceId, videoFormat, audioFormat, (err)=>{
    if (err == null) {
        console.info('setMediaFormat success.');
    } else {
        console.error('setMediaFormat failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="mWQjL"></a>
### setMediaFormat
setMediaFormat(deviceId: string, videoFormat: CodecAttr, audioFormat: CodecAttr): Promise\<void>;

异步方式设置投屏媒体协商格式。媒体协商格式是对投屏source端设备单独设置的，投屏接收器内有默认设置值，如需设置必须通过on('connectionChanged')的CONNECTED事件通知被动调用。通过注册回调函数方式获取返回值。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| videoFormat | [CodecAttr](#WfdVideoFormat) | 是 | 视频编码类型。 |
| audioFormat | [CodecAttr](#WfdVideoFormat) | 是 | 音频编码类型 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 设置媒体格式的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[投屏错误码](#RiDvJ)

| **错误码ID** | **错误信息** |
| --- | --- |
| 18800004 | Service limit. Return by promise. |
| 18800005 | State exception. Return by promise. |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let videoFormat = {codecType: 1, format: 7}
let audioFormat = {codecType: 3, format: 43}

// promise
wfdSink.setMediaFormat(deviceId, videoFormat, audioFormat).then(() => {
    console.info('setMediaFormat success.');
}).catch((err) => {
    console.error('setMediaFormat filed, error code is :' + err.code); //ErrorCode
});
```
<a name="roUEg"></a>
### play
play(deviceId: string, callback: AsyncCallback\<void>): void;

异步方式启动播放或从暂停状态恢复播放。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| callback | AsyncCallback\<void> | 是  | 回调函数，用于获取结果。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"

// asynccallback
wfdSink.play(deviceId, (err)=>{
    if (err == null) {
        console.info('play success.');
    } else {
        console.error('play failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="Y6aFq"></a>
### play
play(deviceId: string): Promise\<void>;

异步方式启动播放或从暂停状态恢复播放。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 启动播放的Promise返回值。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"

// promise
wfdSink.play(deviceId).then(() => {
    console.info('play success.');
}).catch((err) => {
    console.error('play filed, error code is :' + err.code); //ErrorCode
});
```
<a name="e3MwK"></a>
### pause
pause(deviceId: string, callback: AsyncCallback\<void>): void;

异步方式暂停播放。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"

// asynccallback
wfdSink.pause(deviceId, (err)=>{
    if (err == null) {
        console.info('pause success.');
    } else {
        console.error('pause failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="Pw3Nh"></a>
### pause
pause(deviceId: string): Promise\<void>;

异步方式暂停播放。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 暂停播放的Promise返回值。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"

// promise
wfdSink.pause(deviceId).then(() => {
    console.info('pause success.');
}).catch((err) => {
    console.error('pause filed, error code is :' + err.code); //ErrorCode
});
```
<a name="disconnect"></a>
### close
close(deviceId: string, callback: AsyncCallback\<void>): void;

异步方式关闭投屏。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| callback | AsyncCallback\<void> | 是  | 回调函数，用于获取结果。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"

// asynccallback
wfdSink.close(deviceId, (err)=>{
    if (err == null) {
        console.info('close success.');
    } else {
        console.error('close failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="fKYu3"></a>
### close
close(deviceId: string): Promise\<void>;

异步方式关闭投屏。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 关闭投屏流的Promise返回值。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"

// promise
wfdSink.close(deviceId).then(() => {
    console.info('close success.');
}).catch((err) => {
    console.error('close filed, error code is :' + err.code); //ErrorCode
});
```
<a name="SetSurfaceDisplayType-1"></a>
### setSceneType
setSceneType(deviceId: string, surfaceId: string, type: [SceneType](#WfdSurfaceDisplayType), callback: AsyncCallback\<void>): void;

异步方式设置投屏预览的前后景模式，可动态实现视频预览前后景毫秒级切换。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| surfaceId | string | 是 | surface ID。 |
| type | [SceneType](#WfdSurfaceDisplayType) | 是 | 前后景类型。 |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"
let surfaceId = '10006'
let type = wfdSink.SceneType.WFD_SURFACE_BACKGROUND

// asynccallback
wfdSink.setSceneType(deviceId, surfaceId, type, (err)=>{
    if (err == null) {
        console.info('setSceneType success.');
    } else {
        console.error('setSceneType failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="QJThV"></a>
### setSceneType
setSceneType((deviceId: string, surfaceId: string, type: [SceneType](#WfdSurfaceDisplayType)) : Promise\<void>;

异步方式设置投屏预览的前后景模式，可动态实现视频预览前后景毫秒级切换。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| surfaceId | string | 是 | surface ID。 |
| type | [SceneType](#WfdSurfaceDisplayType) | 是 | 前后景类型。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 设置前后景的Promise返回值。 |

**示例：**
```javascript
let deviceId = "00-FF-E8-94-94-8A"
let surfaceId = '10006'
let type = wfdSink.SceneType.WFD_SURFACE_BACKGROUND

// promise
wfdSink.setSceneType(deviceId, surfaceId, type).then(() => {
    console.info('setSceneType success.');
}).catch((err) => {
    console.error('setSceneType filed, error code is :' + err.code); //ErrorCode
});
```
<a name="xnK8w"></a>
### mute
mute(deviceId: string, callback: AsyncCallback\<void>): void;

异步方式对播放静音。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'

// asynccallback
wfdSink.mute(deviceId, (err)=>{
    if (err == null) {
        console.info('mute success.');
    } else {
        console.error('mute failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="GgI7m"></a>
### mute
mute(deviceId: string): Promise\<void>;

异步方式对播放静音。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 使用Promise的方式获取结果。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'

// promise
wfdSink.mute(deviceId).then(() => {
    console.info('mute success.');
}).catch((err) => {
    console.error('mute filed, error code is :' + err.code); //ErrorCode
});
```
<a name="ZbWte"></a>
### unMute
unmute(deviceId: string, callback: AsyncCallback\<void>): void;

异步方式对投播放解除静音。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| callback | AsyncCallback\<void> | 是 | 回调函数，用于获取结果。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'

// asynccallback
wfdSink.unMute(deviceId, (err)=>{
    if (err == null) {
        console.info('unMute success.');
    } else {
        console.error('unMute failed, error code is :' + err.code); //ErrorCode
    }
})
```
<a name="NDHfy"></a>
### unMute
unmute(deviceId: string): Promise\<void>;

异步方式对播放解除静音。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 使用Promise的方式获取结果。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'

// promise
wfdSink.unMute(deviceId).then(() => {
    console.info('unMute success.');
}).catch((err) => {
    console.error('unMute filed, error code is :' + err.code); //ErrorCode
});
```
<a name="qFmaZ"></a>
### release
release(callback: AsyncCallback\<void>): void;

异步方式释放投屏接收器。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**示例：**
```javascript
// asynccallback
wfdSink.release((err) => {
    if (err == null) {
    		console.info('release success.');
    } else {
        console.error('release failed, error code is :' + err.code); //ErrorCode
    }
});
```
<a name="w7fl6"></a>
### release
release(): Promise\<void>

异步方式释放投屏接收器。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

 **返回值：**

| **类型** | **说明** |
| --- | --- |
| Promise\<void> | 使用Promise的方式获取结果。 |

**示例：**
```javascript
// promise
wfdSink.release().then(() => {
    console.info('release success.');
}).catch((err) => {
    console.error('release filed, error code is :' + err.code); //ErrorCode
});
```
<a name="70b04b79"></a>
### on('error')
on(type: 'error', callback: ErrorCallback): void;

开始监听投屏接收器错误事件。当上报error错误事件后，用户需处理error事件，退出投屏操作。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'error'。 |
| callback | ErrorCallback | 是 | 回调函数，用于获取错误信息。 |

**示例：**
```javascript
on('error', (err)=>{
    if(err){
     		console.log('on error, error code is : ', err.code); // ErrorCode
    }
})
```
<a name="8b02a89d"></a>
### on('deviceStateChanged')
on(type: 'deviceStateChanged', callback: Callback<ConnectionInfo>): void;

监听投屏接收器设备状态事件。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'deviceStateChanged'。 |
| callback | Callback<[ConnectionInfo](#WfdConnectionInfo)> | 是 | 回调函数，用于返回连接或断开的设备链接信息。 |

**示例：**
```javascript
wfdSink.on('deviceStateChanged', (stateInfo) => {
    if (stateInfo.state == khsharing.DeviceState.CONNECTED){
    		console.log(`on deviceStateChanged, ${connInfo.macAddr} connected.`)
    } else if(stateInfo.state == khsharing.DeviceState.DISCONNECTED){
    		console.log(`on deviceStateChanged, ${connInfo.macAddr} disconnected.`)
    }
})
```
<a name="vkwBZ"></a>
### on('disableAcceleration')
on(type: 'disableAcceleration', callback: Callback<string>): void;

监听视频加速完成事件，在后景切前景时会触发加速解码过程，加速完成时上报该事件。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.WfdSink

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'disableAcceleration'。 |
| callback | Callback<string> | 是 | 回调函数，用于返回加速播放窗口的surface ID。 |

**示例：**
```javascript
wfdSink.on('disableAcceleration', (surfaceId) => {
    if (surfaceId){
    		console.log(`on disableAcceleration, surface id : ${surfaceId}.`)
    }
})
```
<a name="RiDvJ"></a>
## ErrorCode
投屏错误码枚举。

**系统能力:** SystemCapability.Multimedia.khSharingWfd.Core

| **名称** | **值** | **说明** | 错误出现原因 |
| --- | --- | --- | --- |
| ERR_OK | 0 | 无错误 |  |
| ERR_GENERAL_ERROR | 18800001 | 无类别通用错误 | 服务内部出现非业务错误，例如内存分配失败，极低概率出现 |
| ERR_BAD_PARAMETER | 18800002 | 参数不匹配 | 调用接口使用的参数错误，极低概率出现 |
| ERR_INVALID_ID | 18800003 | 无效的ID | 应用调用无效的设备id，极低概率出现 |
| ERR_SERVICE_LIMIT | 18800004 | 服务超限 | 投屏窗口数量超过限制 |
| ERR_STATE_EXCEPTION | 18800005 | 状态不匹配 | 两个设备一断一投容易出现surface的状态错误，低概率出现 |
| ERR_CONNECTION_FAILURE | 18800006 | 连接失败 | 创建tcp连接失败，可归类为网络问题 |
| ERR_INTERACTION_FAILURE | 18800007 | 交互失败 | 投屏协议交互失败，可归类为网络问题 |
| ERR_CONNECTION_TIMEOUT | 18800008 | 连接超时 | 投屏协议交互超时，可归类为网络问题 |
| ERR_CONNECTION_REFUSED | 18800009 | 连接拒绝 | 投屏拒绝，不会出现 |
| ERR_NETWORK_ERROR   | 18800011 | 网络异常 | 投屏成功，之后由于网络异常断开，可归类为网络问题 |
| ERR_INVALID_SURFACE_ID | 18800101 | 无效surface ID | media服务崩溃导致surface失效，应用已进行适配 |
| ERR_DECODE_ERROR | 18800601 | 解码错误 | 解码错误，基本不出现，不用考虑 |
| ERR_DECODE_FORMAT | 18800602 | 解码格式不支持 | 解码格式错误，基本不出现，不用考虑 |
| ERR_DECODE_SCARCE_CAPACITY | 18800603 | 解码能力不足 | cpu不足，基本不出现，不用考虑 |
| ERR_PLAY_START | 18800608 | 播放启动错误 | 播放视频错误，基本不出现不用考虑 |
| ERR_PLAY_STOP | 18800609 | 播放停止错误 | 播放视频错误，基本不出现不用考虑 |
| ERR_RECEIVING_LIMIT | 18802101 | 接收投屏数量超限 | 投屏设备数量超过限制 |

