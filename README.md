# MYC-YT113i BUILDROOT INSTALLATION
This document has been prepared based on MYIR’s official documentation for the T113-i platform. \
MYIR: https://en.myir.cn/T113/76.html \
For Files: support@demsay.com 

## 🧰 _PREREQUISITES_

### 🛠️ Install Required Packages:
```sh
PC$:sudo apt-get update
```
```sh
PC$:sudo apt install -y git gnupg flex bison gperf build-essential zip curl \
libc6-dev libncurses5-dev:i386 x11proto-core-dev libx11-dev:i386 \
libreadline6-dev:i386 libgl1-mesa-glx:i386 libgl1-mesa-dev g++-multilib \
tofrodos python markdown libxml2-utils xsltproc zlib1g-dev:i386 gawk texinfo \
gettext gcc libncurses5-dev zlib1g-dev libssl-dev autoconf libtool \
linux-libc-dev:i386 wget patch dos2unix tree u-boot-tools
```
### 🛠️ Other non-required configuration packages
```sh
PC$:sudo dpkg-reconfigure dash #dash configuration
PC$:sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so
PC$:sudo apt-get install zlib1g-dev #Install if libz.so is missing
PC$:sudo apt-get install uboot-mkimage #Install or install u-boot-tools when mk imge is missing
```
## 🖥️ _Install cross compilation tool chain_
🔹Copy the SDK archive to the user working directory under Ubuntu, such as
“$HOME/T113X”, which is defined according to your actual situation, and then
unzip the file to get the SDK source code file as follows:

```sh
PC$: cd $HOME/T113X
PC$: tar -jxf YT113X-buildroot-t1-5.4.61-1.0.0.tar.bz2
```
### 📂 View compiled chain file
🔹Go to the SDK directory and you can find it under the “build/toolchain” directory:
```sh
PC$: cd $HOME/T113X/T113Xauto-t113x-linux/build/toolchain
PC$: $HOME/T113X/T113Xauto-t113x-linux/build/toolchain$ ls
```
```
├── gcc-linaro-5.3.1-2016.05-x86_64_aarch64-linux-gnu.tar.xz
├── gcc-linaro-7.4.1-2019.02-x86_64_aarch64-linux-gnu.tar.xz
├── gcc-linaro-arm.tar.xz
├── **gcc-linaro-5.3.1-2016.05-x86_64_arm-linux-gnueabi.tar.xz**
├── gcc-linaro-aarch64.tar.xz
├── gcc-linaro.tar.bz2
```
### 📂 Extracting compilation chain files
🔹Extract to the host's “/opt” directory, or you can choose your own directory if prompted
```sh
PC$: $HOME/T113X/T113Xauto-t113x-linux/build/toolchain
	tar -xf gcc-linaro-5.3.1-2016.05-x86_64_arm-linux-gnueabi.tar.xz -C /opt
```
### ✅ Installing and testing the compilation chain
```sh
PC$: export PATH=$PATH:/opt/gcc-linaro-5.3.1-2016.05-x86_64_arm-linux-gnueabi/bin
PC$: arm-linux-gnueabi-gcc -v
```
## 💾 _Build Development Board İmage Using SDK_
🔹The 04_sources directory in the CD image provided by MYIR provides linux SDK
files and data for the "MYD-YT113X" development board to help developers build
different types of Linux system images that can be run on the "MYD-YT113X"
development board, as shown below.
| Image Files Name | Content Description |
| ------ | ------ |
| myir-image-yt113i-emmc-core| Building an image without QT GUI with buildroot|
| myir-image-yt113i-emmc-full | Build image with QT GUI, 7" LVDS display with buildroot|

🔹The configs directory holds the predefined configuration files, as follows:
| Configuration File | Description|
| ------ | ------ |
| myd_yt113i_emmc_core_br_defconfig| emmc core system buildroot configuration|
| myd_yt113i_emmc_full_br_defconfig | emmc full system buildroot configuration|

