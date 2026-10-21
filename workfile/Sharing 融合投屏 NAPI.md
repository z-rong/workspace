# @ohos.multimedia.khSharingMulticast接口
投播服务为开发者提供一套简单且易于理解的接口，使得开发者能够方便接入系统并使用系统的媒体接入、预览和转分发能力。投播服务可支撑音视频对讲、投屏、拉流播放、推流等业务。
Multicast融合投屏部件，可通过WIFI或软总线进行连接，可将设备的屏幕或窗口画面，投送到另外一台设备进行全屏或小窗口预览，支持多设备、多路并发投屏。设备规格分为两类，一类为主投端Source，另一类为被投端Sink。该模块提供以下常用功能：

- Cast：用于OS设备间投屏业务，可实现多个屏幕或窗口投屏，并提供反控功能。

**相关模块**：

- khSharingChat：用于音视频通话业务，可实现多路音视频通话功能。
- khSharingPlayer：用于流播放业务，可实现RTSP协议拉流播放功能。
- khSharingMiracast: 用于Miracast投屏业务，可实现Miracast协议投屏。

**说明**：本模块接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
文档版本：1.1
## 导入模块
```javascript
import khsharingMulticast from '@ohos.multimedia.khSharingMulticast';
```
## khsharingMulticast.createCast
createCast(): Cast

创建Multicast投屏实例。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

**返回值：**

| **类型** | **说明** |
| --- | --- |
| Cast | 返回一个Multicast投屏实例。 |

**示例：**
```javascript
// synccallback
let multicast = khsharingMulticast.createCast();
```
## CanvasType
表示视频源画布类型的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| SCREEN  | 0 | 屏幕类型视频源 |
| WINDOW | 1 | 窗口类型视频源 |

## CastState
表示投屏状态枚举值。其中，PLAYING状态一般用于Sink端表示已开始渲染播放，如果用于Source端，则代表已经开始向Sink端发送数据。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| CONNECTED | 0 | 已连接 |
| DISCONNECTED | 1 | 断开连接 |
| INIT | 2 | 初始状态 |
| READY | 3 | 就绪状态 |
| PLAYING | 4 | 播放状态 |
| PAUSED | 5 | 暂停播放状态 |
| STOPPED | 6 | 停止播放状态 |

## CodecType
表示媒体编解码类型的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| CODEC_H264	   | 1 | 视频编码H264格式 |
| CODEC_H265 | 2 | 视频编码H265格式（暂不支持） |

## VideoFormat
表示视频协商参数的枚举。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

| **名称** | **值** | **说明** |
| --- | --- | --- |
| VIDEO_640x480_30 | 1 | 视频编码格式480P，30帧 |
| VIDEO_1280x720_25 | 3 | 视频编码格式720P，25帧 |
| VIDEO_1280x720_30 | 4 | 视频编码格式720P，30帧 |
| VIDEO_1920x1080_25 | 6 | 视频编码格式1080P，25帧 |
| VIDEO_1920x1080_30 | 7 | 视频编码格式1080P，30帧 |

