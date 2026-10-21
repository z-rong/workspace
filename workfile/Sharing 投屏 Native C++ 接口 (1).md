## 概述
Miracast镜像投屏模块，支持多台Wi-Fi直连（Wi-Fi Direct)的设备，不需要无线热点AP，直接使用Wi-Fi直连(即wifip2p技术)进行连接。连接成功之后将其中一台(或者多台)设备的画面和声音，直接分享到另外一台设备进行展示。设备规格分为两类，一类为源端Source，另一类为接收端Sink。
@syscap SystemCapability.Multimedia.SharingWfd
**起始版本：**9  **Version:** 1.0
## 汇总
### 文件
| **名称** | **描述** |
| --- | --- |
| native_wfd.h | 声明用于Miracast投屏的C++API接口。 |

### 类型定义
| **名称** | **描述** |
| --- | --- |
| [CastCodecAttr](#IgpQ3) | 媒体编码格式。 |
| [CastDeviceInfo](#AdpAW)  | 设备信息。 |
| [CastErrorInfo ](#vfy2J) | 上报的错误信息。 |

### 类定义
| **名称** | **描述** |
| --- | --- |
| [IWfdListener ](#dVErq)  | 监听回调接口。 |

### 枚举
| **名称** | **描述** |
| --- | --- |
| [CodecType](#xaZqk) | 媒体编码格式。 |
| [AudioFormat](#Rm1fk)  | 音频样本模式。 |
| [VideoFormat](#OgRkh) | 视频分辨率帧率模式。 |
| [CastDeviceState](#BMBdl) | 投屏设备状态。 |
| [SharingErrorCode](#yxzB8) | 框架错误码。 |
| [WfdErrorCode](#EA7Q5)  | 投屏错误码。 |

### 函数
| **名称** | **端侧** | **描述** |
| --- | --- | --- |
| [Start](#P53Xp)() | Sink | 启用投屏接收器服务。 |
| [Stop](#RRfVI)() | Sink | 停止投屏接收器服务。 |
| [AppendSurface](#UcEWI)(string deviceId, uint64_t surfaceId) | Sink | 为投屏设备添加一个XComponent播放组件的surface ID。 |
| [SetMediaFormat](#IWIIW)(string deviceId, [CastCodecAttr](#IgpQ3) videoAttr, [CastCodecAttr](#IgpQ3) audioAttr) | Sink | 设置媒体协商格式。 |
| [Play](#bn3yb) (string deviceId) | Sink | 启动播放或从暂停状态恢复播放。 |
| [Pause](#GaBho) (string deviceId) | Sink | 暂停播放。 |
| [Close ](#O7cCI)(string deviceId) | Sink | 断开设备投屏连接。 |
| [StartDiscover](#pVtro)() | Source | 开始设备发现。 |
| [StopDiscover](#BYPJB)() | Source | 停止设备发现。 |
| [AddDevice](#VjT8f)(const [CastDeviceInfo](#AdpAW) &deviceInfo) | Source | 连接设备投屏。 |
| [RemoveDevice](#SkQOg)(string deviceId) | Source | 断开设备投屏。 |
| [SetListener](#s040N)(const std::shared_ptr<[IWfdListener](#dVErq)> &listener) | 双端 | 设置回调监听者。 |
| [OnDeviceFound](#CJOkJ)(const std::vector<[CastDeviceInfo](#AdpAW)> &deviceInfos) | 双端 | 设备发现事件上报。 |
| [OnDeviceState](#ZADtW)(const [CastDeviceInfo](#AdpAW) &deviceInfo) | 双端 | 设备状态事件上报。 |
| [OnError](#gtkmZ)(const [CastErrorInfo](#vfy2J) &errorInfo) | 双端 | 错误事件上报。 |

## 类型定义说明
### CodecAttr
**描述:** 媒体编码属性。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| codecType | [CodecType](#xaZqk) | 媒体编码格式。 |
| format | int32_t | 音视频频协商参数的枚举：[VideoFormat](#OgRkh) 和 [AudioFormat](#Rm1fk)。 |

### CastDeviceInfo
**描述:** 投屏设备信息，可以是sink或source设备。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备编码 ID。 |
| ipAddr | string  | 设备ip地址。 |
| deviceName | string | 设备名称。 |
| primaryDeviceType | string | 主设备类型。 |
| secondaryDeviceType | string | 从设备类型。 |
| state | [CastDeviceState](#BMBdl) | 设备状态枚举值。 |

### CastErrorInfo
**描述:** 投屏错误信息。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备编码 ID。 |
| errorCode | int32_t | 错误码，参考[SharingErrorCode](#yxzB8)和[WfdErrorCode](#EA7Q5)。 |

## 类定义说明
### IWfdListener  
```cpp
class IWfdListener {
public:
    virtual ~IWfdListener() = default;

	virtual void OnError(const CastErrorInfo &errorInfo) = 0;	
	virtual void OnDeviceFound(const std::vector<CastDeviceInfo> &deviceInfos) = 0;
	virtual void OnDeviceStateChange(const CastDeviceInfo &deviceInfo) = 0;
};
```
**描述:** 监听接口类。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **描述** |
| --- | --- |
| [OnError](#gtkmZ) | 错误事件上报。 |
| [OnDeviceFound](#CJOkJ) | 设备发现事件上报。 |
| [OnDeviceStateChange](#ZADtW) | 设备状态变更事件上报。 |

## 枚举类型说明
### CodecType
**描述:** 媒体编码模式。
@syscap SystemCapability.Multimedia.SharingWfd

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| CODEC_NONE | -1 | 不启用 |
| CODEC_H264 | 1 | H264编码 |
| CODEC_H265 | 2 | H265编码 |
| CODEC_AAC | 3 | AAC编码 |

### VideoFormat
**描述:** 视频分辨率帧率模式。
@syscap SystemCapability.Multimedia.SharingWfd

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| VIDEO_480P_60 | 2 | 640x480 60fps |
| VIDEO_720P_25 | 3 | 1280x720 25fps |
| VIDEO_720P_30 | 4 | 1280x720 30fps |
| VIDEO_1080P_25 | 6 | 1920x1080 25fps |
| VIDEO_1080P_30 | 7 | 1920x1080 30fps |

### AudioFormat
**描述:** 音频样本模式。
@syscap SystemCapability.Multimedia.SharingWfd

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| AUDIO_44100_8_1 | 30 | 采样率44100 ，8位，单通道 |
| AUDIO_44100_8_2 | 31 | 采样率44100 ，8位，双通道 |
| AUDIO_44100_16_1 | 32 | 采样率44100 ，16位，单通道 |
| AUDIO_44100_16_2 | 33 | 采样率44100 ，16位，双通道 |
| AUDIO_48000_8_1 | 40 | 采样率48000 ，8位，单通道 |
| AUDIO_48000_8_2 | 41 | 采样率48000 ，8位，双通道 |
| AUDIO_48000_16_1 | 42 | 采样率48000 ，16位，单通道 |
| AUDIO_48000_16_2 | 43 | 采样率48000 ，16位，双通道 |

### CastDeviceState
**描述:** 投屏设备状态。
@syscap SystemCapability.Multimedia.SharingWfd

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| CONNECTED | 0 | 已连接 |
| DISCONNECTED | 1 | 断开连接 |
| INIT | 2 | 初始状态 |
| READY | 3 | 就绪状态 |
| PLAYING | 4 | 播放状态 |
| PAUSED | 5 | 暂停播放状态 |
| STOPED | 6 | 停止状态 |

### SharingErrorCode
**描述:** 投播框架错误码。
@syscap SystemCapability.Multimedia.SharingWfd

| **枚举** | **值** | **描述** |
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
| ERR_INVALID_SURFACE_ID | 18800101 | 无效surface ID |
| ERR_DECODE_ERROR | 18800601 | 解码错误 |
| ERR_DECODE_FORMAT | 18800602 | 解码格式不支持 |
| ERR_DECODE_SCARCE_CAPACITY | 18800603 | 解码能力不足 |
| ERR_PLAY_START | 18800608 | 播放启动错误 |
| ERR_PLAY_STOP | 18800609 | 播放停止错误 |

### WfdErrorCode
**描述:** 投屏错误码。
@syscap SystemCapability.Multimedia.SharingWfd

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| ERR_RECEIVING_LIMIT | 18802101 | 接收投屏数量超限 |

## 函数说明
### Start
```cpp
int32_t Start()
```
**描述:** 启用投屏接收器服务。
@syscap SystemCapability.Multimedia.SharingWfd.Sink
**参数:**
无
**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### Stop
```cpp
int32_t Stop()
```
**描述:** 停止投屏接收器服务。
@syscap SystemCapability.Multimedia.SharingWfd.Sink
**参数:**
无
**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### AppendSurface
```cpp
int32_t AppendSurface(string deviceId, uint64_t surfaceId)
```
**描述:** 为投屏设备添加一个XComponent播放组件的surface ID。
@syscap SystemCapability.Multimedia.SharingWfd.Sink
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备 ID。 |
| surfaceId | uint64_t | XComponent播放组件的surface ID。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### SetMediaFormat
```cpp
int32_t SetMediaFormat(string deviceId, CastCodecAttr videoAttr, CastCodecAttr audioAttr)
```
**描述:** 设置对应设备的投屏媒体协商格式。
@syscap SystemCapability.Multimedia.SharingWfd.Sink
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备 ID。 |
| videoAttr | [CastCodecAttr](#IgpQ3) | 视频编码格式。 |
| audioAttr | [CastCodecAttr](#IgpQ3) | 音频编码格式。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### Play
```cpp
int32_t Play(string deviceId)
```
**描述:** 启动播放或从暂停状态恢复播放。
@syscap SystemCapability.Multimedia.SharingWfd.Sink
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备 ID。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### Pause
```cpp
int32_t Pause(string deviceId)
```
**描述:** 暂停播放。
@syscap SystemCapability.Multimedia.SharingWfd.Sink
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备 ID。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### Close
```cpp
int32_t Close(string deviceId)
```
**描述:** 关闭播放并断开设备连接。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备 ID。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### StartDiscover
```cpp
int32_t StartDiscover()
```
**描述:** 开始设备发现，需持续一段时间扫描附近设备。
@syscap SystemCapability.Multimedia.SharingWfd.Source
**参数:**
无
**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### StopDiscover
```cpp
int32_t StopDiscover()
```
**描述:** 停止设备发现。
@syscap SystemCapability.Multimedia.SharingWfd.Source
**参数:**
无
**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### AddDevice
```cpp
int32_t AddDevice(const CastDeviceInfo &deviceInfo)
```
**描述:** 连接一个设备投屏。
@syscap SystemCapability.Multimedia.SharingWfd.Source
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceInfo | [CastDeviceInfo](#AdpAW) | 发现的设备信息 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### RemoveDevice
```cpp
int32_t RemoveDevice(string deviceId)
```
**描述:** 断开一个设备投屏。
@syscap SystemCapability.Multimedia.SharingWfd.Source
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备 ID。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

### SetListener
```cpp
void SetListener(const std::shared_ptr<IWfdListener> &listener)
```
**描述:** 设置监听者，监听者需实现OnDeviceFound、OnDeviceStateChange、OnDeviceStateChange。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| listener | [IWfdListener](#dVErq) | 事件监听者。 |

**返回:**
无。
### OnDeviceFound
```cpp
void OnDeviceFound(const std::vector<CastDeviceInfo> &deviceInfos)
```
**描述:** 设备发现回调事件，批量上报设备信息。
@syscap SystemCapability.Multimedia.SharingWfd.Source
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceInfos | [CastDeviceInfo](#AdpAW) | 设备信息。 |

**返回:**
无。
### OnDeviceState
```cpp
void OnDeviceState(const CastDeviceInfo &deviceInfo)
```
**描述:** 设备状态事件，在设备状态变更时上报。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceInfo | [CastDeviceInfo](#AdpAW) | 设备信息。 |

**返回:**
无。
### OnError
```cpp
void OnError(const CastErrorInfo &errorInfo)
```
**描述:** 错误状态事件。
@syscap SystemCapability.Multimedia.SharingWfd
**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| errorInfo | [CastErrorInfo](#vfy2J) | 上报的错误信息。 |

**返回:**
无。