### 📝 MYC-YT113-I Model Configuration Explained
```
device/config/chips/t113_i
├── bin
│ ├── boot0_nand_sun8iw20p1.bin boot0 boot file for nand
│ ├── boot0_sdcard_sun8iw20p1.bin boot0 boot file for emmc
│ ├── boot0_spinor_sun8iw20p1.bin
│ ├── dsp0.bin
│ ├── fes1_sun8iw20p1.bin Initialization file for burn tool
│ ├── optee_sun8iw20p1.bin optee
│ ├── sboot_sun8iw20p1.bin Safe start of bin
│ ├── u-boot-spinor-sun8iw20p1.bin
│ └── u-boot-sun8iw20p1.bin
├── boot-resource
│ ├── boot-resource
│ │ ├── bat
│ │ ├── bootlogo.bmp
│ │ ├── fastbootlogo.bmp
│ │ ├── font24.sft
│ │ ├── font32.sft
│ │ └── wavefile
│ └── boot-resource.ini
├── configs
│ ├── default Not valid under normal conditions
│ └── **myir-image-yt113i-full EMMC**
│ ├── board.dts EMMC type dts configuration
│ ├── bsp │ ├── linux-5.4 │ ├── longan
│ │ │ ├── BoardConfig.mk
Kernel, buildroot, toolchain and other
configurations
│ │ │ ├── BoardConfig_nor.mk
│ │ │ ├── bootlogo.bmp
EMMC board type bootlog picture
│ │ │ ├── env_ab.cfg
│ │ │ ├── env.cfg
│ │ │ ├── env_nor.cfg
│ │ │ ├── sys_partition_ab.fex
│ │ │ ├── sys_partition.fex EMMC board type default partition file
│ │ ├── sys_config.fex EMMC board type sys_config configuration
│ │ └── uboot-board.dts EMMC board type uboot using the dts file
```
### 🐧 Linux SDK Configuration and Build
🔹This section explains how to generate "myir-image-yt113i-full.img" images by performing the following steps
🔹First go to the source top-level directory: "T113Xauto-t113x-linux", then execute the following command.
```sh
PC$: cd $HOME/T113X/T113Xauto-t113x-linux
```
### 1). Step 1 “./build.sh config”
🔹Execute build.sh config and select the configuration in the subsequent dialog.
Enter the string corresponding to the number.
```sh
PC$: $HOME/T113X/T113Xauto-t113x-linux$ ./build.sh config
```
**MYD-YT113-I Model Configuration Selection**
PC$ ./build.sh config
Welcome to mkscript setup progress
All available platform:
0. linux
**Choice [linux]: 0**

All available linux_dev:
0. bsp
1. dragonboard
2. longan
3. tinyos
**Choice [longan]: 2**

All available kern_ver:
0. linux-5.4
**Choice [linux-5.4]: 0**

All available ic:
0. t113
1. t113_i
**Choice [t113_i]: 1**

All available board:
0. myir-image-yt113i-core
1. myir-image-yt113i-full
**Choice [myir-image-yt113i-full]: 1**

All available flash:
0. default
1. nor
**Choice [default]: 0**

All available gnueabi:
0. gnueabi
1. gnueabihf
**Choice [gnueabi]: 0**

If after executing “. /build.sh config”, the following error message appears

