## 在deveco中调用C API方法

1、创建一个openharmony空项目

![image-20250220200724383](images/image-20250220200724383.png)

2、在pages目录中选择创建C++ Napi文件，deveco会自动生成一个cpp目录，并附带一个简单的add接口示例

![image-20250220201116037](images/image-20250220201116037.png)

![image-20250220201241537](images/image-20250220201241537.png)

3、add接口可以通过import动态库进行导入使用，动态库名称和js函数名在oh_modules目录中声明，add接口使用方法和正常的api接口一样

![](images/image-20250220201535474.png)

![image-20250220201701765](images/image-20250220201701765.png)

4、编译的时候需要在项目路径的build-profile.json5文件中添加abiFilters

![image-20250220201901611](images/image-20250220201901611.png)

5、在完成签名之后就能在ets文件中正常使用add接口了

![image-20250220202330469](images/image-20250220202330469.png)

## 使用系统的C API

1、使用系统C API需要在上面的基础上进行修改，首先需要增加cmakelist连接的库，这个一般在系统C API使用说明中会声明需要哪些库。以屏幕录制的C接口为例，需要新增libnative_avscreen_capture.so libnative_buffer.so libnative_media_core.so等库

![image-20250220203306933](images/image-20250220203306933.png)

2、在hello.cpp中按照napi的编写方式声明自己新建的js接口

![image-20250220203459063](images/image-20250220203459063.png)

3、之后就是按照napi的编写方法调用系统C接口，这里如果仅仅是简单使用，也可以简略这些流程，直接在napi声明的函数中直接以C风格编写代码，只需要注意参数和返回值需要以napi的方式进行使用即可

![image-20250220203809383](images/image-20250220203809383.png)

![image-20250220204722813](images/image-20250220204722813.png)