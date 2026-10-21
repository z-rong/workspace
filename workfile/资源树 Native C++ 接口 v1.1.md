## 概述
资源树管理是设备管理的数据分发中心，对外提供服务接口，对内与模型管理、数据持久化、设备接入模块进行数据交互。<br /><br />**起始版本：**9  **Version:** 1.0

## 汇总

### 文件
| **名称** | **描述** |
| --- | --- |
| resource_tree_type.h | 声明用于资源树中定义的结构体。 |
| iresource_tree_listener.h | 声明资源树监听类。 |
| resource_tree_api.h | 声明用于资源树的C++API接口。 |

## 枚举类型说明

### ResourceTreeErrorCode
**描述:** 资源树模块错误码。<br />

| **枚举** | **值** | **描述** |
| --- | --- | --- |
| ERR_OK | 0 | 无错误 |
| ERR_GENERAL_ERROR | -1 | 无类别通用错误 |
| ERR_INVALID_PARAMETER | -2 | 非法参数 |
| ERR_INVALID_ID | -3 | 无效的ID |
| ERR_DEVICE_UNREACHABLE | -4 | 设备不可用 |
| ERR_SERVICE_UNREACHABLE | -5 | 服务不可用 |
| ERR_SERVICE_UNINIT | -6 | 服务未初始化 |
| ERR_PARSE_FAILED | -7 | 解析失败 |
| ERR_IO_ERROR | -8 | 文件操作失败 |
| ERR_PID_AUTH_FAILED | -9 | micROS进程PID权限错误 |
| ERR_UID_AUTH_FAILED | -10 | micROS进程UID权限错误 |
| ERR_IPC_FAILED | -11 | ipc错误 |
| ERR_NETWORK_ERROR | -12 | 网络异常 |
| ERR_NO_MEMORY | -13 | 内存不足 |
| ERR_NULLPTR | -14 | 空指针错误 |
| ERR_DB_ERROR | -15 | 数据库错误 |
| ERR_PLUGIN_CLOSE_ERROR | -16 | 插件关闭失败 |
| ERR_LOAD_FILE_ERROR | -17 | 加载文件失败 |
| ERR_FILE_PARSER_ERROR | -18 | 文件解析错误 |
| ERR_REALPATH_FAIL | -19 | 文件路径错误 |
| ERR_DB_RESULTSET_EMPTY | -20 | 数据库查询结果为空 |
| ERR_MODEL_IN_USE | -21 | 模型仍然在使用中 |
| ERR_MODEL_NOT_EXIST | -22 | 模型不存在 |
| ERR_NODE_NOT_ADD | -23 | 载荷未添加错误 |




