## ffmpegp交叉编译mac
本编译使用mac +[ NDK-r14(linux 上 r10 亲测可行)](https://developer.android.google.cn/ndk/downloads/) + [ffmpegp-2.6.9](http://ffmpeg.org/download.html)

### 步骤
&nbsp;    1.配置好NDK环境，下载ffmpegp源码
&nbsp;    2.修改configure文件，不要将版本号置为.so后面，大概在2779行位置
```java
SLIBNAME_WITH_MAJOR='$(SLIBNAME).$(LIBMAJOR)'
LIB_INSTALL_EXTRA_CMD='$$(RANLIB) "$(LIBDIR)/$(LIBNAME)"'
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_VERSION)'
SLIB_INSTALL_LINKS='$(SLIBNAME_WITH_MAJOR) $(SLIBNAME)'
改为
SLIBNAME_WITH_MAJOR='$(SLIBPREF)$(FULLNAME)-$(LIBMAJOR)$(SLIBSUF)'
LIB_INSTALL_EXTRA_CMD='$$(RANLIB)"$(LIBDIR)/$(LIBNAME)"'
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_MAJOR)'
SLIB_INSTALL_LINKS='$(SLIBNAME)'
```
&nbsp;    3.新建shell脚本，执行configure脚本
```shell
#!/bin/bash
make clean
export NDK=/Users/yangyangyang/Downloads/android-ndk-r14b #修改自己的路径
export SYSROOT=$NDK/platforms/android-19/arch-arm/
export TOOLCHAIN=$NDK/toolchains/arm-linux-androideabi-4.9/prebuilt/darwin-x86_64 #低版本请用4.8  darwin改为linux
export CPU=arm #根据需求改平台
export PREFIX=$(pwd)/android/$CPU #生成的目录 为脚本目录下的 android目录里面
export ADDI_CFLAGS="-marm"

# 根据需求修改configure脚本参数
./configure --target-os=linux \
--prefix=$PREFIX --arch=arm \
--disable-doc \
--enable-shared \
--disable-static \
--disable-yasm \
--disable-symver \
--enable-gpl \
--disable-ffmpeg \
--disable-ffplay \
--disable-ffprobe \
--disable-ffserver \
--disable-doc \
--disable-symver \
--cross-prefix=$TOOLCHAIN/bin/arm-linux-androideabi- \
--enable-cross-compile \
--sysroot=$SYSROOT \
--extra-cflags="-Os -fpic $ADDI_CFLAGS" \
--extra-ldflags="$ADDI_LDFLAGS" \
$ADDITIONAL_CONFIGURE_FLAG
make clean
make -j12 #根据自己的电脑配置来 我这里是6核12线程
make install

```

&nbsp;    3.执行shell脚本（注意给shell脚本权限chmod 777 XXX.sh）

## ffmpegp各个模块


## 最后
如果只需要so或没有编译出so可以直接时候这里编译好的so<br/>
位置：[ffmpegp-2.6.9/android/arm ](https://github.com/TF27674569/ffmpegp/tree/master/ffmpeg-2.6.9/android/arm) <br/>
使用so：[使用ffmpegp解码](https://github.com/TF27674569/FFmpegpDecode) <br/>