## CodecAttr
表示媒体协商的视频属性。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
|  codecType | number | 是 | 编码类型枚举值，参考[CodecType](#dZVPp)。 |
|  format | number | 是 | 视频参数枚举值，参考[VideoFormat](#mr91u)。 |

## Rect
表示屏幕或窗口矩形画布区域。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| left | number | 是 | 矩形区域的左边界，单位为像素。 |
| top | number | 是 | 矩形区域的上边界，单位为像素。 |
| width | number | 是 | 矩形区域的宽度，单位为像素。 |
| height | number | 是 | 矩形区域的高度，单位为像素。 |

## CastInfo
表示投屏设备连接信息。

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| canvasType | [CanvasType](#tqUaZ) | 是 | 画布类型。 |
| canvasId | string | 是 | 画布ID。 |
| deviceId | string | 是 | 对端设备ID。 |
| ipAddr | string | 否 | 对端设备IP。 |
| deviceName | string | 否 | 设备名称。 |
| codecAttrs | Array<[CodecAttr](#WfdVideoFormat)> | 否 | 视频编码属性。 |

## CastStateInfo
表示投屏设备连接状态信息。

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | 对端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| state | [CastState](#OaFL4) | 是 | 投屏状态。 |

## Cast
融合投屏管理类，用于融合投屏业务，在调用Cast的方法前，需要先通过createCast()构建一个Multicast实例。
### reject
reject(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式拒绝Source端投屏或Sink端拉起投屏，通过注册回调函数方式获取返回值。主投端在on('pull')事件中调用，被投端在on('connect')事件中调用。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | 对端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.reject(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('reject success.');
    } else {
        console.error('reject failed, error code is :' + err.code); //ErrorCode
    }
})
```
### reject
reject(deviceId: string, canvasId: string): Promise<void>;

异步方式拒绝Source端投屏或Sink端拉起投屏，通过Promise获取返回值。主投端在on('pull')事件中调用，被投端在on('connect')事件中调用。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | 对端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 拒绝投屏的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.reject(deviceId, canvasId).then(() => {
    console.info('reject success.');
}).catch((err) => {
    console.error('reject filed, error code is :' + err.code); //ErrorCode
});
```
### enableSource
enableSource(callback: AsyncCallback<void>): void;

异步方式启用Source端功能，通过注册回调函数方式获取返回值。默认开启。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
// asynccallback
multicast.enableSource((err)=>{
    if (err == null) {
        console.info('enableSource success.');
    } else {
        console.error('enableSource failed, error code is :' + err.code); //ErrorCode
    }
})
```
### enableSource
enableSource(): Promise<void>;

异步方式启用Source端功能，通过Promise获取返回值。默认开启。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 启用Source端功能的Promise返回值。 |

**示例：**
```javascript
// promise
multicast.enableSource().then(() => {
    console.info('enableSource success.');
}).catch((err) => {
    console.error('enableSource filed, error code is :' + err.code); //ErrorCode
});
```
### disableSource
disableSource(callback: AsyncCallback<void>): void;

异步方式禁用Source端功能，通过注册回调函数方式获取返回值。禁用后无法主动投屏，同时禁止被Sink端拉起投屏。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
// asynccallback
multicast.disableSource((err)=>{
    if (err == null) {
        console.info('disableSource success.');
    } else {
        console.error('disableSource failed, error code is :' + err.code); //ErrorCode
    }
})
```
### disableSource
disableSource(): Promise<void>;

异步方式禁用Source端相关功能，通过Promise获取返回值。禁用后无法主动投屏，同时禁止被Sink端拉起投屏。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 禁用Source端功能的Promise返回值。 |

**示例：**
```javascript
// promise
multicast.disableSource().then(() => {
    console.info('disableSource success.');
}).catch((err) => {
    console.error('disableSource filed, error code is :' + err.code); //ErrorCode
});
```
### addDevice
addDevice(deviceId: string, canvasId: string, canvasType: CanvasType, rect: Rect, callback: AsyncCallback<void>): void;

异步方式选择Source端设备的一个屏幕或窗口，主动投屏到一个Sink设备上，通过注册回调函数方式获取返回值。同一个屏幕或窗口，可同时投屏到多个Sink设备上，但不能重复投到一个Sink设备上。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Sink端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| canvasType | [CanvasType](#tqUaZ) | 是 | 画布类型。 |
| rect | [Rect](#WfdConnectionState) | 是 | 画布区域。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.addDevice(deviceId, canvasId, canvasType, rect, (err)=>{
    if (err == null) {
        console.info('addDevice success.');
    } else {
        console.error('addDevice failed, error code is :' + err.code); //ErrorCode
    }
})
```
### addDevice
addDevice(deviceId: string, canvasId: string, canvasType: CanvasType, rect: Rect): Promise<void>;

异步方式选择Source端设备的一个屏幕或窗口，主动投屏到一个Sink设备上，通过Promise获取返回值。同一个屏幕或窗口，可同时投屏到多个Sink设备上，但不能重复投到一个Sink设备上。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Sink端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| canvasType | [canvasType](#tqUaZ) | 是 | 画布类型。 |
| rect | [Rect](#WfdConnectionState) | 是 | 画布区域。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 添加投屏设备的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.addDevice(deviceId, canvasId, canvasType, rect).then(() => {
    console.info('addDevice success.');
}).catch((err) => {
    console.error('addDevice filed, error code is :' + err.code); //ErrorCode
});
```
### removeDevice
removeDevice(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式移除Source端设备的一路投屏，通过注册回调函数方式获取返回值

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Sink端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let surfaceId = '20001'

// asynccallback
multicast.removeDevice(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('removeDevice success.');
    } else {
        console.error('removeDevice failed, error code is :' + err.code); //ErrorCode
    }
})
```
### removeDevice
removeDevice(deviceId: string, canvasId: string): Promise<void>;

异步方式移除Source端设备的一路投屏，通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Sink端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | Source端移除投屏的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let surfaceId = '20001'

// promise
multicast.removeDevice(deviceId, canvasId).then(() => {
    console.info('removeDevice success.');
}).catch((err) => {
    console.error('removeDevice filed, error code is :' + err.code); //ErrorCode
});
```
### enableSink
enableSink(callback: AsyncCallback<void>): void;

异步方式启用Sink端功能，通过注册回调函数方式获取返回值。默认开启。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
// asynccallback
multicast.enableSink((err)=>{
    if (err == null) {
        console.info('enableSink success.');
    } else {
        console.error('enableSink failed, error code is :' + err.code); //ErrorCode
    }
})
```
### enableSink
enableSink(): Promise<void>;

异步方式启用Sink端功能，通过Promise获取返回值。默认开启。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 启用Sink端功能的Promise返回值。 |

**示例：**
```javascript
// promise
multicast.enableSink().then(() => {
    console.info('enableSink success.');
}).catch((err) => {
    console.error('enableSink filed, error code is :' + err.code); //ErrorCode
});
```
### disableSink
disableSink(callback: AsyncCallback<void>): void;

异步方式禁用Sink端相关功能，通过注册回调函数方式获取返回值。禁用后无法接受投屏，同时禁止向Source端拉起投屏。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
// asynccallback
multicast.disableSink((err)=>{
    if (err == null) {
        console.info('disableSink success.');
    } else {
        console.error('disableSink failed, error code is :' + err.code); //ErrorCode
    }
})
```
### disableSink
disableSink(): Promise<void>;

异步方式禁用Sink端相关功能，通过Promise获取返回值。禁用后无法接受投屏，同时禁止向Source端拉起投屏。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 禁用Sink端功能的Promise返回值。 |

**示例：**
```javascript
// promise
multicast.disableSink().then(() => {
    console.info('disableSink success.');
}).catch((err) => {
    console.error('disableSink filed, error code is :' + err.code); //ErrorCode
});
```
### pullCast
pullCast(deviceId: string, callback: AsyncCallback<void>): void;

异步方式从Sink端主动拉起Source端设备进行镜像投屏，通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'

// asynccallback
multicast.pullCast(deviceId, (err)=>{
    if (err == null) {
        console.info('pullCast success.');
    } else {
        console.error('pullCast failed, error code is :' + err.code); //ErrorCode
    }
})
```
### pullCast
pullCast(deviceId: string): Promise<void>;

异步方式从Sink端主动拉起Source端设备进行镜像投屏，通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 主动拉流的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'

// promise
multicast.pullCast(deviceId).then(() => {
    console.info('pullCast success.');
}).catch((err) => {
    console.error('pullCast filed, error code is :' + err.code); //ErrorCode
});
```
### bindDevice
bindDevice(deviceId: string, canvasId: string, videoAttr: CodecAttr, callback: AsyncCallback<void>): void;

异步方式接受Source端设备的投屏请求，同时选择一种视频编码格式进行绑定，通过注册回调函数方式获取返回值。Sink需要在on('connect')的事件通知后调用此接口。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| codecAttr | [CodecAttr](#WfdVideoFormat) | 是 | 设置的视频编码参数。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.bindDevice(deviceId, canvasId, videoAttr, (err)=>{
    if (err == null) {
        console.info('bindDevice success.');
    } else {
        console.error('bindDevice failed, error code is :' + err.code); //ErrorCode
    }
})
```
### bindDevice
bindDevice(deviceId: string, canvasId: string, videoAttr: CodecAttr): Promise<void>;

异步方式接受Source端设备的投屏请求，同时选择一种视频编码格式进行绑定，通过Promise获取返回值。Sink需要在on('connect')的事件通知后调用此接口。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| codecAttr | [CodecAttr](#WfdVideoFormat) | 是 | 设置的视频编码参数。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | Sink端接受投屏的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.bindDevice(deviceId, canvasId, videoAttr).then(() => {
    console.info('bindDevice success.');
}).catch((err) => {
    console.error('bindDevice filed, error code is :' + err.code); //ErrorCode
});
```
### unBindDevice
unBindDevice(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式关闭Sink端投屏，通过注册回调函数方式获取返回值。Sink主动关闭时调用，或者在对端关闭时触发Sink端的on('close')的事件后，调用此接口。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.unBindDevice(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('unBindDevice success.');
    } else {
        console.error('unBindDevice failed, error code is :' + err.code); //ErrorCode
    }
})
```
### unBindDevice
unBindDevice(deviceId: string, canvasId: string): Promise<void>;

异步方式关闭Sink端投屏，通过Promise获取返回值。Sink主动关闭时调用，或者在对端关闭时触发Sink端的on('close')的事件后，调用此接口。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | Sink端删除投屏的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.unBindDevice(deviceId, canvasId).then(() => {
    console.info('unBindDevice success.');
}).catch((err) => {
    console.error('unBindDevice filed, error code is :' + err.code); //ErrorCode
});
```
### appendWindow
appendWindow(deviceId: string, canvasId: string, surfaceId: string, rect: Rect, callback: AsyncCallback<void>): void;

异步方式为设备添加一个XComponent播放组件的surface ID，同一个设备可以添加多个XComponent播放组件的surface ID。通过注册回调函数方式获取返回值。首次添加surface ID，需要在bindDevice之后进行调用。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| surfaceId | string | 是 | XComponent播放组件的surface ID。 |
| rect | [Rect](#WfdConnectionState) | 是 | XComponent播放组件的画布区域。 |
| windowId | string | 是 | XComponent所属窗口ID |
| posX | number | 是 | 窗口x轴坐标 |
| posY | number | 是 | 窗口y轴坐标 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'
let surfaceId = '10000'

// asynccallback
multicast.appendWindow(deviceId, canvasId, surfaceId, rect, windowId, posX, posY, (err)=>{
    if (err == null) {
        console.info('appendWindow success.');
    } else {
        console.error('appendWindow failed, error code is :' + err.code); //ErrorCode
    }
})
```
### appendWindow
appendWindow(deviceId: string, canvasId: string, surfaceId: string, rect: Rect): Promise<void>;

异步方式为设备添加一个XComponent播放组件的surface ID，同一个设备可以添加多个XComponent播放组件的surface ID。通过Promise获取返回值。首次添加surface ID，需要在bindDevice之后进行调用。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| surfaceId | string | 是 | XComponent播放组件的surface ID。 |
| rect | [Rect](#WfdConnectionState) | 是 | XComponent播放组件的画布区域。 |
| windowId | number | 是 | XComponent所属窗口ID |
| posX | number | 是 | 窗口x轴坐标 |
| posY | number | 是 | 窗口y轴坐标 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 添加surface ID的Promise返回值。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'
let surfaceId = '10000'

// promise
multicast.appendWindow(deviceId, canvasId, surfaceId, rect, windowId, posX, posY).then(() => {
    console.info('appendWindow success.');
}).catch((err) => {
    console.error('appendWindow filed, error code is :' + err.code); //ErrorCode
});
```
### updateWindowRect
updateWindowRect(deviceId: string, canvasId: string, surfaceId: string, rect: Rect, windowId: number, posX: number, posY: number, callback: AsyncCallback<void>): void;

异步方式更新一个XComponent播放组件的画布区域，用于sink端反控更新主控端窗口区域。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| surfaceId | string | 是 | XComponent播放组件的surface ID。 |
| rect | [Rect](#WfdConnectionState) | 是 | XComponent播放组件的画布区域。 |
| windowId | number | 是 | XComponent所属窗口ID |
| posX | number | 是 | 窗口x轴坐标 |
| posY | number | 是 | 窗口y轴坐标 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let surfaceId = '10000'

// asynccallback
multicast.updateWindowRect(surfaceId, rect, windowId, posX, posY, (err)=>{
    if (err == null) {
        console.info('updateWindowRect success.');
    } else {
        console.error('updateWindowRect failed, error code is :' + err.code); //ErrorCode
    }
})
```
### updateWindowRect
updateWindowRect(deviceId: string, canvasId: string, surfaceId: string, rect: Rect, windowId: number, posX: number, posY: number): Promise<void>;

异步方式更新一个XComponent播放组件的画布区域，用于sink端反控更新主控端窗口区域。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | Source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| surfaceId | string | 是 | XComponent播放组件的surface ID。 |
| rect | [Rect](#WfdConnectionState) | 是 | XComponent播放组件的画布区域。 |
| windowId | number | 是 | XComponent所属窗口ID |
| posX | number | 是 | 窗口x轴坐标 |
| posY | number | 是 | 窗口y轴坐标 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 添加surface ID的Promise返回值。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'
let surfaceId = '10000'

// promise
multicast.updateWindowRect(deviceId, canvasId, surfaceId, rect, windowId, posX, posY).then(() => {
    console.info('updateWindowRect success.');
}).catch((err) => {
    console.error('updateWindowRect filed, error code is :' + err.code); //ErrorCode
});
```
### play
play(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式启动播放。通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.play(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('play success.');
    } else {
        console.error('play failed, error code is :' + err.code); //ErrorCode
    }
})
```
### play
play(deviceId: string, canvasId: string): Promise<void>;

异步方式启动播放。通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 启动播放的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.play(deviceId, canvasId).then(() => {
    console.info('play success.');
}).catch((err) => {
    console.error('play filed, error code is :' + err.code); //ErrorCode
});
```
### close
close(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式关闭Sink端投屏播放，通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.close(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('close success.');
    } else {
        console.error('close failed, error code is :' + err.code); //ErrorCode
    }
})
```
### close
close(deviceId: string, canvasId: string): Promise<void>;

异步方式关闭Sink端投屏播放，通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 停止播放的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.close(deviceId, canvasId).then(() => {
    console.info('close success.');
}).catch((err) => {
    console.error('close filed, error code is :' + err.code); //ErrorCode
});
```
### startInputBack
startInputBack(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式开启Sink端一路投屏反控，通过注册回调函数方式获取返回值。为了反控精准，最好先调用updateWindowRect，更新下主控端窗口区域信息。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.startInputBack(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('startInputBack success.');
    } else {
        console.error('startInputBack failed, error code is :' + err.code); //ErrorCode
    }
})
```
### startInputBack
startInputBack(deviceId: string, canvasId: string): Promise<void>;

异步方式开启Sink端一路投屏反控，通过Promise获取返回值。为了反控精准，最好先调用updateWindowRect，更新下主控端窗口区域信息。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 开启反控的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.startInputBack(deviceId, canvasId).then(() => {
    console.info('startInputBack success.');
}).catch((err) => {
    console.error('startInputBack filed, error code is :' + err.code); //ErrorCode
});
```
### stopInputBack
stopInputBack(deviceId: string, canvasId: string, callback: AsyncCallback<void>): void;