### 类型定义
| **名称** | **描述** |
| --- | --- |
| [DeviceInfo](#AbcDA) | 本机设备信息。 |
| [NodeInfo](#AbcDB) | 载荷设备信息。 |
| [ValueInfo](#AbcDC) | 属性字段信息。 |  
| [RoleInfo](#AbcDD) | micROS进程角色信息。 |

### 类定义
| **名称** | **描述** |
| --- | --- |
| [IResourceTreeAPIListener](#AbcDE) | 资源树监听注册接口。 |
| [ResourceTreeAPI](#AbcDF) | 资源树API接口。 |

## 类型定义说明

<a name="AbcDA"></a>

### DeviceInfo
**描述:** 本机设备信息结构体。<br /><br />**成员:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceId | std::string  | 本机设备ID。 |
| cpuCapacity | std::string  | cpu规格。 |
| cpuUsageRate | std::string | cpu使用率。 |
| ramCapacity | std::string | 内存大小。 |
| ramUsageRate | std::string | 内存使用率。 |
| storageCapacity | std::string | 设备存储空间总大小。 |
| storageUsageRate | std::string | 设备存储空间使用率。 |
| netWorkInfo | std::string | 网络信息。 |

<a name="AbcDB"></a>

### NodeInfo
**描述:** 载荷设备信息结构体。<br /><br />**成员:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| properties | std::map<std::string, ValueInfo> | 载荷设备属性集合。 |

<a name="AbcDC"></a>

### ValueInfo
**描述:** 单个属性值信息结构体。<br /><br />**成员:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| value | std::string | 属性值字符串。 |
| fieldType | std::string | 属性值字段类型。 |

<a name="AbcDD"></a>

### RoleInfo
**描述:** micROS进程权限信息结构体。<br /><br />**成员:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| micRosId | std::string | micROS进程ID。 |
| role | std::string  | micROS进程角色："Reader"进程具有读载荷设备属性权限, "Operator"进程具有读载荷设备属性权限和操作载荷设备权限。 |

## 类定义说明

<a name="AbcDE"></a>

### IResourceTreeAPIListener  
```cpp
class IResourceTreeAPIListener {
public:
    virtual void OnMessage(std::string& type, std::string& message) = 0;
};
```
**描述:** 监听接口类。<br /><br />**函数:**

| **名称** | **描述** |
| --- | --- |
| [OnMessage](#AbcDG) | 载荷设备通知上报接口。 |

## 函数说明

<a name="AbcDG"></a>
### OnMessage
```cpp
void OnMessage(std::string& type, std::string& message)
```
**描述:** 载荷设备通知上报，包括设备上下线通知，属性变更通知，事件通知。<br /><br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| type | std::string | 通知类型："event"表示事件,"property"表示属性变更,"result"表示命令执行结果,"online"表示载荷上线,"offline"表示载荷设备离线。|
| message | std::string | 通知详细内容。 |

**返回:**<br />无。


## 类定义说明

<a name="AbcDF"></a>

### ResourceTreeAPI  
```cpp
class ResourceTreeAPI {
public: 
    // 获取单例方法
    static std::shared_ptr<ResourceTreeAPI> GetInstance();
    // 设置micROS进程角色，Reader可以读取所有载荷设备属性,Operator可以读取和操作所有载荷设备。
    int32_t SetUserRole(const std::string micRosId, const std::string role);
    // 删除micROS进程角色。
    int32_t DeleteUserRole(const std::string micRosId);
    // 查询所有micROS进程角色。
    int32_t QueryRoleInfos(std::vector<RoleInfo>& roleInfos);	
	// 查询本micROS进程角色，此接口只供micROS进程调用。
    int32_t QuerySelfRole(std::string& role);
    //添加model。
    int32_t AddModels(std::vector<std::string>& modelIds);
    //删除model。
    int32_t DeleteModel(const std::string modelId);
	//查询所有已添加的模型列表。
    virtual int32_t QueryModelIdList(std::vector<std::string>& modelIds) = 0;
    //添加载荷设备。
    int32_t AddNodes();
    //删除载荷设备。
    int32_t DeleteNodes(const std::vector<std::string> nodeIds);
	//查询所有已添加的载荷设备列表。
    int32_t QueryNodeIdList(std::vector<std::string>& nodeIds);
    //根据modelId查询使用此模型的可用载荷设备列表。
    int32_t QueryAvailableNodesByModelId(const std::string modelId, 
                                         std::vector<std::string>& nodeIds);
	// 根据modelId查询使用此模型的所有载荷设备列表。
    int32_t QueryAllNodesByModelId(const std::string modelId, 
                                   std::vector<std::string>& nodeIds);
    //查询本机信息。
    int32_t GetDeviceInfo(DeviceInfo& deviceInfo); 
    //查询载荷设备信息。
    int32_t GetNodeInfoByNodeId(const std::string nodeId,
                                NodeInfo& nodeInfo);
    //发送命令给载荷设备执行。
    int32_t ExecuteCommand(const std::string NodeId, 
                           const std::string methodName,
                           const std::string input);
    //ResourceTreeAPI注册接口。
    int32_t RegisterListener(const sptr<IResourceTreeAPIListener> listener);
	//订阅接口。
    int32_t Subscribe(std::string nodeId,
                      std::vector<std::string> properties);
    //退订接口。
    int32_t UnSubscribe(std::string nodeId,
                        std::vector<std::string> properties);
    //根据nodeId获取方法名
    int32_t GetFunctionByNodeId(const std::string& nodeId, 
                                std::vector<std::string>& functions);
    //获取nodeId属性名
    int32_t GetPropertyByNodeId(const std::string& nodeId, 
                                std::vector<std::string>& properties);
```
**描述:** 监听接口类。<br /><br />**函数:**

| **名称** | **描述** |
| --- | --- |
| [GetInstance](#AbcDH) | API实例获取接口。 |
| [SetUserRole](#AbcDI) | 设置进程角色，Reader可以读取所有载荷设备属性，Operator可以读取和操作所有载荷设备。本接口只对micROS系统服务开放。 |
| [DeleteUserRole](#AbcDG) | 删除进程角色。本接口只对micROS系统服务开放。 |
| [QueryRoleInfos](#AbcDK) | 查询所有进程角色。本接口只对micROS系统服务开放。 |
| [QuerySelfRole](#AbcDL) | 查询本micROS进程角色。本接口只对micROS进程开放。 |
| [AddModels](#AbcDM) | 添加所有新增模型。本接口只对micROS进程开放。 |
| [DeleteModel](#AbcDN) | 根据模型ID删除模型。本接口只对micROS进程开放。 |
| [QueryModelIdList](#AbcDZ) | 查询所有已添加的模型列表。本接口只对micROS进程开放。 |
| [AddNodes](#AbcDO) | 添加载荷设备。本接口只对micROS进程开放。 |
| [DeleteNodes](#AbcDP) | 删除载荷设备。本接口只对micROS进程开放。 |
| [QueryNodeIdList](#AbcDQ) | 查询所有已添加的载荷设备列表。本接口只对micROS进程开放。 |
| [QueryAvailableNodesByModelId](#AbcDR) | 根据modelId查询可用的载荷设备列表。本接口只对micROS进程开放。 |
| [QueryAllNodesByModelId](#AbcDS) | 根据modelId查询使用此模型的所有载荷设备列表。本接口只对micROS进程开放。 |
| [GetDeviceInfo](#AbcDT) | 查询本机设备属性集合。本接口只对micROS进程开放。 |
| [GetNodeInfoByNodeId](#AbcDU) | 查询载荷设备属性集合。本接口只对micROS进程开放。 |
| [ExecuteCommand](#AbcDV) | 发送命令给载荷设备执行。本接口只对micROS进程开放。 |
| [RegisterListener](#AbcDW) | micROS监听注册接口。本接口只对micROS进程开放。 |
| [Subscribe](#AbcDX) | 资源树订阅接口。本接口只对micROS进程开放。 |
| [UnSubscribe](#AbcDY) | 资源树退订接口。本接口只对micROS进程开放。 |
| [GetFunctionByNodeId](#AbcDZ) | 根据nodeId获取方法名接口。本接口只对micROS进程开放。 |
| [GetPropertyByNodeId](#AbcEA) | 根据nodeId获取属性名接口。本接口只对micROS进程开放。 |

## 函数说明

<a name="AbcDH"></a>

### GetInstance
```cpp
std::shared_ptr<ResoureTreeAPI> GetInstance()
```
**描述:** 获取资源树单例，所有API函数调用通过该单例来调用。<br /><br />**参数:**<br />无<br />**返回:**<br /><br />

| **类型** | **描述** |
| --- | --- |
| std::shared_ptr | 成功时返回ResoureTreeAPI的单例指针，失败时返回NULL。 |

<a name="AbcDI"></a>

### SetUserRole
```cpp
int32_t SetUserRole(const std::string micRosId, const std::string role)
```
**描述:** 设置micROS进程角色，`Reader`可以读取所有载荷设备属性，`Operator`可以读取和操作所有载荷设备。<br /><br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| micRosId | std::string | micROS进程ID。 |
| role | std::string | 进程角色。`Reader`拥有读取所有载荷设备属性的权限，`Operator`拥有读取和操作所有载荷设备的权限，区分大小写。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDJ"></a>

### DeleteUserRole
```cpp
int32_t DeleteUserRole(const std::string micRosId)
```
**描述:** 删除micROS进程角色， 删除后，不允许micROS进程对载荷设备做任何读取和操作<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| micRosId | std::string | micROS进程ID。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDK"></a>

### QueryRoleInfos
```cpp
int32_t QueryRoleInfos(std::vector<RoleInfo>& roleInfos)
```
**描述:** 查询所有micROS进程角色。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| roleInfos | std::vector\<RoleInfo\> | 输出参数，micROS进程角色集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDL"></a>

### QuerySelfRole
```cpp
int32_t QuerySelfRole(std::string role)
```
**描述:** 查询本micROS进程角色。<br /><br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| role | std::string | 输出参数，micROS进程角色集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDM"></a>

### AddModels
```cpp
int32_t AddModels(std::vector<std::string>& modelIds)
```
**描述:** 根据新添加的模型文件，添加所有新增模型 <br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| modelIds | std::vector\<std::string\> | 新增后的模型ID集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDN"></a>

### DeleteModel
```cpp
int32_t DeleteModel(std::string modelId)
```
**描述:** 删除内存中的一个模型, 不会删除模型文件<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| modelId | std::string | 模型的ID。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDZ"></a>

### QueryModelIdList
```cpp
int32_t QueryModelIdList(std::vector<std::string>& modelIds)
```
**描述:** 查询所有已添加的模型列表<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| modelIds | std::vector\<std::string\> | 模型的ID集合 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDO"></a>

### AddNodes
```cpp
int32_t AddNodes()
```
**描述:** 根据micROS进程传入的载荷设备配置文件路径集合，添加配置文件对应的设备。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示命令下发成功返回0，非0 表示命令下发失败， 后续载荷设备添加结果通过回调通知micROS。 |

<a name="AbcDP"></a>

### DeleteNodes
```cpp
int32_t DeleteNodes(const std::vector<std::string> nodeIds);
```
**描述:** 删除多个载荷设备。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| nodeIds | std::vector\<std::string\> | 载荷设备的ID集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示命令下发成功返回0，非0 表示命令下发失败， 后续载荷设备删除结果通过回调通知micROS。 |

<a name="AbcDQ"></a>

### QueryNodeIdList
```cpp
int32_t QueryNodeIdList(std::vector<std::string>& nodeIds);
```
**描述:** 查询所有已添加的载荷设备列表。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| nodeIds | std::vector\<std::string\> | 传出参数，可用的载荷设备ID集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDR"></a>

### QueryAvailableNodesByModelId
```cpp
int32_t QueryAvailableNodesByModelId(const std::string modelId, 
                                     std::vector<std::string>& NodeIds);
```
**描述:** 根据modelId查询使用此模型的可用的载荷设备列表。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| modelId | std::string | 模型ID。 |
| NodeIds | std::vector\<std::string\> | 传出参数，可用的载荷设备ID集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDS"></a>

### QueryAllNodesByModelId
```cpp
int32_t QueryAllNodesByModelId(const std::string modelId, 
                               std::vector<std::string>& nodeIds);
```
**描述:** 根据modelId查询使用此模型的所有载荷设备列表。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| modelId | std::string | 模型ID |
| nodeIds | std::vector\<std::string\> | 传出参数，可用的载荷设备ID集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDT"></a>

### GetDeviceInfo
```cpp
int32_t GetDeviceInfo(DeviceInfo& deviceInfo)
```
**描述:** 查询本机设备信息：cpu，内存，磁盘，网络信息。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| deviceInfo | DeviceInfo | 传出参数，本机设备信息。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDU"></a>

### GetNodeInfoByNodeId
```cpp
int32_t GetNodeInfoByNodeId(const std::string nodeId,
                        NodeInfo& nodeInfo);

```
**描述:** 根据NodeId查询载荷设备属性信息。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| nodeId | std::string | 载荷设备ID。 |
| nodeInfo | NodeInfo | 传出参数，载荷设备信息。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。|

<a name="AbcDV"></a>

### ExecuteCommand
```cpp
int32_t ExecuteCommand(const std::string nodeId, 
                       const std::string methodName,
                       const std::string input);
```
**描述:** 调用某个载荷设备的方法。<br /><br />**参数:**
| **名称** | **类型** | **描述** |
| --- | --- | --- |
| nodeId | std::string | 载荷设备ID。 |
| methodName | std::string | 载荷设备的方法名。 |
| input | std::string | 方法的输入参数。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 负数表示失败。正数表示成功，返回命令序列seqNo，micRos需要保存seqNo，用于后续异步返回命令处理结果时匹配是哪条命令执行结果。 |

<a name="AbcDW"></a>

### RegisterListener
```cpp
int32_t RegisterListener(const sptr<IResourceTreeAPIListener> listener)
```
**描述:** 设置监听者注册接口，监听者需实现OnMessage。后续可以通过Subscribe接口来添加需要订阅的属性 <br /><br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| listener | sptr\<IResourceTreeAPIListener\> | 事件监听者。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败 |

<a name="AbcDX"></a>

### Subscribe
```cpp
int32_t Subscribe(std::string nodeId, std::vector<std::string> properties);
```
**描述:** 订阅载荷设备的属性变更通知 <br /><br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| nodeId | std::string | 载荷设备ID。 |
| properties | std::vector\<std::string\> | 属性名集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDY"></a>

### UnSubscribe
```cpp
int32_t UnSubscribe(std::string nodeId, std::vector<std::string> properties);
```
**描述:** 退订载荷设备的属性变更通知 <br /><br />**参数:**

| **名称** | **类型** | **描述** |
| --- | --- | --- |
| nodeId | std::string | 载荷设备ID。 |
| properties | std::vector\<std::string\> | 属性名集合。 |

**返回:**<br />

| **类型** | **描述** |
| --- | --- |
| int32_t | 0 表示成功，非0 表示失败。 |

<a name="AbcDZ"></a>

### GetFunctionByNodeId

```c++
int32_t GetFunctionByNodeId(const std::string& nodeId, std::vector<std::string>& functions);
```

**描述:** 根据载荷ID获取支持的方法名 <br /><br />

**参数:**

| **名称**  | **类型**                   | **描述**     |
| --------- | -------------------------- | ------------ |
| nodeId    | std::string                | 载荷设备ID。 |
| functions | std::vector\<std::string\> | 方法名集合。 |

**返回:**<br />

| **类型** | **描述**                   |
| -------- | -------------------------- |
| int32_t  | 0 表示成功，非0 表示失败。 |

<a name="AbcEA"></a>

### GetPropertyByNodeId

```c++
int32_t GetPropertyByNodeId(const std::string& nodeId, std::vector<std::string>& properties);
```

**描述:** 根据载荷ID获取属性名 <br /><br />

**参数:**

| **名称**   | **类型**                   | **描述**     |
| ---------- | -------------------------- | ------------ |
| nodeId     | std::string                | 载荷设备ID。 |
| properties | std::vector\<std::string\> | 属性名集合。 |

**返回:**<br />

| **类型** | **描述**                   |
| -------- | -------------------------- |
| int32_t  | 0 表示成功，非0 表示失败。 |