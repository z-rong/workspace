下载代码

```
git clone https://gitee.com/chenheyun2022/dropwatch.git
```

安装依赖

```
sudo apt-get install autoconf automake libtool
sudo apt-get install libnl-genl-3-dev
sudo apt-get install libpcap-dev
```

使用测试

```
sudo apt install iperf3
```



dropwatch编译

```
./configure --prefix=$(pwd)/install --host=aarch64-linux-gnu \
CPPFLAGS="-I/home/zengrong/kaihong/kit/binutils-2.43/bfd -I/home/zengrong/kaihong/kit/libnl-3.2.25/install/include -I/home/zengrong/kaihong/kit -I/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/include -I/home/zengrong/kaihong/kaihongOS_4.1/third_party/musl/third_party/FreeBSD/sys" \
LDFLAGS="-L/home/zengrong/kaihong/kit/libnl-3.2.25/install/lib -L/home/zengrong/kaihong/kit/libpcap/install/lib -L/home/zengrong/kaihong/kit/readline/install/lib -L/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/lib -L/home/zengrong/kaihong/kit/termcap-1.3/install/lib -ltermcap -L/home/zengrong/kaihong/kaihongOS_4.1/out/arm64/targets/obj/third_party/musl/usr/lib/aarch64-linux-ohos"  \
CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LD=/usr/local/musl-cross/bin/aarch64-linux-musl-ld \
AR=/usr/local/musl-cross/bin/aarch64-linux-musl-ar \
RANLIB=/usr/local/musl-cross/bin/aarch64-linux-musl-ranlib \
NM=/usr/local/musl-cross/bin/aarch64-linux-musl-nm \
OBJDUMP=/usr/local/musl-cross/bin/aarch64-linux-musl-objdump

./configure --prefix=$(pwd)/install --host=aarch64-linux-gnu \
CPPFLAGS="-static -I/home/zengrong/kaihong/kit/binutils-2.43/bfd -I/home/zengrong/kaihong/kit/libnl-3.2.25/install/include -I/home/zengrong/kaihong/kit -I/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/include -I/home/zengrong/kaihong/kaihongOS_4.1/third_party/musl/third_party/FreeBSD/sys -I/home/zengrong/kaihong/kit/binutils-2.43/libiberty -I/home/zengrong/kaihong/kit/libpcap" \
LDFLAGS="-static -L/home/zengrong/kaihong/kit/libnl-3.2.25/install/lib -L/home/zengrong/kaihong/kit/libpcap/install/lib -L/home/zengrong/kaihong/kit/readline/install/lib -L/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/lib -liberty -L/home/zengrong/kaihong/kit/termcap-1.3/install/lib -ltermcap "  \
CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LD=/usr/local/musl-cross/bin/aarch64-linux-musl-ld \
AR=/usr/local/musl-cross/bin/aarch64-linux-musl-ar \
RANLIB=/usr/local/musl-cross/bin/aarch64-linux-musl-ranlib \
NM=/usr/local/musl-cross/bin/aarch64-linux-musl-nm \
OBJDUMP=/usr/local/musl-cross/bin/aarch64-linux-musl-objdump \
--enable-static

./configure --with-pcap=linux --prefix=$(pwd)/install --host=aarch64-linux-gnu LDFLAGS="-static"  CC=aarch64-linux-gnu-gcc 

CPPFLAGS="-I/home/zengrong/kaihong/kit/binutils-2.43/bfd -I/home/zengrong/kaihong/kit/libnl-3.2.25/install/include -I/home/zengrong/kaihong/kit -I/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/include -I/home/zengrong/kaihong/kaihongOS_4.1/third_party/musl/third_party/FreeBSD/sys"
```



```
查看依赖库
ldd /usr/local/bin/dropwatch
patchelf --print-needed /usr/local/bin/dropwatch
```



dropwatch依赖的库如下

```
libnl-genl-3.so.200
libnl-3.so.200
libreadline.so.8
libbfd-2.34-system.so
libc.so.6
```

genl-3移植安装

```
// 先下载源码，通过git或者下载源码包都行
// 配置编译环境
sudo ./configure \
--target=aarch64-linux-gnu \
--host=aarch64-linux-gnu \
--prefix=$(pwd)/install CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LDFLAGS="-L/home/zengrong/kaihong/kaihongOS_4.1/out/arm64/targets/obj/third_party/musl/usr/lib/aarch64-linux-ohos -lc"


LDFLAGS="-static -Wl,--dynamic-linker=/home/zengrong/kaihong/kaihongOS_4.1/out/arm64/targets/common/common/libc/ld-musl-aarch64.so.1" \
TARGET_CC=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc TARGET_OBJCOPY=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-objcopy TARGET_STRIP=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-strip \
TARGET_RANLIB=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ranlib \
TARGET_NM=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-nm \
TARGET_LD=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ld

// 编译
sudo make

// 保存生成的库到指定位置，在configure中的--prefix中指定位置
sudo make install
```

readline移植

```
// 下载源码
git clone https://git.savannah.gnu.org/git/readline.git

// 配置环境
./configure --target=aarch64-linux-gnu --host=aarch64-linux-gnu --prefix=$(pwd)/install CC=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc TARGET_CC=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc TARGET_OBJCOPY=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-objcopy TARGET_STRIP=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-strip TARGET_RANLIB=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ranlib TARGET_NM=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-nm TARGET_LD=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ld

sudo ./configure --target=aarch64-linux-gnu --host=aarch64-linux-gnu --prefix=$(pwd)/install CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc  AR=/usr/local/musl-cross/bin/aarch64-linux-musl-ar RANLIB=/usr/local/musl-cross/bin/aarch64-linux-musl-ranlib LD="--sysroot=/home/zengrong/kaihong/kaihongOS_4.1/out/arm64/targets/obj/third_party/musl"

// 编译
make

// 保存
make install

// 查看文件格式
file install/lib/libreadline.so
```

