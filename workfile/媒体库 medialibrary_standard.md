# 功能简介

**medialibrary_standard** 仓库提供了一系列易用的接口用于获取媒体文件元数据信息。

> MediaLibrary接口暂不对外部应用开放, 仅内部使用。


支持能力列举如下：

- 查询音频、视频和图片文件元数据信息
- 查询图片和视频相册
- 媒体文件操作如创建、重命名、拷贝和删除
- 相册操作如创建、重命名和删除

> 当前版本3.1Release的medialibrary_standard非常的垃圾，接口实现混乱，ts定义的接口和napi实现的接口有部分出入，且冗余代码太多。可以说是multimedia子系统中设计和实现最烂的组件。


# 媒体库架构分析

媒体库主要包含1个服务和2个动态APP，动态APP功能上可以理解为和服务差不多，但是注册和使用上有服务不同。主要以NAPI-ts API对外提供服务。

**1个服务**：媒体库服务 medialibrary_service

**2个动态HAP APP**:

-  媒体库数据能力APP 
   - 进程名：com.ohos.medialibrary.MediaLibraryDataA
   - 引用动态库：libmedialibrary_data_ability.z.so
-  媒体扫描能力APP 
   - 进程名：com.ohos.medialibrary.MediaScannerAbilityA
   - 引用动态库：libmedia_scanner_service.z.so

