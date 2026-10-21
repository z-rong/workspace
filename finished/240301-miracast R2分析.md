# miracast R2分析

**多屏互动无线投屏**的标准协议之一的Miracast/WIDI(Wi-Fi Display Technical Specification)在2017/4/21发布了2.0版本(Release 2 final version), 在2017/7/31发布了2.1版本

此次修改,增加了不少内容,本文将对此梳理一下.

**改动1**:增加了新的发现方式

原来的接收端要寻找接收端是通过WIFI-P2P进行发现的, Version 2版本增加了mDNS发现方式, mDNS底层需要BSS(AP)/IP网支持. mDNS在很多领域都有应用,其中**Airplay**的发现过程就是使用的mDNS.

mDNS的机制我们不在这里详述,有兴趣的同学可以访问[**//blog.csdn.net/wirelessdisplay/article/details/78088992**](https://blog.csdn.net/wirelessdisplay/article/details/78088992)

**改动2**:连接建立过程**

在版本1中, RTSP TCP的建立是由SINK端建立的, 而版本2中,如果走mDNS/IP方式, 则TCP的建立是Source端发起,并发送TCP SYN包至SINK, 通过协商, 再确定RTSP TCP的链接方式, 再由SINK端发起RTSP TCP链接

**改动3***媒体传输方式

在版本2中,承载音频和视频的RTP可以通过TCP和UDP进行传输,而且可以随时切换, 并且为了音视频的传输质量, 特意增加了几个RTSP信令,用于调节buffer缓存, 分别是:

wfd2-transport-switch/wfd2-buffer-length/wfd2-audio-playback-status/wfd2-video-playback-status

**改动4**媒体协商

版本2中,增加了很多媒体格式,所有协商过程变动更加复杂

**改动5**增加视频格式

版本1中,视频大小支持1080P/H264, 版本2中,支持最高到4Kp60, 格式增加H265

**改动6**增加辅流

版本1中是没有辅流选项的, 辅流格式主要有PNG/JPEG/H264

**改动7**增加音频格式

增加CTA Audio/AAC-ELDv2格式

**改动8**增加音频格式

增加了不少命令, 其中有一个wfd2-direct-streaming-mode比较有意思, 他是从source发往sink的,用于表示主流是否在激活状态. 此命令还没有实测,实际效果不知如何

总体来讲, **Miracast/**WIDI新版本的修改主要目的就是增加音视频传输的质量, 其中mDNS和TCP的增加,主要借鉴**Airplay**, P2P的性能太依赖网卡质量导致有些场景P2P传输质量较差的问题.

也增加了流量控制方面命令,用于再网络情况较差的情况下,调整传输码流

当然更重要的是支持了H265, 在当前,在同等质量下, H265是能够显著的降低码流的唯一手段



R2新增能力字段描述：

```
wfd2-audio-codecs // 音频编码格式，新增CTA、AAC-ELDv2两种格式，原有格式增加了更多模式选项
wfd2-video-formats // 视频编码格式，新增H265，新增更多分辨率帧率
wfd2-aux-stream-formats // 辅流格式，格式为PNG/JPEG/H264 CBP
wfd2-direct-streaming-mode // 直流模式，不编码直接进行数据传输
wfd2-transport-switch // 媒体流传输通道切换，udp和tcp切换
wfd2-buffer-length  // 缓存buffer长度，用于判断释放支持tcp切换
wfd2-audio-playback-status // 当前sink端音频缓存长度以及音频pts，可能用于多个sink同步播放
wfd2-video-playback-status // 当前sink端视频缓存长度及pts
wfd2-cta-datablock-collection // 用于cta音频模式
wfd2-cta-audio-infoframe // 用于cta音频模式
```