pcap移植

```
// 下载代码
git clone https://github.com/the-tcpdump-group/libpcap.git
// 配置环境
./autogen.sh
./configure --target=aarch64-linux-gnu --host=aarch64-linux-gnu --prefix=$(pwd)/install \
CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LD=/usr/local/musl-cross/bin/aarch64-linux-musl-ld \
LDFLAGS="-L/home/zengrong/kaihong/kit/libnl-3.2.25/install/lib" \
AR= /usr/local/musl-cross/bin/aarch64-linux-musl-ar



BUILD_CC=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc TARGET_CC=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc TARGET_OBJCOPY=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-objcopy TARGET_STRIP=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-strip TARGET_RANLIB=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ranlib TARGET_NM=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-nm TARGET_LD=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ld CPPFLAGS="-I/home/zengrong/kaihong/kit/libnl-3.2.25/__install/include" LDFLAGS="-L/home/zengrong/kaihong/kit/libnl-3.2.25/__install/lib"

// 编译保存
make
make install
```

bfd移植

```
// 下载代码，这里使用的是2.13版本
https://ftp.gnu.org/gnu/binutils/
// 配置环境
./configure --prefix=$(pwd)/install --host=aarch64-linux-gnu \
CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LD=/usr/local/musl-cross/bin/aarch64-linux-musl-ld \
NM=/usr/local/musl-cross/bin/aarch64-linux-musl-nm \
LDFLAGS="-L/home/zengrong/kaihong/kaihongOS_4.1/out/arm64/targets/obj/third_party/musl/usr/lib/aarch64-linux-ohos -L/home/zengrong/kaihong/kit/binutils-2.43/libsframe/install/lib"

--build=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc CC

    
// 编译保存
make
make install
```

编译dropwatch报错readline缺少reference，确定readline库依赖的 libtermcap库缺少，继续移植termcap库

```
// 下载代码，这里使用的是2.13版本
wget http://ftp.gnu.org/gnu/termcap/termcap-1.3.tar.gz
// 配置环境
./configure --prefix=$(pwd)/install --host=aarch64-linux-gnu --build=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc

    
// 编译保存
make
make install
```

由于生成的termcap库只有静态库，所以在编译dropwatch的地方加上-ltermcap进行链接

编译完成后的依赖库发进去之后，报错

```
/bin/sh: /bin/dropwatch: No such file or directory
```

比较一下正常的可执行程序

```
# file hicar_test
hicar_test: ELF shared object, 64-bit LSB arm64, dynamic (/lib/ld-musl-aarch64.so.1), BuildID=2367fb3b568800e9d2fff6cda729bf25
# file dropwatch
dropwatch: ELF shared object, 64-bit LSB arm64, dynamic (/lib/ld-linux-aarch64.so.1), not stripped
```

可能链接库有区别导致

```
./configure \
--prefix=$(pwd)/install \
--host=aarch64-linux-gnu \
CPPFLAGS="-I/home/zengrong/kaihong/kit -I/home/zengrong/kaihong/kit/libnl-3.2.25/__install/include  -I/home/zengrong/kaihong/kit/readline -I/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/include -I/home/zengrong/kaihong/kit/libpcap/install/include" \
LDFLAGS="-static -L/home/zengrong/kaihong/kit/libnl-3.2.25/__install/lib -L/home/zengrong/kaihong/kit/libpcap/install/lib -L/home/zengrong/kaihong/kit/readline/install/lib -L/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/lib -L/home/zengrong/kaihong/kit/termcap-1.3/install/lib -ltermcap" \
CC=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc \
AR=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ar \
TARGET_OBJCOPY=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-objcopy \
TARGET_STRIP=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-strip \
TARGET_RANLIB=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-ranlib \
TARGET_NM=/home/zengrong/kaihong/kaihongOS_4.1/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-nm

-L/home/zengrong/kaihong/kit/libas


-L/home/zengrong/kaihong/kit/libnl-3.2.25/__install/lib -L/home/zengrong/kaihong/kit/libpcap/install/lib -L/home/zengrong/kaihong/kit/readline/install/lib -L/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/lib -L/home/zengrong/kaihong/kit/termcap-1.3/install/lib -ltermcap

-L/home/zengrong/kaihong/kit/libnl-3.2.25/__install/lib -L/home/zengrong/kaihong/kit/libpcap/install/lib -L/home/zengrong/kaihong/kit/readline/install/lib -L/home/zengrong/kaihong/kit/binutils-2.43/bfd/install/lib -L/home/zengrong/kaihong/kit/termcap-1.3/install/lib -lnl-genl-3 -lnl-3 -lreadline -lbfd
```

libsframe.so.1编译

```
./configure --prefix=$(pwd)/install --host=aarch64-linux-gnu \
CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LD=/usr/local/musl-cross/bin/aarch64-linux-musl-ld \
NM=/usr/local/musl-cross/bin/aarch64-linux-musl-nm \
LDFLAGS="-shared -L/home/zengrong/kaihong/kaihongOS_4.1/out/arm64/targets/obj/third_party/musl/usr/lib/aarch64-linux-ohos"
```

libiberty编译

```
./configure --prefix=$(pwd)/install --host=aarch64-linux-gnu \
CC=/usr/local/musl-cross/bin/aarch64-linux-musl-gcc \
LD=/usr/local/musl-cross/bin/aarch64-linux-musl-ld \
NM=/usr/local/musl-cross/bin/aarch64-linux-musl-nm
```