![image-20220722175728593.png](https://cdn.nlark.com/yuque/0/2022/png/26957361/1658742037734-7e5f2b2e-4a96-4410-b0b8-f61b5f2f2e0d.png#clientId=u4f2cc2f9-c42c-4&from=ui&id=u43c66d3b&originHeight=321&originWidth=829&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20311&status=done&style=none&taskId=u7ef0c016-f9d9-4a46-81cd-2b0c256b8ce&title=)

#### 与其他仓库的异同

从架构上讲与多媒体其他仓库的异同点如下：

- 都有CS架构模式，以IPC进行通信，但是媒体库服务实际没有太大用处。
- 媒体库仓库新增两个动态HAP应用，提供操作数据库和扫描目录的能力。
- 其他仓库接口多数都有流程上的顺序性，比如播放器setSrc/Play/Stop，可以画流程图来辅助理解。
- 多媒体仓库的接口 功能简单，但是设计繁杂，内部源码不便于理解。流程图不适用于展现媒体库内部逻辑。

## 代码结构

```
medialibrary_standard
├── frameworks
│   ├── innerkitsimpl
│   │   ├── media_library							# libmedia_library.z.so
│   │   ├── medialibrary_data_ability 				# 【媒体库数据能力APP】
│   │   ├── media_library_manager					# libmedia_library_manager.z.so
│   │   ├── media_scanner							# 媒体扫描能力APP的源码
│   │   ├── signature								# Hap 签名证书
│   │   └── test
│   └── kitsimpl
│       └── medialibrary							# libmedialibrary.z.so --NAPI	[deps:libmedia_scanner_client.z.so]
├── interfaces
├── sa_profile
│   ├── 3007.xml								# medialibrary_service
│   └── BUILD.gn
└── services
    ├── media_library_service					# 【媒体库服务】
    │   └── src									# libmedialibrary_proxy.z.so + libmedialibrary_service.z.so
    └── media_scanner_service					# 【媒体扫描能力APP】
        ├── client								# libmedia_scanner_client.z.so
        ├── ipc
        └── media_scanner_ability				# libmedia_scanner_service.z.so
```

# 媒体库对外接口

接口繁杂，只列主要的接口。详细[点这里](https://gitee.com/openharmony/docs/blob/OpenHarmony-3.1-Release/zh-cn/application-dev/reference/apis/js-apis-medialibrary.md) 。

### 获取文件资源 getFileAssets

调用 【媒体库能力APP-MediaLibraryDataAbility】`Query()`接口查询数据库`media_library.db`。

### 新建文件资源 createAsset

调用 【媒体库能力APP-MediaLibraryDataAbility】`Insert()`接口插入数据库`media_library.db`。调用数据库接口插入之前，创建这个空文件。

### 查询公共目录 getPublicDirectory

NAPI直接返回媒体库的相对路径。例如："Camera/"、"Videos/"、"Pictures/"、"Audios/"、"Documents/"和"Download/"。

### 获取相册目录 getAlbums

检索数据库，返回有媒体内容的文件夹信息。

### 打开资源文件 FileAsset::open()

调用 【媒体库能力APP-MediaLibraryDataAbility】的`OpenFile`函数使用系统函数open()打开文件。

### 关闭资源文件 FileAsset::close()

系统函数`close(fd)`关闭文件描述符，然后调用【媒体库能力APP-MediaLibraryDataAbility】`Insert()`接口，图片的话创建缩略图和扫描此文件信息，并把信息插入到数据库。

### 获取文件缩略图 getThumbnail

1. 先调用【媒体库能力APP-MediaLibraryDataAbility】`Query()`的从`media_library.db`中读取缩略图id。
2. 根据ID从KVStore中读取缩略图图片数据。
3. 调用ImageStandard中的接口`CreateImageSource()/CreatePixelMap()` 生成图像数据并返回。
4. 关闭本地文件的时候，如果是新文件，则会检索数据库，需要的时候，调用ImageStandard的接口读取图像文件，生成缩略图写在KVStore中。

### 修改资源文件 commitModify

修改文件属性，修改时间等信息同步更新到数据库中。

### 【无用接口：NAPI有定义但TS没有暴露】

1.  获取资源文件
getMediaAssets、getAudioAssets、getVideoAssets、getImageAssets
调用medialibrary_service服务遍历相应目录下的对应类型的文件。 
2.  创建资源文件
createVideoAsset、createImageAsset、createAudioAsset
调用medialibrary_service服务使用系统函数创建对应类型的文件。 
3.  获取资源文件夹
getVideoAlbums、getImageAlbums 
4.  创建资源文件夹
createImageAsset、createAudioAsset 

# 媒体库服务 - medialibrary_service

代码路径 `services/media_library_service`，编译成动态库 libmedialibrary_service.z.so，以medialibrary_service 服务形式， 内部调用 MediaLibrary 类，提供目录`/storage/media/local/files/` 下文件的增删改查操作 能力。

【目前对外提供js接口没有直接调用此服务】

## 服务能力

1.  获取媒体资源 GetMediaAssets
系统函数`nftw()`遍历相应目录（只能是媒体库的目录），不是检索数据库。 
2.  获取图像资源 GetImageAssets
同上，根据文件后缀返回图像文件（bmp/bm/gif/jpg/jpeg/jpe/png/webp/raw/svg/heif）。 
3.  获取音频资源 GetAudioAssets
同上，返回音频文件（aac/mp3/flac/wav/ogg/m4a）。 
4.  获取视频资源 GetVideoAssets
同上，返回视频文件（mp4/3gp/mpg/mov/webm/mkv）。 
5.  获取图像文件夹资源 GetImageAlbumAssets
系统函数`opendir()/readdir()`遍历，返回包含图像文件的文件夹AlbumAsset。 
6.  获取视频文件夹资源 GetVideoAlbumAssets
同上，返回包含视频文件的文件夹AlbumAsset 
7.  新建媒体资源 CreateMedia
系统函数创建一个空文件，文件夹不存在则创建文件夹。 
8.  删除媒体资源 DeleteMedia
系统函数`remove()`删除文件。 
9.  修改媒体资源 ModifyMedia
修改资源数据，若路径有差异旧文件存在且新文件不存在，直接系统函数`rename()`重命名。 
10.  复制媒体资源 CopyMedia
系统函数`open()/sendfile()`复制文件。 
11.  新建媒体文件夹 CreateAlbumMedia
系统函数`mkdir()`创建目录。 
12.  删除媒体文件夹 DeleteAlbumMedia
系统函数`nftw()/remove()`递归删除文件夹。 
13.  修改媒体文件夹 ModifyAlbumMedia
系统函数`rename()`文件夹重命名。 

# 动态HAP简介

目前看到动态库可以以服务形式运行，和 以动态应用的形式运行。

动态HAP的编译脚本关键字为`ohos_hap("medialibrary_dynamic_hap"){}`，本质上是用纯C++代码实现的系统APP，对外提供类似SystemAbility的服务。

以服务运行的动态库类，构造函数之前有个宏定义`REGISTER_SYSTEM_ABILITY_BY_ID()`

```cpp
REGISTER_SYSTEM_ABILITY_BY_ID(MediaLibService, MEDIA_LIBRARY_SERVICE_ID, true);

// 展开形式
const bool CalculatorService_RegisterResult = SystemAbility::MakeAndRegisterAbility(
	new MediaLibService(MEDIA_LIBRARY_SERVICE_ID, true) );
```

以APP形式运行的动态库，构造函数之前有个宏定义`REGISTER_AA();` 主要就是注册这个ability，在需要的时候调用这个APP提供的接口。

```cpp
REGISTER_AA(MediaLibraryDataAbility);

// 展开形式
__attribute__((constructor)) void RegisterAAMediaLibraryDataAbility()
{
	AbilityLoader::GetInstance().RegisterAbility(
		MediaLibraryDataAbility, []()->Ability * { return new (std::nothrow) MediaLibraryDataAbility; } );
}
```

如果函数被设定为constructor属性，则该函数会在main（）函数执行之前被自动的执行；

若函数被设定为destructor属性，则该函数会在main（）函数执行之后或者exit（）被调用后被自动的执行。

# 媒体库数据能力APP - MediaLibraryDataAbility

路径 `frameworks/innerkitsimpl/medialibrary_data_ability/`，编译成动态库 libmedialibrary_data_ability.z.so，以APP com.ohos.medialibrary.MediaLibraryDataA 形式提供 操作媒体数据库的 能力。

数据库位置 "AbilityContext::GetDatabaseDir() /media_library.db" ，内部维护成员变量，`std::shared_ptr<NativeRdb::RdbStore> rdbStore_ = NativeRdb::RdbHelper::GetRdbStore(xxxx)`，使用**NativeRdb::RdbStore**来增删改查数据库`media_library.db`。

初始化的时候会创建数据库和表，以及查询VIEW。

主要能力接口有：

```cpp
// 操作数据库
// 如果是新建资源文件操作则会在这个函数内 open 创建一个文件
// 
// 如果是关闭文件，触发生成缩略图，然后需要扫描一下文件，更新数据库中媒体信息
int32_t Insert(const Uri &uri, const NativeRdb::ValuesBucket &value) override;

// 仅仅删除数据库中的记录
int32_t Delete(const Uri &uri, const NativeRdb::DataAbilityPredicates &predicates) override;

// 批量插入，循环调用Insert()
int32_t BatchInsert(const Uri &uri, const std::vector<NativeRdb::ValuesBucket> &values) override;

// 修改文件属性时，同步更新数据库
int32_t Update(const Uri &uri, const NativeRdb::ValuesBucket &value,
                       const NativeRdb::DataAbilityPredicates &predicates) override;

// 查询数据库
std::shared_ptr<NativeRdb::AbsSharedResultSet> Query(const Uri &uri,
            const std::vector<std::string> &columns,
            const NativeRdb::DataAbilityPredicates &predicates) override;

// open 打开该文件
int32_t OpenFile(const Uri &uri, const std::string &mode) override;

// 从数据库查询文件类型
std::string GetType(const Uri &uri) override;
```

# 媒体扫描能力APP - MediaScannerAbility

路径 `services/media_scanner_service/media_scanner_ability/`，编译成动态库 libmedia_scanner_service.z.so，以APP com.ohos.medialibrary.MediaScannerAbilityA 形式提供 扫描文件和目录的 能力。

扫描的默认ROOT路径为 `/storage/media/local/files/`。

该APP不直接提供接口，实现了`OnConnect()`和 `OnDisconnect()`基类接口。当APP第一次被拉起时，执行`OnConnect()`。

```cpp
sptr<IRemoteObject> MediaScannerAbility::OnConnect(const Want &want)
{
    OHOS::AppExecFwk::Ability::OnConnect(want);
	// Creating remote object and returning to client
    auto scannerAbilityService = new(std::nothrow) MediaScannerAbilityService();

    return scannerAbilityService->AsObject();
}
```

客户端连接此APP时能获取到 **MediaScannerAbilityService** 实例。MediaScannerAbilityService 是对 **MediaScanner** 单例的封装。

提供以下接口：

```cpp
int32_t ScanDirService(std::string &scanDirPath, const sptr<IRemoteObject> &callback) override;
int32_t ScanFileService(std::string &scanFilePath, const sptr<IRemoteObject> &callback) override;
bool IsScannerRunning() override;
```

需要扫描的时候，内部会启动扫描线程（当前配置最大并大为1）进行目录或者文件扫描。

**扫描文件**

先检查文件所在目录，全路径中是否有层级目录是隐藏文件夹，或者是否包含 `.nomedia` 文件。有的话返回`ERR_NOT_ACCESSIBLE`。

检查所在目录再数据库中`dataability:///media` 中的目录ID。

先用 `stat()`探测文件信息。存在Metadata结构中。如果是媒体库的文件，还要设置 媒体库相对路径。

如果是媒体文件，则 open(), 调用`AVMetadataHelperImpl`类 （avMetadataHelperService_ = MediaServiceFactory::GetInstance().CreateAVMetadataHelperService();）媒体服务`media_sevice`中的 `AVMetadataHelperServer/AVMetadataHelperEngineGstImpl/gstreamer`,来探测媒体元数据。

存储到数据库中。并发出数据库变更消息`DataAbilityHelper::NotifyChange(uri)`。

# 媒体数据库相关

媒体库操作的数据库位于`/data/accounts/account_0/appdata/com.ohos.medialibrary.MediaLibraryDataA/database/media_library.db`。是个SQLite3数据库文件，可以使用软件 SQLiteStudio 打开。数据库中主要用到`Files`数据表，以及几个包含查询语句的VIEW，其他都是空表。

![image-20220722185225875.png](https://cdn.nlark.com/yuque/0/2022/png/26957361/1658742067549-27ea87d8-fa32-4cf2-ab8b-b1a640a69939.png#clientId=u4f2cc2f9-c42c-4&from=ui&id=u463e8baa&originHeight=513&originWidth=1834&originalType=binary&ratio=1&rotation=0&showTitle=false&size=77746&status=done&style=none&taskId=u075a397e-e065-407f-879e-db405967152&title=)

Files 数据表主要字段如下所示。

| 字段 | 注释 | 示例 |
| --- | --- | --- |
| file_id | 自动递增的序号 | 1 |
| data | 文件绝对路径 | /storage/media/local/files/Pictures/Screenshots/Screenshot_23308248.jpg |
| size | 文件大小 | 1436880 |
| parent | 所属目录的file_id | 1 |
| date_added | 创建时间，unix时间戳 | 286 |
| date_modified | 修改时间。unix时间戳 | 286 |
| mime_type | 文件类型 | 图片 image/_    or  文件夹 file/_ |
| display_name | 显示名 | Screenshot_23308248.jpg |
| thumbnail | 缩略图存在KVStore中的key | 2e22f09596693a05f293e8c03a00a40ae834a701b86dbb9f0b56363fa43f0264_THU |
| width/height | 图像和视频文件的宽高 | 1920 |
| relative_path | 媒体库的相对路径 | Pictures/Screenshots/ |
| uri | 数据能力集uri | dataability:///media/image |

注释：
uri 字段：文件管理的接口listFile() 返回的文件列表中包含此属性，文件uri = ${uri}/${file_id}。
# 截屏APP使用媒体库示例

主要流程为：

1. 调用媒体库接口createAsset()创建空文件
2. 调用媒体库接口FileAsset::open()打开此文件获取文件fd。
3. 往此文件fd中写图像数据。
4. 调用媒体库接口FileAsset::close()关闭文件描述符。 
   1. 媒体库生成缩略图数据并存储。 
      1. 使用ImageSource读取图像，生成PixelMap。
      2. 调用ImagePacker的接口生成缩略图的PixelMap。
   2. 扫描文件信息，把媒体信息再更新到媒体数据库`media_library.db`。

![screenshot.png](https://cdn.nlark.com/yuque/0/2022/png/26957361/1658742098382-2b44b5c8-c605-4447-b52e-d3ad2288859e.png#clientId=u4f2cc2f9-c42c-4&from=ui&id=u71708f6e&originHeight=2153&originWidth=3076&originalType=binary&ratio=1&rotation=0&showTitle=false&size=164418&status=done&style=none&taskId=u759735f3-a265-486e-b04e-b6d57022437&title=)