异步方式关闭Sink端一路投屏反控，通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// asynccallback
multicast.stopInputBack(deviceId, canvasId, (err)=>{
    if (err == null) {
        console.info('stopInputBack success.');
    } else {
        console.error('stopInputBack failed, error code is :' + err.code); //ErrorCode
    }
})
```
### stopInputBack
stopInputBack(deviceId: string, canvasId: string): Promise<void>;

异步方式关闭Sink端一路投屏反控，通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | source端设备ID。 |
| canvasId | string | 是 | 画布ID。 |

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 关闭反控的Promise返回值。 |

**示例：**
```javascript
let deviceId = '00-FF-E8-94-94-8A'
let canvasId = '20001'

// promise
multicast.stopInputBack(deviceId, canvasId).then(() => {
    console.info('stopInputBack success.');
}).catch((err) => {
    console.error('stopInputBack filed, error code is :' + err.code); //ErrorCode
});
```
### startDiscover
startDiscover(callback: AsyncCallback<void>): void;

异步方式开启设备发现，通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
// asynccallback
multicast.startDiscover((err)=>{
    if (err == null) {
        console.info('startDiscover success.');
    } else {
        console.error('startDiscover failed, error code is :' + err.code); //ErrorCode
    }
})
```
### startDiscover
startDiscover(): Promise<void>;

