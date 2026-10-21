1、下载代码
// 创建hisi/ohos5目录 注意ohos5是必须的，代码中有该路径代码，hisi路径也建议创建，代码patch会在ohos5的同级目录下创建vendor目录
mkdir -p hisi/ohos5 && cd ohos5
repo init -u ssh://xxxx@gerrit.kaihongdigi.com:29418/Hisilicon_Group/manifests -b Hisilicon2_5.0.3 --no-repo-verify
repo sync -c -j16
repo forall -c "pwd; git lfs pull;";

2、下载组件包并打上patch
// 从svn下载组件包，解压放到OHOS代码目录：ohos5/common_patch/
// svn路径
http://svn.kaihongdigi.com:88/svn/03_项目资料/36_平台项目/202506_超级容器二期技术项目_KH5T20250609016/11_海思SDK/HiOS-platform V200R001C20L37810SPC001T.zip
需要放到common_patch的tar包
R200X_V730R001C10SPC001TB010_Software_Base-package.tar.gz
R200X_V730R001C10SPC001TB010_Software_patch-package.tar.gz

执行patch脚本
cd  ohos5/common_patch
chmod a+x apply_patches_sdk.sh
./apply_patches_sdk.sh

下载工具
cd ohos5
build/prebuilts_download.sh --skip-ssl

3、编译
首次编译需要加patch，之后不需要，注意这里的patch和第二步的patch功能不一样
./build.sh --product-name mp_hi3781v730 --cache --patch

若编译失败，回退patch需要回到第二步重新打patch

后续编译命令
./build.sh --product-name mp_hi3781v730

编译成功后建议使用命令暂存所有修改，避免acs仓打patch失败回退时影响之前的patch
repo forall -c 'pwd;git add .;git commit -m "temp"'

针对patch新增的仓，建议使用git在本地使用git管理
cd ohos5/device;git init;git add .;git commit -m "temp"
cd ohos5/vendor/hisilicon;git init;git add .;git commit -m "temp"
cd hisi/vendor;git init;git add .;git commit -m "temp"

镜像路径：
MP730镜像路径：~/OHOS/ohos5/out/wudangstick/packages/phone/images

4、添加acs编译
由于海思首次编译需要打patch，因此acs仓的patch需要在首次编译完成后才能应用
cd third_party/acs/patches/
./apply-patch.sh -p

回退acs修改
./apply-patch.sh -c

5、修改生成patch
soc、vendor路径代码是通过patch生成的，不能通过git生成patch，修改代码前需要先备份后使用diff生成patch
其它仓建议先确认仓内是否有修改，先commit一次后通过git生成patch
