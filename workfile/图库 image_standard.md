# 简介
**image_standard仓库**提供了一系列易用的接口用于存放image的源码信息，提供创建图片源和位图管理能力，支持运行标准系统的设备使用。
# 图库架构
![](https://cdn.nlark.com/yuque/0/2022/jpeg/27487736/1659058425327-06c08b29-0d4f-4640-96fc-91b6fed771de.jpeg)
**                                    图1** Image组件架构图
支持能力列举如下：

- 创建、释放位图。
- 读写像素。
- 获取位图信息。
- 创建、释放图片源。
- 获取图片源信息。
- 创建、释放packer实例。
# 目录
仓目录结构如下：
```cpp
/foundation/multimedia/image_standard  
├── frameworks                                  # 框架代码
│   ├── innerkitsimpl                           # 内部接口实现
│   │   └──image                                # Native 实现
│   └── kitsimpl                                # 外部接口实现
│       └──image                                # 外部  NAPI 实现
├── interfaces                                  # 接口代码
│   ├── innerkits                               # 内部 Native 接口
│   └── kits                                    # 外部 JS 接口
├── plugins                                     # 图片编解码插件
│   ├── common
│   │   └── libs
│   │       ├── BUILD.gn
│   │       └── image
│   │           ├── formatagentplugin
│   │           ├── libbmpplugin                # bmp 解码插件
│   │           │   ├── BUILD.gn
│   │           │   ├── include
│   │           │   │   ├── bmp_decoder.h
│   │           │   └── src
│   │           │       ├── bmp_decoder.cpp
│   │           ├── libgifplugin                # gif 解码插件
│   │           │   ├── BUILD.gn
│   │           │   ├── include
│   │           │   │   └── gif_decoder.h
│   │           │   └── src
│   │           │       ├── gif_decoder.cpp
│   │           ├── libheifplugin               # heif 解码插件
│   │           │   ├── BUILD.gn_old
│   │           │   ├── include
│   │           │   │   ├── heif_decoder.h
│   │           │   └── src
│   │           │       ├── heif_decoder.cpp
│   │           ├── libjpegplugin               # jpeeg 编解码插件
│   │           │   ├── BUILD.gn
│   │           │   ├── include
│   │           │   │   ├── jpeg_decoder.h
│   │           │   │   ├── jpeg_encoder.h
│   │           │   └── src
│   │           │       ├── jpeg_decoder.cpp
│   │           │       ├── jpeg_encoder.cpp
│   │           ├── libpngplugin                # png 解码插件
│   │           │   ├── BUILD.gn
│   │           │   ├── include
│   │           │   │   ├── png_decoder.h
│   │           │   └── src
│   │           │       ├── png_decoder.cpp
│   │           └── libwebpplugin               # wbmp 解码插件
│   │               ├── BUILD.gn
│   │               ├── include
│   │               │   └── webp_decoder.h
│   │               ├── src
│   │                   └── webp_decoder.cpp
├── LICENSE                                     # 证书文件
├── ohos.build                                  # 编译文件
├── sa_profile                                  # 服务配置文件
└── services                                    # 服务实现
```
# 接口说明
## 导入模块
```javascript
import image from '@ohos.multimedia.image';
```
## image.createPixelMap
createPixelMap(colors: ArrayBuffer, options: InitializationOptions, callback: AsyncCallback<PixelMap>): void
通过属性**创建PixelMap对象**，通过回调函数返回结果。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| colors | ArrayBuffer | 是 | BGRA_8888格式的颜色数组。 |
| options | [InitializationOptions](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#initializationoptions8) | 是 | 属性。 |
| callback | AsyncCallback<[PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7)> | 是 | 通过回调返回PixelMap对象。 |

**示例：**
```javascript
const color = new ArrayBuffer(96);
let bufferArr = new Uint8Array(color);
let opts = { editable: true, pixelFormat: 3, size: { height: 4, width: 6 } }
image.createPixelMap(color, opts, (error, pixelmap) => {
    if(error) {
        console.log('Failed to create pixelmap.');
    } else {
        console.log('Succeeded in creating pixelmap.');
    }
})
```
## PixelMap
**图像像素**类，用于**读取或写入图像数据以及获取图像信息**。在调用PixelMap的方法前，需要先通过createPixelMap创建一个PixelMap实例。
**系统能力：** SystemCapability.Multimedia.Image.Core

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| isEditable | boolean | 是 | 否 | 设定是否图像像素可被编辑。 |

### readPixelsToBuffer
readPixelsToBuffer(dst: ArrayBuffer, callback: AsyncCallback<void>): void
读取图像像素数据，结果写入ArrayBuffer里，使用callback形式返回。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| dst | ArrayBuffer | 是 | 缓冲区，函数执行结束后获取的图像像素数据写入到该内存区域内。缓冲区大小由getPixelBytesNumber接口获取。 |
| callback | AsyncCallback<void> | 是 | 获取回调，失败时返回错误信息。 |

**示例：**
```javascript
const readBuffer = new ArrayBuffer(96);
pixelmap.readPixelsToBuffer(readBuffer, (err, res) => {
    if(err) {
        console.log('Failed to read image pixel data.');  //不符合条件则进入
    } else {
        console.log('Succeeded in reading image pixel data.');  //符合条件则进入
    }
})
```
### readPixels
readPixels(area: PositionArea, callback: AsyncCallback<void>): void
**读取区域内的图片数据**，使用callback形式返回读取结果。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| area | [PositionArea](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#positionarea7) | 是 | 区域大小，根据区域读取。 |
| callback | AsyncCallback<void> | 是 | 获取回调，失败时返回错误信息。 |

**示例：**
```javascript
const color = new ArrayBuffer(96);
let bufferArr = new Uint8Array(color);
let opts = { editable: true, pixelFormat: 3, size: { height: 4, width: 6 } }
image.createPixelMap(color, opts, (err, pixelmap) => {
    if(pixelmap == undefined){
        console.info('createPixelMap failed.');
    } else {
        const area = { pixels: new ArrayBuffer(8),
            offset: 0,
            stride: 8,
            region: { size: { height: 1, width: 2 }, x: 0, y: 0 }};
        pixelmap.readPixels(area, () => {
            console.info('readPixels success');
        })
    }
})
```
### writePixels
writePixels(area: PositionArea, callback: AsyncCallback<void>): void
将PixelMap写入指定区域内，使用callback形式返回写入结果。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| area | [PositionArea](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#positionarea7) | 是 | 区域，根据区域写入。 |
| callback | AsyncCallback<void> | 是 | 获取回调，失败时返回错误信息。 |

**示例：**
```javascript
const area = new ArrayBuffer(400);
pixelmap.writePixels(area, (error) => {
    if (error != undefined) {
		console.info('Failed to write pixelmap into the specified area.');
	} else {
	    const readArea = {
            pixels: new ArrayBuffer(20),
            offset: 0,
            stride: 8,
            region: { size: { height: 1, width: 2 }, x: 0, y: 0 },
        }
	}
})
```
### writeBufferToPixels
writeBufferToPixels(src: ArrayBuffer, callback: AsyncCallback<void>): void
读取缓冲区中的图片数据，结果写入PixelMap中，使用callback形式返回。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| src | ArrayBuffer | 是 | 图像像素数据。 |
| callback | AsyncCallback<void> | 是 | 获取回调，失败时返回错误信息。 |

**示例：**
```javascript
const color = new ArrayBuffer(96);
const pixelMap = new ArrayBuffer(400);
let bufferArr = new Uint8Array(color);
pixelMap.writeBufferToPixels(color, function(err) {
    if (err) {
        console.error("Failed to write data from a buffer to a PixelMap.");
        return;
    } else {
		console.log("Succeeded in writing data from a buffer to a PixelMap.");
	}
});
```
### getImageInfo
getImageInfo(callback: AsyncCallback<ImageInfo>): void
获取图像像素信息，使用callback形式返回获取的图像像素信息。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<[ImageInfo](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imageinfo)> | 是 | 获取图像像素信息回调，异步返回图像像素信息，失败时返回错误信息。 |

**示例:**
```javascript
pixelmap.getImageInfo((imageInfo) => { 
    console.log("Succeeded in obtaining the image pixel map information.");
})
```
## image.createImageSource
createImageSource(uri: string, options: SourceOptions): ImageSource
通过传入的**uri**创建图片源实例。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| uri | string | 是 | 图片路径，当前仅支持应用沙箱路径。 |
| options | [SourceOptions](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#SourceOptions) | 是 | 图片属性，包括图片序号与默认属性值。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| [ImageSource](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imagesource) | 返回ImageSource类实例，失败时返回undefined。 |

**示例：**
```javascript
let path = this.context.getApplicationContext().fileDirs + "test.jpg";
const imageSourceApi = image.createImageSource(path);
```
## image.createImageSource
createImageSource(fd: number, options: SourceOptions): ImageSource
通过传入**文件描述符**来创建**图片源实例**。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| fd | number | 是 | 文件描述符fd。 |
| options | [SourceOptions](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#SourceOptions) | 是 | 图片属性，包括图片序号与默认属性值。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| [ImageSource](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imagesource) | 返回ImageSource类实例，失败时返回undefined。 |

**示例：**
```javascript
const imageSourceApi = image.createImageSource(fd);
```
## image.createImageSource
createImageSource(buf: ArrayBuffer, options: SourceOptions): ImageSource
通过**缓冲区**创建图片源实例。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| buf | ArrayBuffer | 是 | 图像缓冲区数组。 |
| options | [SourceOptions](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#SourceOptions) | 是 | 图片属性，包括图片序号与默认属性值。 |

**返回值：**


**示例：**
```javascript
const data = new ArrayBuffer(112); 
const imageSourceApi = image.createImageSource(data);
```
## ImageSource
**图片源**类，用于**获取图片相关信息**。在调用ImageSource的方法前，需要先通过createImageSource构建一个ImageSource实例。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| supportedFormats | Array<string> | 是 | 否 | 支持的图片格式，包括：png，jpeg，wbmp，bmp，gif，webp，heif等。 |

### getImageInfo
getImageInfo(index: number, callback: AsyncCallback<ImageInfo>): void
获取**指定序号**的图片信息，使用callback形式返回图片信息。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| index | number | 是 | 创建图片源时的序号。 |
| callback | AsyncCallback<[ImageInfo](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imageinfo)> | 是 | 获取图片信息回调，异步返回图片信息对象。 |

**示例：**
```javascript
imageSourceApi.getImageInfo(0,(error, imageInfo) => { 
    if(error) {
        console.log('getImageInfo failed.');
    } else {
        console.log('getImageInfo succeeded.');
    }
})
```
### getImageInfo
getImageInfo(callback: AsyncCallback<ImageInfo>): void
获取图片信息，使用callback形式返回图片信息。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<[ImageInfo](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imageinfo)> | 是 | 获取图片信息回调，异步返回图片信息对象。 |

**示例：**
```javascript
imageSourceApi.getImageInfo(imageInfo => { 
    console.log('Succeeded in obtaining the image information.');
})
```
### getImageProperty
getImageProperty(key:string, callback: AsyncCallback<string>): void
获取图片中给定**索引处图像的指定属性键的值**，用callback形式返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| key | string | 是 | 图片属性名。 |
| callback | AsyncCallback<string> | 是 | 获取图片属性回调，返回图片属性值，如获取失败则返回属性默认值。 |

**示例：**
```javascript
imageSourceApi.getImageProperty("BitsPerSample",(error,data) => { 
    if(error) {
        console.log('Failed to get the value of the specified attribute key of the image.');
    } else {
        console.log('Succeeded in getting the value of the specified attribute key of the image.');
    }
})
```
### getImageProperty
getImageProperty(key:string, options: GetImagePropertyOptions, callback: AsyncCallback<string>): void
获取图片指定属性键的值，callback形式返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| key | string | 是 | 图片属性名。 |
| options | [GetImagePropertyOptions](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#getimagepropertyoptions7) | 是 | 图片属性，包括图片序号与默认属性值。 |
| callback | AsyncCallback<string> | 是 | 获取图片属性回调，返回图片属性值，如获取失败则返回属性默认值。 |

**示例：**
```javascript
const property = new ArrayBuffer(400);
imageSourceApi.getImageProperty("BitsPerSample",property,(error,data) => { 
    if(error) {
        console.log('Failed to get the value of the specified attribute key of the image.');
    } else {
        console.log('Succeeded in getting the value of the specified attribute key of the image.');
    }
})
```
### modifyImageProperty
modifyImageProperty(key: string, value: string, callback: AsyncCallback<void>): void
通过**指定的键修改图片属性**的值，callback形式返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| key | string | 是 | 图片属性名。 |
| value | string | 是 | 属性值。 |
| callback | AsyncCallback | 是 | 修改属性值，callback返回结果。 |

**示例：**
```javascript
imageSourceApi.modifyImageProperty("ImageWidth", "80",() => {})
```
### updateData
updateData(buf: ArrayBuffer, isFinished: boolean, value: number, length: number, callback: AsyncCallback<void>): void
**更新增量数据**，callback形式返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| buf | ArrayBuffer | 是 | 增量数据。 |
| isFinished | boolean | 是 | 是否更新完。 |
| value | number | 否 | 偏移量。 |
| length | number | 否 | 数组长。 |
| callback | AsyncCallback | 是 | 回调表示成功或失败。 |

**示例：**
```javascript
const array = new ArrayBuffer(100);
imageSourceIncrementalSApi.updateData(array, false, 0, 10,(error,data )=> {
            if(data !== undefined){
                console.info('Succeeded in updating data.');     
            }
		})
```
### createPixelMap
createPixelMap(callback: AsyncCallback<PixelMap>): void
通过**默认参数**创建PixelMap对象，使用callback形式返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<[PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7)> | 是 | 通过回调返回PixelMap对象。 |

**示例：**
```javascript
imageSourceApi.createPixelMap((err, pixelmap) => {
                    console.info('Succeeded in creating pixelmap object.')；
                })
```
### createPixelMap
createPixelMap(options: DecodingOptions, callback: AsyncCallback<PixelMap>): void
通过**图片解码参数**创建PixelMap对象。
**系统能力：** SystemCapability.Multimedia.Image.ImageSource
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| options | [DecodingOptions](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#decodingoptions7) | 是 | 解码参数。 |
| callback | AsyncCallback<[PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7)> | 是 | 通过回调返回PixelMap对象。 |

**示例：**
```javascript
const decodingOptions = new ArrayBuffer(400);
imageSourceApi.createPixelMap(decodingOptions, pixelmap => { 
    console.log('Succeeded in creating pixelmap object.');
})
```
## image.createImagePacker
createImagePacker(): ImagePacker
创建ImagePacker实例。
**系统能力：** SystemCapability.Multimedia.Image.ImagePacker
**返回值：**

| **类型** | **说明** |
| --- | --- |
| [ImagePacker](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imagepacker) | 返回ImagePacker实例。 |

**示例：**
```javascript
const imagePackerApi = image.createImagePacker();
```
## imagePacker
图片打包器类，用于图片压缩和打包。在调用ImagePacker的方法前，需要先通过createImagePacker构建一个ImagePacker实例。
**系统能力：** SystemCapability.Multimedia.Image.ImagePacker

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| supportedFormats | Array<string> | 是 | 否 | 图片打包支持的格式，jpeg。 |

### packing
packing(source: PixelMap, option: PackingOption, callback: AsyncCallback<ArrayBuffer>): void
图片压缩或重新打包，使用callback形式返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImagePacker
**参数：**

| **参数名** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| source | [PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap) | 是 | 打包的PixelMap资源。 |
| option | [PackingOption](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#packingoption) | 是 | 设置打包参数。 |
| callback | AsyncCallback<ArrayBuffer> | 是 | 获取图片打包回调，返回打包后数据。 |

**示例：**
```javascript
let packOpts = { format:"image/jpeg", quality:98 }
const pixelMapApi = new ArrayBuffer(400);
imagePackerApi.packing(pixelMapApi, packOpts, data => { 
    console.log('Succeeded in packing the image.');
}).catch(error => {
	console.log('Failed to pack the image.');
})

```
## image.createImageReceiver
createImageReceiver(width: number, height: number, format: number, capacity: number): ImageReceiver
通过宽、高、图片格式、容量**创建ImageReceiver实例**。

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| width | number | 是 | 图像的默认宽度。 |
| height | number | 是 | 图像的默认高度。 |
| format | number | 是 | 图像格式。 |
| capacity | number | 是 | 同时访问的最大图像数。 |

**返回值：**

| **类型** | **说明** |
| --- | --- |
| [ImageReceiver](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imagereceiver9) | 如果操作成功，则返回ImageReceiver实例。 |

**示例：**
```javascript
var receiver = image.createImageReceiver(1920, 1080, 4, 8);
```
## ImageReceiver
图像接收类，用于获取组件surface id，**接收最新的图片和读取下一张图片**，以及释放ImageReceiver实例。
在调用以下方法前需要先创建ImageReceiver实例。

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| size | [Size](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#size) | 是 | 否 | 图片大小。 |
| capacity | number | 是 | 否 | 同时访问的图像数。 |
| format | [ImageFormat](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#imageformat9) | 是 | 否 | 图像格式。 |

### getReceivingSurfaceId
getReceivingSurfaceId(callback: AsyncCallback<string>): void
用于获取一个surface id**供Camera或其他组件使用**。使用callback返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageReceiver
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<string> | 是 | 回调函数，返回surface id。 |

**示例:**
```javascript
receiver.getReceivingSurfaceId((err, id) => { 
    if(err) {
        console.log('getReceivingSurfaceId failed.');
    } else {
        console.log('getReceivingSurfaceId succeeded.');
    }
});
```
### readLatestImage
readLatestImage(callback: AsyncCallback<Image>): void
从ImageReceiver**读取最新的图片**，并使用callback返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageReceiver
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<[Image](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#image9)> | 是 | 回调函数，返回最新图像。 |

**示例：**
```javascript
receiver.readLatestImage((err, img) => { 
    if(err) {
        console.log('readLatestImage failed.');
    } else {
        console.log('readLatestImage succeeded.');
    }
});
```
### readNextImage
readNextImage(callback: AsyncCallback<Image>): void
从ImageReceiver**读取下一张图片**，并使用callback返回结果。
**系统能力：** SystemCapability.Multimedia.Image.ImageReceiver
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| callback | AsyncCallback<[Image](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#image9)> | 是 | 回调函数，返回下一张图片。 |

**示例：**
```javascript
receiver.readNextImage((err, img) => { 
    if(err) {
        console.log('readNextImage failed.');
    } else {
        console.log('readNextImage succeeded.');
    }
});
```
### on('imageArrival')
on(type: 'imageArrival', callback: AsyncCallback<void>): void
**接收图片时注册回调**。
**系统能力：** SystemCapability.Multimedia.Image.ImageReceiver
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| type | string | 是 | 注册事件的类型，固定为'imageArrival'，接收图片时触发。 |
| callback | AsyncCallback<void> | 是 | 注册的事件回调。 |

**示例：**
```javascript
receiver.on('imageArrival', () => {})
```
## image
提供基本的图像操作，包括**获取图像信息、读写图像数据**。调用[readNextImage](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#readnextimage9)和[readLatestImage](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#readlatestimage9)接口时会返回image。
**系统能力：** 以下各项对应的系统能力均为SystemCapability.Multimedia.Image.Core

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| clipRect | [Region](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#region7) | 是 | 是 | 要裁剪的图像区域。 |
| size> | [Size](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#size) | 是 | 否 | 图像大小。 |
| format | number | 是 | 否 | 图像格式，参考[PixelMapFormat](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmapformat7) |

### getComponent
getComponent(componentType: ComponentType, callback: AsyncCallback<Component>): void
根据图像的**组件类型**从图像中获取**组件缓存**并使用callback返回结果。
**系统能力：** SystemCapability.Multimedia.Image.Core
**参数：**

| **名称** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| componentType | [ComponentType](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#componenttype9) | 是 | 图像的组件类型。 |
| callback | AsyncCallback<[Component](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#component9)> | 是 | 用于返回组件缓冲区。 |

**示例：**
```javascript
img.getComponent(4, (err, component) => {
    if(err) {
        console.log('getComponent failed.');
    } else {
        console.log('getComponent succeeded.');
    }
})
```
## ComponentType
枚举，图像的组件类型。
**系统能力：** 以下各项对应的系统能力均为SystemCapability.Multimedia.Image.ImageReceiver

| **名称** | **默认值** | **描述** |
| --- | --- | --- |
| YUV_Y | 1 | 亮度信息。 |
| YUV_U | 2 | 色度信息。 |
| YUV_V | 3 | 色度信息。 |
| JPEG | 4 | Jpeg 类型。 |

## Component
描述图像颜色分量。
**系统能力：** 以下各项对应的系统能力均为SystemCapability.Multimedia.Image.Core

| **名称** | **类型** | **可读** | **可写** | **说明** |
| --- | --- | --- | --- | --- |
| componentType | [ComponentType](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#componenttype9) | 是 | 否 | 组件类型。 |
| rowStride | number | 是 | 否 | 行距。 |
| pixelStride | number | 是 | 否 | 像素间距。 |
| byteBuffer | ArrayBuffer | 是 | 否 | 组件缓冲区。 |

# 实例
## 1.裁剪图片
主要流程为：
1. 通过**buffer**创建imageSource
2. 通过GetFileSize获取文件大小
3.通过ReadFileToBuffer读取文件到buffer
        4.通过CreateImageSource创建imageSource对象
       5.通过**图片解码参数**创建PixelMap对象
```cpp
    /**
     * @tc.steps: step1. create image source by buffer source stream and default format hit
     * @tc.expected: step1. create image source success.
     */
    size_t bufferSize = 0;
    bool ret = ImageUtils::GetFileSize(IMAGE_INPUT_JPEG_PATH, bufferSize);
    ASSERT_EQ(ret, true);
    uint8_t *buffer = (uint8_t *)malloc(bufferSize);
    ASSERT_NE(buffer, nullptr);
    ret = OHOS::ImageSourceUtil::ReadFileToBuffer(IMAGE_INPUT_JPEG_PATH, buffer, bufferSize);
    ASSERT_EQ(ret, true);

    uint32_t errorCode = 0;
    SourceOptions opts;
    std::unique_ptr<ImageSource> imageSource = ImageSource::CreateImageSource(buffer, bufferSize, opts, errorCode);
    ASSERT_EQ(errorCode, SUCCESS);
    ASSERT_NE(imageSource.get(), nullptr);

    /**
     * @tc.steps: step2. decode image source to pixel map by default decode options
     * @tc.expected: step2. decode image source to pixel map success.
     */
    DecodeOptions decodeOpts;
    std::unique_ptr<PixelMap> pixelMap = imageSource->CreatePixelMap(decodeOpts, errorCode);
    ASSERT_EQ(errorCode, SUCCESS);
    ASSERT_NE(pixelMap.get(), nullptr);

    ImageInfo imageInfo;
    pixelMap->GetImageInfo(imageInfo);
    decodeOpts.CropRect = { imageInfo.size.width - 100, imageInfo.size.height - 100, 100, 100 };
    std::unique_ptr<PixelMap> cropPixelMap = imageSource->CreatePixelMap(decodeOpts, errorCode);
    ASSERT_NE(pixelMap.get(), nullptr);
    cropPixelMap->GetImageInfo(imageInfo);
  
    /**
     * @tc.steps: step3. compress the pixel map to jpeg file.
     * @tc.expected: step3. pack pixel map success and the jpeg compress file size equals to JPEG_PACK_SIZE.
     */
    ImagePacker imagePacker;
    int64_t packSize = OHOS::ImageSourceUtil::PackImage(IMAGE_OUTPUT_JPEG_BUFFER_PATH, std::move(cropPixelMap));
 
    // ASSERT_NE(packSize, 0);
    packSize = 0;
    free(buffer);
```
**剪切前**![home.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27487736/1659016399565-a1b8c176-06c8-473e-b7d4-3583d01910a8.jpeg#averageHue=%23a38974&clientId=u04062322-dd59-4&from=ui&id=u64c41503&originHeight=141&originWidth=176&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19244&status=done&style=none&taskId=u6d5675e8-dc42-4ee7-bf0f-65bc2f5d50c&title=)
**剪切后**![home_buffer.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27487736/1659016417124-8881e3b5-a18e-4d0b-81a7-91b986d9be43.jpeg#averageHue=%23886c57&clientId=u04062322-dd59-4&from=ui&id=u1f8c1c8d&originHeight=100&originWidth=100&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9654&status=done&style=none&taskId=u6c9f04e7-e288-4ae5-8f9b-fe8c7ddb209&title=)
也可**通过path创建imageSource**
```cpp
    /**
     * @tc.steps: step1. create image source by correct jpeg data and jpeg format hit.
     * @tc.expected: step1. create image source success.
     */
    uint32_t errorCode = 0;
    SourceOptions opts;
    std::unique_ptr<ImageSource> imageSource = ImageSource::CreateImageSource(IMAGE_INPUT_JPEG_PATH, opts, errorCode);
    ASSERT_EQ(errorCode, SUCCESS);
    ASSERT_NE(imageSource.get(), nullptr);

    ImageInfo imageInfo;
    uint32_t index = 0;
    uint32_t ret = imageSource->GetImageInfo(index, imageInfo);
    ASSERT_EQ(ret, SUCCESS);
    ret = imageSource->GetImageInfo(imageInfo);
    ASSERT_EQ(ret, SUCCESS);
```
## 2.图片裁剪及旋转
```cpp
    /**
     * @tc.steps: step1. create jpg image source by istream source stream and default format hit
     * @tc.expected: step1. create jpg image source success.
     */
    std::unique_ptr<std::fstream> fs = std::make_unique<std::fstream>();
    fs->open("/data/local/tmp/image/test.jpg", std::fstream::binary | std::fstream::in);
    bool isOpen = fs->is_open();
    ASSERT_EQ(isOpen, true);
    uint32_t errorCode = 0;
    SourceOptions opts;
    std::unique_ptr<ImageSource> imageSource = ImageSource::CreateImageSource(std::move(fs), opts, errorCode);
    ASSERT_EQ(errorCode, SUCCESS);
    ASSERT_NE(imageSource.get(), nullptr);
    /**
     * @tc.steps: step2. crop jpg image source to pixel map crop options
     * @tc.expected: step2. crop jpg image source to pixel map success.
     */
    DecodeOptions decodeOpts;
    decodeOpts.CropRect.top = 3;
    decodeOpts.CropRect.width = 100;
    decodeOpts.CropRect.left = 3;
    decodeOpts.CropRect.height = 200;
    decodeOpts.desiredSize.width = 200;
    decodeOpts.desiredSize.height = 400;
    decodeOpts.rotateDegrees = 90;
    std::unique_ptr<PixelMap> pixelMap = imageSource->CreatePixelMap(decodeOpts, errorCode);
    HiLog::Debug(LABEL_TEST, "create pixel map error code=%{public}u.", errorCode);
    ASSERT_EQ(errorCode, SUCCESS);
    ASSERT_NE(pixelMap.get(), nullptr);
    EXPECT_EQ(200, pixelMap->GetWidth());
    EXPECT_EQ(400, pixelMap->GetHeight());

    ImageInfo imageInfo;
    pixelMap->GetImageInfo(imageInfo);
    // decodeOpts.CropRect = { imageInfo.size.width - 100, imageInfo.size.height - 100, 100, 100 };
    std::unique_ptr<PixelMap> cropPixelMap = imageSource->CreatePixelMap(decodeOpts, errorCode);
    ASSERT_NE(pixelMap.get(), nullptr);
    cropPixelMap->GetImageInfo(imageInfo);
  
    /**
     * @tc.steps: step3. compress the pixel map to jpeg file.
     * @tc.expected: step3. pack pixel map success and the jpeg compress file size equals to JPEG_PACK_SIZE.
     */
    ImagePacker imagePacker;
    int64_t packSize = OHOS::ImageSourceUtil::PackImage(IMAGE_OUTPUT_JPEG_BUFFER_PATH, std::move(cropPixelMap));

```
**图片裁剪旋转前**![test.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27487736/1659065783737-727fbb03-72e9-4961-944e-2e90b450b52d.jpeg#averageHue=%23ffc90d&clientId=u50165d8d-2425-4&from=drop&id=axM5w&originHeight=226&originWidth=472&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2431&status=done&style=none&taskId=u4cc1a16c-1358-49c9-bd12-01063303221&title=)![image.png](https://cdn.nlark.com/yuque/0/2022/png/27487736/1661951866643-31963a62-1488-4967-9b0e-95e940ea654b.png#averageHue=%23f8f7f6&clientId=ub4c9ab54-69cb-4&from=paste&height=200&id=Fys1c&originHeight=332&originWidth=394&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17836&status=done&style=none&taskId=ubc91de75-90d7-422b-885b-a5fecd31ff1&title=&width=237.1999969482422)![image.png](https://cdn.nlark.com/yuque/0/2022/png/27487736/1661951882155-87104534-9fb3-43b9-b2f5-bbb2e03dc2e8.png#averageHue=%23f9f8f7&clientId=ub4c9ab54-69cb-4&from=paste&height=204&id=mofn3&originHeight=292&originWidth=373&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15170&status=done&style=none&taskId=u3e0bdc7c-c3d4-46bc-b636-18925b06078&title=&width=260.40000915527344)
**图片裁剪旋转后**![test1.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27487736/1659065783763-e0085749-65b8-46a0-b0f6-db5006c47089.jpeg#averageHue=%23ffc90d&clientId=u50165d8d-2425-4&from=drop&id=iRRo8&originHeight=400&originWidth=200&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1933&status=done&style=none&taskId=u318bef22-1d11-4cba-baca-e504a6b27e3&title=)![image.png](https://cdn.nlark.com/yuque/0/2022/png/27487736/1661995914234-ae419b58-3ed3-4037-8c1d-677fe898903d.png#averageHue=%23f8f8f7&clientId=ub4c9ab54-69cb-4&from=paste&height=289&id=idY1Y&originHeight=381&originWidth=397&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19422&status=done&style=none&taskId=u0e5dc885-b95a-4c0f-9254-ae49c57a434&title=&width=301.6000061035156)![image.png](https://cdn.nlark.com/yuque/0/2022/png/27487736/1661995938747-de7716c3-df6c-4066-81cc-514b9b5e87b4.png#averageHue=%23f9f8f6&clientId=ub4c9ab54-69cb-4&from=paste&height=232&id=Hsde2&originHeight=290&originWidth=369&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14039&status=done&style=none&taskId=u64275087-ce55-4529-866c-bf11d1d453c&title=&width=295.2)
## 3.相机拍照
```cpp
class CameraService {  
  constructor() {
    this.mReceiver = image.createImageReceiver(CameraSize.WIDTH, CameraSize.HEIGHT, 4, 8)
    Logger.info(this.tag, 'createImageReceiver')
    this.mReceiver.on('imageArrival', () => {  // 事件回调上来之后，触发后面的流程
      Logger.info(this.tag, 'imageArrival')
      this.mReceiver.readNextImage((err, image) => {
        Logger.info(this.tag, 'readNextImage')
        if (err || image === undefined) {
          Logger.error(this.tag, 'failed to get valid image')
          return
        }
        image.getComponent(4, (errMsg, img) => {
          Logger.info(this.tag, 'getComponent')
          if (errMsg || img === undefined) {
            Logger.info(this.tag, 'failed to get valid buffer')
            return
          }
          let buffer = new ArrayBuffer(4096)
          if (img.byteBuffer) {
            buffer = img.byteBuffer
          } else {
            Logger.error(this.tag, 'img.byteBuffer is undefined')
          }
          this.savePicture(buffer, image)
        })
      })
    })
  }
```

**主要流程为：**

1. 相机在启动服务同时，调用图库接口创建imageReceiver实例
2. 图库实例启动监听事件 imageArrival
3. 相机驱动往OnBufferAvaliable接口塞数据时，数据通过napi回调给应用
4. 应用调用readNextImage接口进行保存图片

![](https://cdn.nlark.com/yuque/0/2022/jpeg/27487736/1662469504316-499e00fe-f66c-4e94-a3cb-21b6b4827b4a.jpeg)
# 后记
**图库单元测试用例使用方法：**
1.在ohos.build中添加test，专门用于测试
```cpp
      "test": [
         "//foundation/multimedia/image_standard:image_test_list"
       ],
```
2.整体编译
```cpp
./build.sh --product-name RK3568-KHDVK
```
编译之后，发现在out/rkxxx/tests/目录下没有出现单元测试的目录
3.执行编译模块命令
```cpp
ninja -C out/rkxxx  image_test_list
```
发现，在out/rk3568-khdvk/tests/unittest/multimedia_image/image_standard下面生成了需要的可执行文件
4.拷贝可执行程序到宿主机上指定目录（如果没有拷贝权限，使用命令 mount -o remount -rw /)
5.修改文件执行权限(chmod 777  xxx)
6.执行测试用例，查看结果
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27487736/1661998640694-8c5b7154-6844-406f-9068-0d1b337d1b51.png#averageHue=%23171717&clientId=ub4c9ab54-69cb-4&from=paste&height=434&id=u8e5be564&originHeight=542&originWidth=953&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82317&status=done&style=none&taskId=u6527c60d-e673-4bc3-b297-aeb6de05659&title=&width=762.4)
**好处**：添加或修改测试用例，编译简单，能快速验证。