异步方式开启设备发现，通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 启用Source端功能的Promise返回值。 |

**示例：**
```javascript
// promise
multicast.startDiscover().then(() => {
    console.info('startDiscover success.');
}).catch((err) => {
    console.error('startDiscover filed, error code is :' + err.code); //ErrorCode
});
```
### stopDiscover
stopDiscover(callback: AsyncCallback<void>): void;

异步方式停止设备发现，通过注册回调函数方式获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<void> | 是 | 回调函数，用于获取结果。 |


**示例：**
```javascript
// asynccallback
multicast.stopDiscover((err)=>{
    if (err == null) {
        console.info('stopDiscover success.');
    } else {
        console.error('stopDiscover failed, error code is :' + err.code); //ErrorCode
    }
})
```
### stopDiscover
stopDiscover(): Promise<void>;

异步方式停止设备发现，通过Promise获取返回值。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**返回值**：

| **类型** | **说明** |
| --- | --- |
| Promise<void> | 启用Source端功能的Promise返回值。 |

**示例：**
```javascript
// promise
multicast.stopDiscover().then(() => {
    console.info('stopDiscover success.');
}).catch((err) => {
    console.error('stopDiscover filed, error code is :' + err.code); //ErrorCode
});
```
### on('discover')
on(type: 'discover', callback: AsyncCallback<Array<string>>): void;

