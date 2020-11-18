# android_device_xiaomi_umi-twrp
For building TWRP for Xiaomi Mi 10

TWRP device tree for Xiaomi Mi 10

Kernel,Dtbo,Dtb均提取至MIUI20.11.7-Android11

## 手机参数

| Device       | Xiaomi Mi 10                           |
| -----------: | :------------------------------------------ |
| SoC          | 高通 骁龙865              |
| 初始版本 | Android10.0                               |


## 特性

**支持**
- 启动
- ADB
- MTP
- 动态分区
- USB-OTG

**不支持**
- 震动
- ADB sideload
- 解密(安卓11不支持)


## 编译

安装环境
```
sudo apt update&&sudo apt install git-core gnupg flex bison gperf zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache 
libgl1-mesa-dev libxml2-utils xsltproc unzip openjdk-8-jdk build-essential git repo fastboot adb
```
配置ccache
```
# 启用ccache
export USE_CCACHE=1
# 改变ccache缓存路径
export CCACHE_DIR=~/.ccache
# 生效
source ~/.bashrc
# 配置ccache大小
ccache -M 50G
```

创建进入Twrp文件夹
```
mkdir -p twrp&&cd twrp
```

同步Twrp的omni最小Tree:
```
repo init -u git://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-10.0
repo sync -j$(nproc --all)
```

添加这个项目到 .repo/manifest.xml:

```xml
<project path="device/xiaomi/umi" name="LittleTurtle2333/android_device_xiaomi_umi-twrp" remote="github" revision="android-11.0" />
```

同步Device Tree:
```
repo sync --force-sync device/xiaomi/umi
```

开始编译:
```
. build/envsetup.sh
lunch omni_umi-eng
mka recoveryimage ALLOW_MISSING_DEPENDENCIES=true # Only if you use minimal twrp tree.
```

临时测试:
```
fastboot boot out/target/product/umi/recovery.img
```

刷入:
```
fastboot flash recovery out/target/product/umi/recovery.img
```