File "<string>", line 1
import os.path; print os.path.relpath('/home/XXX/T113X/auto-t113x-linux/kern
Notes
el/linux-5.4/arch/arm/configs/sun8iw20p1smp_t113_auto_defconfig', '/home/XXX
/T113X/auto-t113x-linux/kernel/linux-5.4/arch/arm/configs')

**SyntaxError: invalid syntax
ERROR: Can't find kernel defconfig!**

❗️You need to edit your Python version
```sh
PC$: sudo apt-get install python
```
Check if the current version is python2
```sh
PC$ python --version
Python 2.7.18
```
❗️If Python2 is installed but the current Python version is different,it is necessary to change this
```sh
PC$: sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2
PC$: sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 1
PC$: sudo update-alternatives --config python
```
-----------------------
 0 /usr/bin/python2.7 2
*1 /usr/bin/python2.7 2
 2 /usr/bin/python3.6 1
-----------------------
### 2). Step 2 “./build.sh”
```sh
PC$: cd $HOME/T113X/T113Xauto-t113x-linux
PC$: $HOME/T113X/T113Xauto-t113x-linux$ ./build.sh
```
When the installation is completed successfully, you will get an output like the following
```
ACTION List: mklichee;========
Execute command: mklichee
INFO: ----------------------------------------
INFO: build lichee ...
INFO: chip: sun8iw20p1
INFO: platform: linux
INFO: kernel: linux-5.4
INFO: board: emmc_full
INFO: output: /home/koi/T113X/T113Xauto-t113x-linux/out/t113/myir-image-yt1
13s3-emmc-full/longan
INFO: ----------------------------------------
INFO: build buildroot ...
.......
INFO: pack rootfs ok ...
INFO: ----------------------------------------
INFO: build lichee OK.
INFO: ----------------------------------------
```
❗️The following error may be encountered when performing this step

gdbusconnection.c: In function 'check_initialized':
gdbusconnection.c:567:8: warning: unused variable 'flags' [-Wunused-variable]
  567 |     gint flags = g_atomic_int_get (&connection->atomic_flags);
      |         ^~~~~
gdbusmessage.c: In function 'parse_value_from_blob':
gdbusmessage.c:1712:29: warning: variable 'item' set but not used [-Wunused-but-set-variable]
  1712 |     GVariant *item;
       |             ^~~~
gdbusmessage.c: In function 'append_value_to_blob':
gdbusmessage.c:2326:24: warning: unused variable 'end' [-Wunused-variable]
  2326 |     const gchar *end;
       |                        ^~~
gdbusauth.c: In function '_g_dbus_auth_run_server':
gdbusauth.c:1302:11: **error:** '%s' directive argument is null **[-Werror=format-overflow=]**
  1302 |     **debug_print ("SERVER: WaitingForBegin, read '%s'", line);**
       |           ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  CC       libgio_2_0_la-gdbusinterface.lo
cc1: some warnings being treated as errors
Makefile:3633: recipe for target 'libgio_2_0_la-gdbusauth.lo' failed
make[5]: *** [libgio_2_0_la-gdbusauth.lo] Error 1
make[5]: *** Waiting for unfinished jobs....
gdbusmessage.c: In function 'g_dbus_message_to_blob':
gdbusmessage.c:2702:30: **error:** '%s' directive argument is null **[-Werror=format-overflow=]**
  2702 |     tupled_signature_str = **g_strdup_printf ("%s", signature_str);**
       |                              ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
gdbusintrospection.c: In function 'g_dbus_interface_info_generate_xml':
gdbusintrospection.c:751:3: warning: 'access_string' may be used uninitialized in this function [-Wmaybe-uninitialized]


**Modify step 1:Enter the "gdbusauth.c" code file**
```sh
PC$: $HOME/T113X/T113Xauto-t113x-linux$ vim ./out/t113_i/myir-image-yt113i-full/longan/buildroot/build/host-libglib2-2.56.3/gio/gdbusauth.c
```
Add this judgment code in the following position:
```
line = _my_g_input_stream_read_line_safe (g_io_stream_get_input_stream (auth->
priv->stream),
&line_length,
cancellable,
error);
**if (line != NULL)**
   debug_print ("SERVER: WaitingForBegin, read '%s'", line);
if (line == NULL)
```
**Modify step 2: Enter the "gdbusmessage.c" code file**
```sh
PC$: $HOME/T113X/T113Xauto-t113x-linux$ vim ./out/t113_i/myir-image-yt113i-full/longan/buildroot/build/host-libglib2-2.56.3/gio/gdbusmessage.c
```
Add this judgment code in the following position:
```
signature_str = g_variant_get_string (signature, NULL);
if (message->body != NULL)
{
    gchar *tupled_signature_str;
**if (signature != NULL)**
      tupled_signature_str = g_strdup_printf ("(%s)", signature_str);
    if (signature == NULL)
```
❗️If you encounter an error like the following:
```
make[4]: *** [Makefile:1615: errnos.sym.h] Error 2
make[4]: *** Waiting for unfinished jobs....
gawk: ./mkerrcodes.awk:88: warning: regexp escape sequence `\#' is not a known regexp operator
rm mkerrcodes.h
make[3]: *** [Makefile:508: all-recursive] Error 1
make[2]: *** [Makefile:440: all] Error 2
make[1]: *** [package/pkg-generic.mk:241: /home/cat/T113/auto-t113-linux/out/t113/evb1_auto/longan/buildroot/build/libgpg-error-1.33/.stamp_built] Error 2
make: *** [Makefile:84: _all] Error 2
make: Leaving directory '/home/cat/T113/auto-t113-linux/buildroot/buildroot-201902'
```
ERROR: build buildroot Failed

**follow my current step**
```sh
PC$ cd $HOME/T113X/T113Xauto-t113x-linux/out/t113_i/myir-image-yt113i-full/longan/buildroot/build/libgpg-error-1.33/src
```
🔹Change "namespace" to "pkg_namespace" in "Makefile", "Makefile.am",
"Makefile.in", and "mkstrtable.awk" in this directory, and then re-execute the
compile command.

### 3) Qt Compilation 
🔹If you don't need the qt function, you can skip this step and execute the following
command to compile qt:
```sh
PC$: $HOME/T113X/T113Xauto-t113x-linux$ ./build.sh qt
ACTION List: mkqt;========
Execute command: mkqt
INFO: build Qt ...
INFO: build arm-linux-gnueabi version's Qt
$HOME/T113X/T113Xauto-t113x-linux/platform/framework/qt/qt-everywhere-src
-5.12.5
.......
INFO: build buildroot OK.
INFO: build Qt and buildroot Ok.
```
At this point, qt is compiled successfully, and then you must execute **". /build.sh"**
command, this command will just compile qt generated by the relevant files,

### 4) “./build.sh pack”
The last step is to package the image to generate the system by executing the
following command:
```sh
PC$: $HOME/T113X/T113Xauto-t113x-linux$ ./build.sh pack
ACTION List: mkpack ;========
Execute command: mkpack
INFO: packing firmware ...
INFO: Use BIN_PATH: $HOME/T113X/T113Xauto-t113x-linux/device/config/chips/
t113/bin
......
Dragon execute image.cfg SUCCESS !
----------image is at----------
size:456M $HOME/T113X/T113Xauto-t113x-linux/out/myir-image-yt113s3-emmc
-full.img
```