监听设备发现事件。设备发现后全量上报。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'discover'。 |
| callback | AsyncCallback<Array<string>> | 是 | 回调函数，用于获取发现的设备Id列表。 |

**示例：**
```javascript
on('discover', (devs)=>{
    if(devs){
      devs.forEach((item: string, index) => {
      	console.log("device id: ",item);
    	})
    }
})
```
### on('stateChange')
on(type: 'stateChange', callback: AsyncCallback<CastStateInfo>): void;

监听设备状态变更。设备状态变更后上报。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'stateChange'。 |
| callback | AsyncCallback<[CastStateInfo](#WRZuC)> | 是 | 回调函数，用于获取上报的状态信息。 |

**示例：**
```javascript
on('stateChange', (state)=>{
    if(state){
      console.log("state: ",state.state)
    }
})
```

### on('pull'|'connect'|'accept'|'close'|'reject')
on(type: 'pull' | 'connect' | 'accept' | 'close' | 'reject', callback: AsyncCallback<CastInfo>): void;

监听投屏设备状态事件。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 事件回调类型，支持的事件为：'pull'，'connect'，'accept'，'close'，'reject' 。 |
| callback | callback: AsyncCallback<CastInfo> | 是 | 回调函数，用于返回对端上报来的投屏信息。 |

**示例：**
```javascript
multicast.on('pull', (castInfo) => {
    console.log('cast pull:', castInfo.deviceId);
});
multicast.on('connect', (castInfo) => {
    console.log('cast connect:', castInfo.deviceId);
});
multicast.on('accept', (castInfo) => {
    console.log('cast accept:', castInfo.deviceId);
});
multicast.on('close', (castInfo) => {
    console.log('cast close:', castInfo.deviceId);
});
multicast.on('reject', (castInfo) => {
    console.log('cast reject:', castInfo.deviceId);
});
```
### on('error')
on(type: 'error', callback: ErrorCallback<ErrorInfo>): void;

监听投屏错误事件。当上报error错误事件后，用户需处理error事件，退出投屏操作。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Cast

**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 监听事件，固定为'error'。 |
| callback | ErrorCallback<ErrorInfo> | 是 | 回调函数，用于获取错误信息。 |

**示例：**
```javascript
on('error', (err)=>{
    if(err){
     		console.log('on error, error code is : ', err.code); // ErrorCode
    }
})
```
## ErrorInfo
表示投屏错误信息。

| **成员** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| code | [ErrorCode](#RiDvJ) | 是 | 错误码。 |
| deviceId | string | 是 | 对端设备ID。 |
| canvasId | string | 是 | 画布ID。 |
| message | string | 否 | 错误信息。 |

## ErrorCode
投屏错误码枚举。

**系统能力:** SystemCapability.Multimedia.khSharingMulticast.Core

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
| ERR_NETWORK_ERROR   | 18800011 | 网络异常 |
| ERR_REMOTE_DIED   | 18800014 | 框架服务死亡 |
| ERR_TCP_KEEPALIVE_TIMEOUT | 18800015 | TCP心跳超时 |
| ERR_UDP_KEEPALIVE_TIMEOUT | 18800016 | UDP心跳超时 |
| ERR_PROTOCOL_INTERACTION_TIMEOUT | 18800017 | 协议交互超时 |
| ERR_INVALID_SURFACE_ID | 18800101 | 无效surface ID |
| ERR_INTAKE_TIMEOUT | 18800305 | 获取数据超时 |
| ERR_DECODE_ERROR | 18800601 | 解码错误 |
| ERR_DECODE_FORMAT | 18800602 | 解码格式不支持 |
| ERR_DECODE_SCARCE_CAPACITY | 18800603 | 解码能力不足 |
| ERR_PLAY_START | 18800608 | 播放启动错误 |
| ERR_PLAY_STOP | 18800609 | 播放停止错误 |
| ERR_RECEIVING_LIMIT | 18802101 | 接收投屏数量超限 |

