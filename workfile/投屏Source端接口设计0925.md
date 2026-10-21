<a name="ZOo3J"></a>
## 概述
Miracast镜像投屏模块，支持多台Wi-Fi直连（Wi-Fi Direct)的设备，不需要无线热点AP，直接使用Wi-Fi直连(即wifip2p技术)进行连接。连接成功之后将其中一台(或者多台)设备的画面和声音，直接分享到另外一台设备进行展示。设备规格分为两类，一类为传送端Source，另一类为接收端Sink。该模块提供以下常用功能：

- [WfdSink](https://www.yuque.com/shifeilong-iyxip/kshg9i/ecelpk1u37edgxrx#WfdSink)：被投端接收器用于Sink端业务，可同时接收多路Miracast投屏流。
- [WfdS](https://www.yuque.com/shifeilong-iyxip/kshg9i/ecelpk1u37edgxrx#WfdSink)ource：主投端发送器用于Source端业务，可同时发送多路Miracast投屏流。

@syscap SystemCapability.Multimedia.SharingWfd<br />**起始版本：**9  **Version:** 1.0
<a name="m8kdI"></a>
## 汇总
<a name="jDhS4"></a>
### 文件
| **名称** | **描述** |
| --- | --- |
| wfd_source.h | 声明用于Miracast投屏的source端Native接口。 |

<a name="Pd1YT"></a>
### 类型定义
| **名称** | **描述** |
| --- | --- |
| [CodecAttr](#IgpQ3) | 媒体编码格式。 |
| [CastDeviceInfo](#AdpAW)  | 设备信息，可以是sink或source设备。 |
| [CastErrorInfo ](#vfy2J) | 上报的错误信息。 |

<a name="jh1nt"></a>
### 类定义
| **名称** | **描述** |
| --- | --- |
| IWfdListener   | 监听回调接口类。 |

<a name="hMgef"></a>
### 枚举
| **名称** | **描述** |
| --- | --- |
| [CodecType](#xaZqk) | 媒体编码格式。 |
| [AudioFormat](#Rm1fk)  | 音频样本模式。 |
| [VideoFormat](#OgRkh) | 视频分辨率帧率模式。 |
| [CastDeviceState](#BMBdl) | 投屏设备状态。 |
| [SharingErrorCode](#yxzB8) | 框架错误码。 |
| [WfdErrorCode](#EA7Q5)  | 投屏错误码。 |

<a name="IRDkX"></a>
### 函数
| **名称** | **模块** | **描述** |
| --- | --- | --- |
| [StartDiscover](#pVtro)() | Native | 开始设备发现（持续一段时间内扫描附件设备）。 |
| [StopDiscover](#BYPJB)() | Native | 停止设备发现。 |
| [AddDevice](#VjT8f)(uint64_t screenId, [CastDeviceInfo](#AdpAW) &deviceInfo) | Native | 连接某个设备并开始投屏。 |
| [RemoveDevice](#SkQOg)(string deviceId) | Native | 断开某个设备。 |
| [SetListener](#s040N)(const std::shared_ptr<IWfdSourceListener> &listener) | Native | 设置回调监听者。 |
| [OnInfo](#CJOkJ)(std::shared_ptr<BaseMsg> &info) | Native | Native通用事件回调。 |

<a name="QFjkR"></a>
## 类型定义说明
<a name="IgpQ3"></a>
### CodecAttr
**描述:** 媒体编码格式。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| codecType | [CodecType](#xaZqk) | 媒体编码格式。 |
| format | int32_t | 音视频频协商参数的枚举：[VideoFormat](#OgRkh) 或 [AudioFormat](#Rm1fk)。 |

<a name="AdpAW"></a>
### CastDeviceInfo
**描述:** 投屏设备信息，可以是sink或source设备。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备编码Id。 |
| ipAddr | string  | 设备ip地址。 |
| ctrlPort | int32_t | WFD 通讯端口。 |
| deviceName | string | 设备名称 |
| primaryDeviceType | string | 主设备类型。 |
| secondaryDeviceType | string | 从设备类型。 |
| state | [CastDeviceState](#BMBdl) | 设备状态枚举值。 |

<a name="vfy2J"></a>
### CastErrorInfo
**描述:** 投屏错误上报信息。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备编码Id。 |
| errorCode | int32_t | 错误码，参考[SharingErrorCode](#yxzB8)和[WfdErrorCode](#EA7Q5)。 |

<a name="wn9LZ"></a>
## 类定义说明
<a name="dVErq"></a>
### IWfdSourceListener  
```cpp
class IWfdSourceListener {
public:
    virtual ~IWfdSourceListener() = default;

    virtual void OnInfo(std::shared_ptr<BaseMsg> &info) = 0;
};
```
**描述:** 监听回调接口类。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core<br />**参数:**

| **名称** | **描述** |
| --- | --- |
| [onError](https://www.yuque.com/shifeilong-iyxip/kshg9i/bv8x87488wx42uyg#gtkmZ) | 监听投屏服务运行错误。 |
| [OnDeviceFound](https://www.yuque.com/shifeilong-iyxip/kshg9i/bv8x87488wx42uyg#CJOkJ) | 监听source端设备发现后上报。 |
| [OnDeviceStateChange](https://www.yuque.com/shifeilong-iyxip/kshg9i/bv8x87488wx42uyg#ZADtW) | 监听设备运行状态变更上报。 |

<a name="aprRi"></a>
## 枚举类型说明
<a name="xaZqk"></a>
### CodecType
**描述:** 媒体编码模式。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| CODEC_NONE | -1 | 不启用 |
| CODEC_DEFAULT |  0 | 源编码类型 |
| CODEC_H264 | 1 | H264编码 |
| CODEC_H265 | 2 | H265编码 |
| CODEC_AAC | 3 | AAC编码 |

<a name="OgRkh"></a>
### VideoFormat
**描述:** 视频分辨率帧率模式。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| VIDEO_480P_60 | 2 | 640x480 60fps |
| VIDEO_720P_25 | 3 | 1280x720 25fps |
| VIDEO_720P_30 | 4 | 1280x720 30fps |
| VIDEO_1080P_25 | 6 | 1920x1080 25fps |
| VIDEO_1080P_30 | 7 | 1920x1080 30fps |

<a name="Rm1fk"></a>
### AudioFormat
**描述:** 音频样本模式。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core

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

<a name="BMBdl"></a>
### CastDeviceState
**描述:** 投屏设备状态。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| CONNECTED | 0 | 已连接 |
| DISCONNECTED | 1 | 断开连接 |
| INIT | 2 | 初始状态 |
| READY | 3 | 就绪状态 |
| PLAYING | 4 | 播放状态 |
| PAUSEED | 5 | 暂停播放状态 |
| STOPED | 6 | 停止播放状态 |

<a name="yxzB8"></a>
### SharingErrorCode
**描述:** 投播框架错误码。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core

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

<a name="EA7Q5"></a>
### WfdErrorCode
**描述:** 投屏错误码。<br />@syscap SystemCapability.Multimedia.SharingWfd.Core

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| ERR_RECEIVING_LIMIT | 18802101 | 接收投屏数量超限 |

<a name="b530d"></a>
## Native模块函数说明
<a name="pVtro"></a>
### StartDiscover
```cpp
int32_t StartDiscover()
```
**描述:** <br />开始设备发现（持续一段时间内扫描附件设备）。<br />@syscap SystemCapability.Multimedia.SharingWfd.Source<br />**参数:**<br />无<br />**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

<a name="BYPJB"></a>
### StopDiscover
```cpp
int32_t StopDiscover()
```
**描述:** <br />停止设备发现。<br />@syscap SystemCapability.Multimedia.SharingWfd.Source<br />**参数:**<br />无<br />**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

<a name="VjT8f"></a>
### AddDevice
```cpp
int32_t AddDevice(uint64_t screenId,CastDeviceInfo &deviceInfo)
```
**描述:** <br />从发现的设备列表中，选择一个设备进行连接并开始投屏。<br />@syscap SystemCapability.Multimedia.SharingWfd.Source<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| screenId | uint64_t | 选择的屏幕id，为0时选择默认屏幕。 |
| deviceInfo | [CastDeviceInfo](#AdpAW) | 发现的设备信息 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

<a name="SkQOg"></a>
### RemoveDevice
```cpp
int32_t RemoveDevice(string deviceId)
```
**描述:** <br />断开某个sink设备。<br />@syscap SystemCapability.Multimedia.SharingWfd.Source<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | string | 设备Id。 |

**返回:**

| **类型** | **描述** |
| --- | --- |
| int32_t | 成功时返回0，失败时返回-1。 |

<a name="s040N"></a>
### SetListener
```cpp
void SetListener(const std::shared_ptr<IWfdSourceListener> &listener)
```
**描述:** <br />设置回调监听者，监听者需实现OnDeviceDiscover、OnDeviceStateChange、OnDeviceStateChange。<br />@syscap SystemCapability.Multimedia.SharingWfd<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| listener | IWfdSourceListener | 设置监听者。 |

**返回:**<br />无
<a name="CJOkJ"></a>
### OnInfo
```cpp
void OnInfo(std::shared_ptr<BaseMsg> &info)
{
    switch (info->GetMsgId()) {
        case WfdErrorMsg::MSG_ID: {
            auto data = std::static_pointer_cast<WfdErrorMsg>(info);
            OnError("inner error", data->errorCode, data->message);
            break;
        }
        default:
            SHARING_LOGI("none process case.");
            break;
    }
}
```
**描述:** <br />设备发现回调（发现一个上报一个）。<br />@syscap SystemCapability.Multimedia.SharingWfd<br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| info | BaseMsg | Native上报给监听者的通用消息，通过到消息号转换为对应的消息结构体。 |

**返回:**<br />无
<a name="LG1mC"></a>
## Scene模块函数说明
<a name="cHYOy"></a>
### StartDiscover
int32_t StartDiscover();<br />开始设备发现（持续一段时间内扫描附件设备）。
<a name="QG87D"></a>
### StopDiscover
int32_t StopDiscover( );<br />停止设备发现。
<a name="aQX7Q"></a>
### ConnectDevice
 int32_t ConnectDevice（ConnectDeviceReq &msg）;<br />选中上报的设备，进行p2p连接，scene只是转发这个连接请求，不做数据处理，然后在p2p设备连接上来的回调中，来处理设备信息的存储和上报。<br />**请求参数结构体：**ConnectDeviceReq

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| deviceId | string | 是 | 设备 ID。 |

<a name="Diw8R"></a>
### CreateScreenCapture
int32_t CreateScreenCapture(CreateScreenCaptureReq &msg, CreateScreenCaptureRsp &reply);<br />同步接口，创建一个屏幕采集视频源。<br />**请求参数结构体：**CreateScreenCaptureReq
```cpp
struct CreateScreenCaptureReq : public BaseMsg {
    enum { MSG_ID = WfdMsgId::WFD_CREATE_SCREEN_CAPTURE_REQ };

    int32_t GetMsgId() final
    {
        return MSG_ID;
    }

    CreateScreenCaptureReq &operator=(const GetSinkConfigRsp &rReq)
    {
        if (this != &rReq) {
            this->screenId = rReq.screenId;
        }
        return *this;
    }

    IPC_BIND_ATTR(screenId)

    uint64_t screenId;
};
```
| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| screenId | uint64_t  | 是 | 屏幕 ID（0则采集默认） |

**返回参数结构体：**CreateScreenCaptureRsp
```cpp
struct CreateScreenCaptureRsp : public BaseMsg {
    enum { MSG_ID = WfdMsgId::WFD_CREATE_SCREEN_CAPTURE_RSP };

    int32_t GetMsgId() final
    {
        return MSG_ID;
    }

    CreateScreenCaptureRsp &operator=(const CreateScreenCaptureRsp &rRsp)
    {
        if (this != &rRsp) {
            this->contextId = rRsp.contextId;
            this->sinkAgentId = rRsp.sinkAgentId;
        }
        return *this;
    }

    IPC_BIND_ATTR(contextId, sinkAgentId)

    uint32_t contextId;
    uint32_t sinkAgentId;
};
```
| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| contextId | uint32_t | 是 | 业务容器 ID |
| sinkAgentId | uint32_t | 是 | 源代理 ID，0时表示创建失败。 |

<a name="hszq7"></a>
### DestroyScreenCapture
 int32_t DestroyScreenCapture（DestroyScreenCaptureReq &msg）;<br />删除屏幕采集，与之相关的投屏都会停止。<br />**请求参数结构体：**DestroyScreenCaptureReq

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| contextId | uint32_t | 是 | 业务容器 ID |
| sinkAgentId | uint32_t | 是 | 流代理 ID。 |

<a name="v4xei"></a>
### AppendCast
int32_t AppendCast (AppendCastReq &msg, AppendCastRsp &reply)<br />source端开始连接选中的设备，并把对应的媒体源投屏过去。<br />**请求参数结构体：**AddCastReq

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| contextId | uint32_t | 是 | 业务容器 ID |
| sinkAgentId | uint32_t | 是 | 源流代理 ID。 |
| deviceId | string | 是 | 设备 ID。 |

**返回参数结构体：**AddCastRsp

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| srcAgentId | uint32_t | 是 | 推流代理 ID。 |

<a name="ykwsW"></a>
### RemoveCast
 int32_t RemoveCast（RemoveCastReq &msg）;<br />主动断开投屏。<br />**请求参数结构体：**RemoveCastReq

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| contextId | uint32_t | 是 | 业务容器 ID。 |
| srcAgentId | uint32_t | 是 | 推流代理 ID。 |

