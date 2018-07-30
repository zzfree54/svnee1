
------
# desknote-atc-zz2017
tags: desknote.zz2017
time: 2018-5-7

-----

### REF:

    1700010000009449
    D:\programs\cygwin32\bin\bash --login -i
    ID: \w{2}A48A02 DATA: (\w{2} ){1}


### Write:

    zhongzhun @ shenzhen
    zz @ 2017-11-27
    zz @ 2018-1-15
    zz @ 2018-2-9 10:53:36
    zz @ 2018-2-26 14:36:42
    zz @ 2018-3-29
    zz @ 2018-4-17

*********
## 38 python beautifulsoup 玩转

#### REF
```

利用Python读取txt文档的方法
https://blog.csdn.net/supergiser_lee/article/details/60571983

Beautiful Soup 4.2.0 文档
https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html
```

```
from __future__ import print_function
import sys

f = open("f:\\html_doc.txt")
lines = f.readlines()

print(lines)

html_doc = lines

print(html_doc)

```

#### 安装 BeautifulSoup
```
D:\programs\Python27\Scripts>
    pip install bs4
```

#### 使用 BeautifulSoup
```
from bs4 import BeautifulSoup

soup1 = BeautifulSoup(html_doc)
    出错啦，编码问题？ ANSI全英文字符也不行么？

soup2 = BeautifulSoup(open("f:\\html_doc.txt"))
    可以成功
```

```
soup = BeautifulSoup(open("f:\\html_doc.txt"))

print(soup.prettify())

soup.title
soup.title.name

soup.p
soup.a

soup.find_all('a')

soup.find(id="link3")

print(soup.get_text())

```


*********
## 37 包含且不包含abc ((?!abc).)*

```
> REF:
    正则表达式匹配:包含且不包含
    https://blog.csdn.net/thewindkee/article/details/52785763

//zz// 包含且不包含abc ((?!abc).)*
//zz// ^((?!abc).)*$
```

测试用
```
myabcasdlkfjalskdf
xxxabcl;skf;lasd;f*g*a*f*s*d*g*

klflasdfadsfasf
asdklgasdf;gasf
```

*********
## 36 正则表达式,去掉C语言注释

#### REF:
```
    批量去掉 C++/C 中的注释
    http://www.cnblogs.com/gaoduan/p/4139847.html

    正则表达式规则以及贪婪匹配与非贪婪匹配
    https://blog.csdn.net/chenlycly/article/details/54982537
```

```
//zz// (/\*)(.*)(\*/) 贪婪模式 .* .+
//zz// (/\*)(.*?)(\*/) 非贪婪模式 .*? .+?
	/* /* adsfa */ */

//zz.fail// (/\*)([.|\n]*?)(\*/)
//zz.succ// (/\*)((.|\n)*?)(\*/)
    非贪婪模式匹配 /* */ 内容
    不知道为啥 [.\n]* 不能用,要用 (.|\n)*
//zz.succ// (/\*)(((?!/\*)(?!\*/).|\n)*)(\*/)
    (((?!/\*)(?!\*/).|\n)*)
    缺省贪婪模式 + 不包含实现
    包含且不包含模式, 中间不包含 "/*" 或 "*/"
    然后还是用 .|\n 来匹配

```

几个测试用的注释
```
/* adsfa 
*/

/*
abc/defghijklmn
afdgadsf
*/

/*
* asdkflj;a
*asdfads hgadf
*/


/**
*a*s*d*f*g*a*f*s*d*g*
*/
```

*********
## 35 opencv 图像处理,人脸识别,车牌识别

#### REF:
移植opencv到嵌入式arm详细过程
https://blog.csdn.net/guet_kite/article/details/78667175

#### 1) 编译环境搭建
```
cmake-gui --version
apt-get install cmake-qt-gui

https://github.com/opencv/opencv
https://github.com/opencv/opencv.git
https://codeload.github.com/opencv/opencv/zip/3.3.0
```

#### 2) PC上虚拟机x86玩转
```
sudo apt-get install libopencv-dev python-opencv
apt-get install libopencv-dev
apt-get install python-opencv
```

编译,库so的调用,参考如下
https://blog.csdn.net/li_wen01/article/details/71641408
https://blog.csdn.net/guet_kite/article/details/78667175

```
g++ -lcv -lcxcore -lhighgui -lpthread -lrt -o test-opcv test-opcv.c
    会提示没有 cv cxcore highgui库

g++  -o test-opcv test-opcv.c `pkg-config --cflags --libs opencv`
    echo `pkg-config --cflags --libs opencv`
    这种方式怎么用在 arm-linux 上

g++ -Wno-psabi -I/usr/include/opencv/ -L/usr/lib/i386-linux-gnu/ -lopencv_core -lopencv_highgui -lpthread -lrt -o test-opcv test-opcv.c
    需要自己找到各种库,没有pkg-config方便

```

*********
## 34 python图片识别 tesseract编译

```
git clone https://github.com/tesseract-ocr/tesseract tesseract
//zz// git clone https://github.com/tesseract-ocr/tesseract
//zz// git clone https://github.com/zzfree54/tesseract.git

./autogen.sh
    Missing autoconf-archive. Check the build requirements.

apt-get install autoconf-archive
./configure --prefix=$(pwd)/_install
    error: Leptonica 1.74 or higher is required. Try to install libleptonica-dev package.

apt-get install libleptonica-dev

git checkout 3.05
```

#### 2) 下载包到本地编译
apt-get install autoconf-archive

http://www.leptonica.com/download.html?=183945625
```
tar zxvf leptonica-1.74.4.tar.gz
cd leptonica-1.74.4/
./configure --prefix=$(pwd)/_install
make
make install
cd _install/
cp * -ar /usr/local/

cat /etc/ld.so.conf
ldconfig

```

https://github.com/tesseract-ocr/tesseract
https://github.com/tesseract-ocr/tesseract.git
https://codeload.github.com/zzfree54/tesseract/zip/3.05
```
unzip tesseract-3.05.zip
cd tesseract-3.05/

./autogen.sh
./configure --prefix=$(pwd)/_install

make
make install

cd _install/
cp * -ar /usr/local/
```
#### 3) tesseract数据等环境的搭建及使用
```
下载官网的训练数据
https://github.com/tesseract-ocr/tesseract/wiki/Data-Files#data-files-for-version-304305

==>
https://raw.githubusercontent.com/tesseract-ocr/tessdata/3.04.00/chi_sim.traineddata
https://raw.githubusercontent.com/tesseract-ocr/tessdata/3.04.00/eng.traineddata

#### 拷贝学习数据到 share/tessdata/
cp eng.traineddata chi_sim.traineddata /workdisk/python-ocr/tesseract-3.05/_install/share/tessdata/
cp eng.traineddata chi_sim.traineddata /usr/local/share/tessdata/

#### 执行图片识别
tesseract zzocr-english.png result -l eng

tesseract cn-denggao.bmp cn-denggao -l chi_sim
    可以输出中文,但是效果不好

#### 错误如下
Tesseract Open Source OCR Engine v3.05.01 with Leptonica
Error in pixReadStreamPng: function not present
Error in pixReadStream: png: no pix returned
Error in pixRead: pix not read
Error during processing.

CR使用Tesseract 命令的时候出现上面异常
http://www.myexception.cn/c/1174316.html
    需要将图片更改为单色图
    例如用 mspaint 修改后保存为单色图.bmp
    => 不对,测试后发现支持 bmp 24bit 16bit 单色
    => 对 jpeg png tif 不支持,应该是没有安装相关的库

```

#### 4) 图片格式的支持
apt-get install zlib-dev

apt-get install libjpeg-dev
apt-get install libtiff-dev
apt-get install libpng-dev


*********
## 33 uboot 两个阶段 SPL/MLO 及 u-boot.img

### 1) uboot 两个阶段 MLO 及 u-boot.img 的编译文件及执行过程

```
scripts/Makefile.spl:24:CONFIG_SPL_BUILD := y

include/configs/ti_armv7_common.h:202:#define CONFIG_SPL_FRAMEWORK

scripts/Makefile.spl:94:libs-$(CONFIG_SPL_FRAMEWORK) += common/spl/

include/configs/ti_armv7_common.h:23:#define CONFIG_SYS_GENERIC_BOARD

```

include/configs/ti_am335x_common.h:75:#define CONFIG_SKIP_LOWLEVEL_INIT

board_init_f() 函数两种,SPL及img
arch/arm/lib/spl.o
    CONFIG_SPL_BUILD
    CONFIG_SPL_FRAMEWORK

arch/arm/cpu/armv7/am33xx/board.c

#### arch/arm/lib/Makefile 对于SPL及img的区别
```
ifndef CONFIG_SPL_BUILD
    // ndef CONFIG_SYS_GENERIC_BOARD
    obj-y	+= board.o
else
    obj-$(CONFIG_SPL_FRAMEWORK) += spl.o
endif
```

### 2) 添加头文件系统目录,可直接包含无需加相对路径
/Makefile -Iinclude
    include/rtl8367/ 目录有头文件,需要作为顶层 include 头文件目录,则需要添加 -Iinclude/rtl8367/

```
/Makefile
#zz# eth phy smi init, switch rtl8367, add -Iinclude/rtl8367
UBOOTINCLUDE    := \
		-Iinclude \
		-Iinclude/rtl8367 \
```

### 3) 添加源码文件
drivers/rtl8367

```
注意:
    drivers/rtl8367/ 中目标文件,是2选1种方法
    在 / 或 /drivers 目录中都可以,但不能都加
/Makefile
#zz# rtl8367, /Makefile or /drivers/Makefile
# libs-y += drivers/rtl8367/

drivers/Makefile
#zz# phy smi init, switch rtl8367
#zz# rtl8367, /Makefile or /drivers/Makefile
obj-y += rtl8367/

drivers/rtl8367/Makefile
obj-y	+= rtk_api.o
obj-y	+= rtl8370_asicdrv_acl.o
obj-y	+= rtl8370_asicdrv.o
...

```

*********
## 32 okmx6ulc 烧录sd制作及uboot启动 boot.scr

烧录 启动用的 uboot 到 sd 卡中
dd if=/dev/zero of=/dev/mmcblk0 bs=1k seek=1 count=1024 conv=fsync
dd if=u-boot.imx of=/dev/mmcblk0 bs=1k seek=1 conv=fsync

清除 emmc 中 uboot-env
dd if=/dev/zero of=/dev/mmcblk1 bs=1k seek=384 conv=fsync count=129

将烧录sd卡中的uboot烧到emmc中,只烧录uboot其他不动
cd /media/mmcblk0p1
dd if=system/u-boot.imx of=/dev/mmcblk1 bs=1k seek=1 conv=fsync

*********
## 31 okmx6ulc3-atcxjf dtb 驱动调试
```
echo 7 > /sys/devices/soc0/backlight.8/backlight/backlight.8/brightness
    背光要设置为7,不然不亮

/etc/rc.d/qt_env.sh
QWS_MOUSE_PROTO=tslib:/dev/input/event1
    修改为 event2
    原因,多了一个tsc输入设备

```

*********
## 30 shell批量重命名

### REF:
```
    rename命令和批量重命名
    http://blog.csdn.net/ggxiaobai/article/details/53507454

    Shell脚本批量重命名文件后缀的3种实现
    http://www.jb51.net/article/55255.htm

```

```
rename -f 's/\.hcc/\.h/' *.hcc
rename -f 's/\.ccc$/\.c/' *.ccc

```

目录递归for方法
```
for fn in $(find ./ -iname "*.ccc")
do
	# echo $fn;
	# echo $(echo $fn | sed 's/\.ccc/\.c/');
	echo "do copy/move $fn $(echo $fn | sed 's/\.ccc/\.c/')"
	# mv $fn $(echo $fn | sed 's/\.ccc/\.c/');
	cp $fn $(echo $fn | sed 's/\.ccc/\.c/');
done;

for fn in $(find ./ -iname "*.hcc")
do
	# echo $fn;
	# echo $(echo $fn | sed 's/\.hcc/\.h/');
	echo "do copy/move $fn $(echo $fn | sed 's/\.hcc/\.h/')"
	# mv $fn $(echo $fn | sed 's/\.hcc/\.h/');
	cp $fn $(echo $fn | sed 's/\.hcc/\.h/');
done;

```

*********
## 29 dtc 分析 iomxc 驱动及 gpio-user

dtb 反汇编

./dtc -I dtb -O dts imx6ul-14x14-evk-iomuxc.dtb >  atc-xjf-imx6ul-14x14-iomuxc.dts

of_get_named_gpio_flags()
    "gpios"

### 1) gpio-user.c 自己写驱动与 dts 中的 gpios 节点

### REF:
```
    gpio-user.c
    ul添加gpio的方法.doc / ul添加gpio的方法.pdf
    
    我眼中的Linux设备树(三 属性)
    http://www.cnblogs.com/targethero/p/5072812.html

    linux 设备树dts 相关of_ API实验总结
    http://lxiaogao.lofter.com/post/1cc6a101_4f92a29?=3380954356?=3380954356
```

驱动采用 platform_driver 方式开始注册
匹配 "gpio-user"

of_property_read_u32()
    include/linux/of.h
    drivers/of/base.c => of_property_read_u32_array()
        => of_find_property_value_of_size()

of_get_property()
    获取字符串值
    drivers/of/base.c

of_get_gpio_flags()
    特殊的 gpios 获取 int值得方式??

```
gpio-user.c
    gpio_user_probe()
    .name = "gpio-user"
    .of_match_table
        .compatible = "gpio-user"

gpio_user_probe(pdev)
    struct device_node *node = pdev->dev.of_node;
    gpio_misc->gpio_count = of_get_available_child_count(node);

    for_each_available_child_of_node(node,child)
        of_get_property(child,"label",NULL);
        of_get_property(child,"default-direction",NULL);

        of_get_gpio_flags(child,0,&data->dft);
```

对每个子节点 child 的 dtb 数据的解析
关键的键值 "gpios"
```
arch/arm/mach-imx/of_gpio.h
of_get_gpio_flags()
of_get_gpio_flags(child,0,&data->dft);

    ==>
    CONFIG_OF_GPIO
    of_get_named_gpio_flags(np, "gpios", index, flags);
    
    ==>
    //zz// "gpios"
    desc = of_get_named_gpiod_flags(np, list_name, index, flags);
        //zz// drivers/gpio/gpiolib-of.c

    desc_to_gpio(desc);
         //zz// drivers/gpio/gpiolib.c

```

### 2) iomuxc 标准的gpio方式

应用层访问,系统中通过访问目录
```
/sys/class/gpio/
    export / unexport
        通过 echo 引脚号进行导出

    ${(n-1)*32 + x}/direction
    ${(n-1)*32 + x}/value
        对导出的引脚,通过direction设置输入输出,通过value设置/读取值

```

对 dts 设备树的信息
arch/arm/boot/dts/imx6ul-14x14-evk-7-r.dts
    #include "imx6ul.dtsi"
    匹配字符串 "fsl,imx6ul-iomuxc"

```

iomuxc: iomuxc@020e0000
{
    compatible = "fsl,imx6ul-iomuxc";
    reg = <0x020e0000 0x4000>;
};

搜索关键字
grep -Inr "fsl,imx6ul-iomuxc" ./ | grep -v "\.dts"
    arch/arm/mach-imx/mach-imx6ul.c
    arch/arm/mach-imx/pm-imx6.c
    drivers/pinctrl/pinctrl-imx6ul.c

```

drivers/pinctrl/pinctrl-imx6ul.c
"fsl,imx6ul-iomuxc"

```
//zz// 重要的数据结构
imx6ul_pinctrl_driver

imx6ul_pinctrl_info
    此结构中包含了所有的引脚gpio
    imx6ul_pinctrl_pads

//zz// 重要的函数
imx6ul_pinctrl_init()
    imx6ul_pinctrl_driver()

imx6ul_pinctrl_probe()
    imx_pinctrl_probe()
    //zz// drivers/pinctrl/pinctrl-imx.c
```

drivers/pinctrl/pinctrl-imx.c
imx_pinctrl_probe()
    ==>
    imx_pinctrl_probe_dt()
    pinctrl_register() // drivers/pinctrl/core.c
        pinctrl_register_pins()
        pinctrl_register_one_pin()


*********
## 28 优盘升级,lcd显示,结束home目录所有进程

### 1) 显示屏LCD打印输出文本信息
```
echo "hello asdfadsf" > /dev/tty0
setterm -clear all > /dev/tty0
```

给所有终端发送广播消息
```
tty
    /dev/pts/1
    查看当前终端为 /dev/pts/1
echo "xxx" > /dev/pts/1
    在别的终端向 /dev/pts/1 终端发送消息

echo "xxx" > /dev/console
    向串口控制台发送消息

echo -n -e "334455" > /dev/console
echo -n -e "\b\b\b" > /dev/console
    不要发回车,发送几个字符后,通过 \b 来删除

echo asdgfdshghth | wall
    将一段信息转发,广播到所有终端
wall xx.txt
    将文件内容转发,广播到所有终端

```

### 2) 读取配置文件
```
cat nvngd/guardCfg.ini  | grep -v -E "^;|^\[" | grep "path="
cat nvngd/guardCfg.ini  | grep -v -E "^;|^\[" | grep "path=" | xargs echo
	多行变成了1行

cat nvngd/guardCfg.ini  | grep -v -E "^;|^\[" | grep "path=" | sed 's/path=//g'
	去掉 "path="

cat nvngd/guardCfg.ini  | grep -v -E "^;|^\[" | grep "path=" | sed 's/.*\///g'
	正则替换,使用 .* 

cat nvngd/guardCfg.ini  | grep -v -E "^;|^\[" | grep "path=" | sed 's/.*\///g' | xargs pidof
	获取程序 pid

cat nvngd/guardCfg.ini  | grep -v -E "^;|^\[" | grep "path=" | sed 's/.*\///g' | while read line; do echo "hello $line"; done;
	使用 read 逐行读入 while do done 逐行处理
```
	
### 3) 获取 home 目录下所有存放的程序的在跑的进程pid
ls /proc | grep -E '[0-9]{2,}'
ls /proc | grep -E '[0-9]{2,}' | xargs


for dir in $(ls /proc | grep -E '[0-9]{2,}')
do
	"hello ${dir}"
done

```
## for dir in $(ls /proc | grep -E '[0-9]{2,}'); do; echo "hello ${dir}"; done;
	多行变成了1行,语法错误
for dir in $(ls /proc | grep -E '[0-9]{2,}'); do echo "hello ${dir}"; done
	多行变成了1行,可以用

```

basename $(readlink /proc/1247/exe)

*********
## 27 qt QRadioButton 文本颜色样式需有 spacing 属性

QRadioButton 样式设置,文本颜色color 需要设置 spacing 才能生效

QRadioButton {
	spacing: 2px;
	font: 14pt ;
	color: white;
}


*********
## 26 CAN 总线 ip命令, 设置 txqueuelen 大小
```
echo 1000 > /sys/class/net/can0/tx_queue_len
ip link set can0 txqueuelen 500
```

*********
## 25 arm linux 查看/设置系统频率,显示fb0大小

```
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
    800000
    => 查看当前cpu主频为800MHz

cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq

echo 800000 > /sys/devices/system/cpu/cpu0/cpufreq/scaling_setspeed
    设置cpu主频为 800MHz


cat /sys/class/graphics/fb0/virtual_size                  
    800,960
    => 表示屏幕分辨率为 800x480,横向因为双缓冲所有为实际屏幕宽度x2??

cat /sys/devices/virtual/input/mice/uevent
    查看输入设备
cat /proc/bus/input/devices
    查看当前系统所有输入设备的名称,主次设备号等信息
    tslib 配置要用到的 event0~event4 或 mice touchscreen0 等等根据这个来

cat /proc/devices
    所有设备的 /dev 主次设备号
cat /proc/interrupts
    系统所有中断号

```

*********
## 24 结构体中放 std::string 再 memset() 导致段错误

//zz.segfault// struct => std::string + memset => setfault

> REF:
    prj-ac2gun

*********
## 23 route arp

### REF:
    Linux 下查看局域网内所有主机IP和MAC 
    http://blog.chinaunix.net/uid-26729093-id-4257855.html

1) linux route
```
route -n

route add default gw 10.0.0.1
route delete default gw 10.0.0.1

route del  -net 169.254.0.0 netmask 255.255.0.0
route add  -net 169.254.0.0 netmask 255.255.0.0 dev eth1
route add  -net 169.254.0.0 netmask 255.255.0.0 gw 10.0.0.1

```

```
man route
route add -net 127.0.0.0 netmask 255.0.0.0 dev lo
route add -net 192.56.76.0 netmask 255.255.255.0 dev eth0

route add -net 192.57.66.0 netmask 255.255.255.0 gw ipx4
route add -net 224.0.0.0 netmask 240.0.0.0 dev eth0
route add -net 10.0.0.0 netmask 255.0.0.0 reject
    add/delete|del 增加或删除
    -net -host 指定目标网络或目标主机
    netmask 指定子网掩码
    gw 指定网管ip
    dev 指定网络接口

```

2) linux arp
查看局域网内所有主机

```
nmap -sP 192.168.1.0/24
nmap -sL 192.168.1.0/24
nmap -sS 192.168.1.0/24

cat /proc/net/arp
arm -n

nmap -sS 10.0.0.0/16
cat /proc/net/arp  | grep "10.0." | wc -l
    查看数量
```

*********
## 22 ac2gun 串口 数据库

```
stty -F /dev/ttyS3 speed 2400 cs8 parenb -parodd -cstopb  -echo
	# 电能表 2400 8 EVEN 1
stty -F /dev/ttyS6 speed 115200 cs8 -parenb -cstopb  -echo
	# 读卡器 115200 8 N 1

ll /dev/ttyS*
lrwxrwxrwx    1 root     root            12 Dec 27 05:19 /dev/ttyS3 -> /dev/ttymxc3
lrwxrwxrwx    1 root     root            12 Dec 27 05:19 /dev/ttyS4 -> /dev/ttymxc4
lrwxrwxrwx    1 root     root            12 Dec 27 05:19 /dev/ttyS6 -> /dev/ttymxc1
```

//zz.segfault// struct => std::string + memset => setfault
//zz.ac2gun// 充电桩/接口(枪) 过温告警 ==> 当做故障处理..
SELECT * FROM cf_errInfos WHERE notes GLOB '*充电桩过温*';
SELECT * FROM cf_errRelations WHERE actualType=3 AND actualCode=25;
INSERT INTO cf_errRelations (type,code,actualtype,actualcode) VALUES (14,2048,3,25);

// 充电接口过温 告警 ==> 故障
SELECT * FROM cf_errInfos WHERE notes GLOB '*充电接口过温故障*';
SELECT * FROM cf_errRelations WHERE actualType=3 AND actualCode=26;
INSERT INTO cf_errRelations (type,code,actualtype,actualcode) VALUES (10,131072,3,26);

// 充电接口过温故障
SELECT * FROM cf_errInfos WHERE notes GLOB '*充电接口过温故障*';
SELECT * FROM cf_errRelations WHERE actualType=3 AND actualCode=20;
INSERT INTO cf_errRelations (type,code,actualtype,actualcode) VALUES (10,131072,3,20);

### ------
增加 cf_errInfos
234	18	33554432	交流输出接触器粘连故障	0	2	2
INSERT INTO cf_errInfos (type,code,notes,needUpload,alarmLevel,procesFun) VALUES (18,33554432,'交流输出接触器粘连故障',0,2,2);

INSERT INTO cf_errRelations (type,code,actualtype,actualcode) VALUES (18,33554432,3,27);


*********
## 21 JAVA PATH 配置

C:\ProgramData\Oracle\Java\javapath;%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;F:\Git\cmd;D:\QT\5.5\msvc2013_64\bin

%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;C:\Program Files\Java\jdk1.8.0_60\bin;C:\Program Files\Java\jre1.8.0_60\bin;D:\Qt\mingw32\bin;

%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;%JAVA_HOME%\bin;D:\Qt\mingw32\bin;

JAVA_HOME
C:\Program Files\Java\jdk1.8.0_60

CLASSPATH
C:\Program Files\Java\jre1.8.0_60\lib


*********
## 20 v2v 调试 GUI界面修改
@ 2017-11-23

1. 启动 放电/充电 失败,原因 只有0,1
	=> 需要显示具体原因,比如 98 155 等等

2. 充电记录界面,显示为 在为0时候,只要
	用电电能 0.0
	放电电量 0.00000 

#### ------
这两个确认一下是否去掉
	额定容量	去掉
	核定容量	去掉
	损耗电量	去掉

#### ------
这几项需要修改
	用电电量	=>	充电电量
	用电电能	=>	放电电量
		放电电能(改为 放电电量)	=>	单位 kWh
	原 放电电量	=>	放电容量 单位: Ah

3. 客户端查 事件/告警 充电记录 时候
	超时卡死

SetTotalForm.ui

tBtn_chargRe
	充电记录

tBtn_eventRe
	事件/告警


*********
## 19 NvnbcSy.db

17	256		从机接收到主机故障
	==> 收到放电机停止充电请求
17	1024	达到放电终止电压停止充电
17	2048	达到放电终止SOC停止充电

####
ChargeSrv::StopChargeCompleteEvent()
	=> DataManager::Instance().SetEndChgStatus()

数据库中增加
cf_errInfos
	17	4096	收到放电机停止充电请求

cf_stopReasons
	停止原因 109
	17	4096	109

cf_errRelations 遥信实际故障 转 编码故障码
	不需要加
SELECT * FROM cf_errRelations WHERE actualType=20 AND actualCode=13;
SELECT * FROM cf_errInfos WHERE type=20 AND code=8192;

SELECT * FROM cf_errRelations WHERE actualType=20 AND actualCode=3;
UPDATE cf_errRelations SET actualCode=23 WHERE type=20 AND code=8;
SELECT * FROM cf_errRelations WHERE actualType=20 AND (actualCode=3 or actualCode=23);

	1	2	读卡器通讯故障
		20	3
	20	8	电能表时钟异常
		20	3 => 改为 20	23

*********
## 18. v2v Offilne
在哪里设置的 CAN 离线

与 server 无关
在 client 中根据与 server 的心跳来设置

client
IDevice::NvnbcCon()
	定时器中,检测到 10 秒钟未收到报文

#### ------
解决办法:
	CAN 总线/驱动问题, canconfig/ip link/ifconfig can0 down/up 也不行
	AtcCan 增加 ReOpen() 方法,将 socket CAN 关闭后重新打开


*********
## 17. LS_COLORS
LS_COLORS

rs=0:di=01;33:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lz=01;31:*.xz=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.axv=01;35:*.anx=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.axa=00;36:*.oga=00;36:*.spx=00;36:*.xspf=00;36

*********
## 16. smbclient

smbclient //10.0.3.253/zhongzhun -U zhongzhun
	输入密码
smbclient //10.0.3.253/zhongzhun -U zhongzhun%zhongzhun


mount -t cifs //10.0.3.253/zhongzhun /mnt/smbc253/ -o username=zhongzhun
	输入密码

mount -t cifs //192.168.7.1/office  /mnt/smbfs-cifs -o username=zhongzhun,password=zhongzhun

mount.cifs //192.168.7.1/office /mnt/smbfs-cifs -o username=zhongzhun
	输入密码

mount.cifs //192.168.7.1/office /mnt/smbfs-cifs -o username=zhongzhun,password=zhongzhun

*********
## 15. mysql

命令行 
mysql -u 用户名 -p 
	输入密码登录后执行 sql 语句
	关于字符串 引号的问题

#### 错误,不能使用 ' 或 " 括起来字段名,表名
CREATE TABLE IF NOT EXISTS 'runoot_tbl'('runoob_id' INT UNSIGNED AUTO_INCREMENT,'runoob_title' VARCHAR(100) NOT NULL,'runoob_author' VARCHAR(40) NOT NULL,'submission_date' DATE,PRIMARY KEY( 'runoob_id' ) )ENGINE=InnoDB DEFAULT CHARSET=utf8;

#### 要使用 ` 符号括起来,或者不用
CREATE TABLE runoob_tbl(runoob_id INT NOT NULL AUTO_INCREMENT,runoob_title VARCHAR(100) NOT NULL, runoob_author VARCHAR(40) NOT NULL, submission_date DATE, PRIMARY KEY ( runoob_id ) )ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE IF NOT EXISTS `runoob_tbl`(`runoob_id` INT UNSIGNED AUTO_INCREMENT,`runoob_title` VARCHAR(100) NOT NULL,`runoob_author` VARCHAR(40) NOT NULL,`submission_date` DATE,PRIMARY KEY( `runoob_id` ) )ENGINE=InnoDB DEFAULT CHARSET=utf8;


### 1.登录,添加用户,增加网络用户
mysqladmin -u root -p


mysql -u root -p
	show databases;
	use mysql;

	select host,user,authentication_string from user;


# grant all privileges on *.* to 创建的用户名 @"%" identified by "密码";
grant all privileges on *.* to zz @"%" identified by "zz";

### 2. 解决navicat 2003错误,重启服务

```
/etc/my.cnf
	#zz# bind-address		= 127.0.0.1
	#zz# bind-address		= 0.0.0.0
	#zz#
	skip-external-locking
	skip-name-resolve

systemctrl restart mysql.service
```

*********
## 14. client充电窗口中 "放电电能/用电电能" kWh数据来源
client ChargeDialog 放电电能 kwh
	=> server PGN.0XF9 广播充电终端实时数据

### ------
client:
BROADCAST_REAL_DATA_PGN 0XF9
	RecvBroadcastRealDataAck()

用电电能/放电电能
ChargeDialog::UpdateData()
	m_lblChargeEnergy ==> fChargeE+fConsumE
=>
	fChargeE = m_pDevice->TransRecord(m_gunNo).TotalPPRTALL

=>
IDevice::UpdateData()
	m_transRec[gunNo].TotalPPRTALL =  cmd.ChargeEnergy;	

==>
	ChargeSrv::RecvBroadcastRealDataAck() call

==>
####	CCU01Link::RecvBroadcastRealDataAck() call
==>
	CCU01Link::RecvPacket()
		case BROADCAST_REAL_DATA_PGN


### ------
server:
#### UiServerSrv::brocastTerminalRealTimeData()

#### S_A_TERMINAL_REALTIME_DATA 0XF9
DATA_S_A_TERMINAL_REALTIME_DATA terminalInfo;
	.ChargeEnergy
		IDevice-->TransRecord().TotalPPRTALL
	.ChargeTime
		IDevice-->TransRecord().ChargeTime

TotalPPRTALL =>
####	PaymentSrv::UpdateTransData()
=>
	TotalPPRT1 TotalPPRT2 TotalPPRT3 TotalPPRT4
=>
	EndPPRT1-BeginPPRT1 ...

### 关于电量,在做 v2v 时候被屏蔽了
/*
	//liu v2v
	transRec.EndTotalPPRT =  qRound(pEe->TotalEnergy*100);
	transRec.EndPPRT1 =  qRound(pEe->SharpEnergy*100);
	... ...
*/
	transRec.ChargeTime = (AtcUtility::GetSysRunningTimeSinceBoot() - transRec.StartChargintTimePoint)/1000/60;

### ------
### 关于时间,计算开始时间,当前时间,做减法
PaymentSrv::updateProcess()
	PaymentSrv::DealOrderUpdate()
		case OrderInfo::READY_TO_START_WORK:
		QtConcurrent::run() =>
#### UiServerSrv::startCharge()
#### OrderInfo::SetStartChargintTimePoint()
	

#### ------
逆向分析,电能数据最终来源于电能表通讯 dlt645-2007
#### MeterLink::ParsePacket2007()
	pMe->elecEnergy.TotalEnergy = elecenergy.TotalEnergy;
	pMe->elecEnergy.SharpEnergy = elecenergy.SharpEnergy;



*********
## 13. client => server 界面更换
client 左上角点击5次,密码进入设置
	系统设置 => 配置程序 => 进入 server 的 UI
	
	通过 udp 进行本地通讯的
	udpRead()

*********
## 12. alarm 问题

SELECT * FROM cf_errRelations WHERE actualType=20 and actualCode=3;
查出来有两个告警都是 actual <20,3>

```
1	2	|	20	3
20	8	|	20	3

```

SELECT * FROM rc_uploadAlarms WHERE type=1 AND errorCode=2;

*********
## 11. v2v 告警去掉

### 1) 实时告警
IDevice::
	RTAlarm m_RtAlarm[GUN_COUNT_MAX];
		vector<AlarmInfo> 内部是 type,code,time
	GetRtAlarmList()
	m_RtAlarm[]

IDevice::AddAlarm()
	AddAlarmList()

#### ------
IDevice::UpdateData()
根据遥信类型 + CAN遥信数据 => 计算出告警的 <type,code>
actualErrCode => (i*8+j) => GetErrCfgByActualErrCode() => ErrCfg


### 2) 告警处理,是否可继续充电
DeviceManager::
	GetDeviceCanCharge()

告警配置 => 根据级别,处理方式 确认是否可充电
```
<type,code>
<1,2>				读卡器通讯故障
<20,16>				后台通讯故障

<20,8>				电能表时钟异常
<20,838868>			电能表数据异常

#### 这个不用屏蔽
# <15,134217728>		整组模块通讯故障

故障时候还可充电的类型
<2,16384>				直流电能表通讯故障
```

*********
## 10. ATC alarm告警

map<AlarmKey, ErrCfg>
	AlarmKey 含有 type code
	ErrCfg 包含一种告警的所有信息..

SELECT * FROM cf_errInfos WHERE notes GLOB "电能表时钟异常";
SELECT * FROM cf_errInfos WHERE notes GLOB "整组模块通讯故障";

告警配置从数据库 NvnbcSy.db 中加载
cf_errInfos
	这个表存放 <type,code> 对应的告警信息,级别,处理方式
cf_errRelations
	这个表存放本地告警 <type,code> 与 Actual远程后台告警 <type,code> 的对应关系

### 1)
type + code => 确定一种错误,ATC对所有错误的编码

### 2)
m_alarmKeyMap
m_alarmActualKeyMap

*********
## 9. zz.v2v 启动充电,停止充电

//zz.v2v// @ 2017-11-1
//zz.v2v//
server
	online cardType 返回 白卡
client
	启动充电 编写 SwipCardInfo => 启动充电
	停止充电 编写 SwipCardInfo => 停止充电



*********
## 8. 关于数据库

NvnbcSy.db
	常量,例如告警时候code与info的对应表

NvnbcCf.db
	程序配置数据,server/client都可以用到

NvnbcRc.db
	充电记录,告警记录
NvnbcBu.db
	NvnbcRc.db 的备份,NvnbcRc.db不能写入等问题是,将其数据拷贝过来用

#### 另外:
	黑名单,白名单在 namelist.ini 配置文件中


*********
## 7. v2v 搜索修改

//liu uncharge
	基于 1.1 的版本增加放电功能

//liu v2v
	funcCode 数据库中对 v2v 充电/放电属性配置
	初始化来自 CCU01Link::RecvUpdataTerPrmAck()
	
	通过 UPDATA_TER_PRM_ACK_CMD 结构体


*********
## 6. v2v 修改
IDevice::
增加了 serviceType 变量
m_pDevice->serviceType


*********
## 5. 数据库中增加 funcCode
client
UPDATA_TER_PRM_ACK_CMD 结构体中增加 funcCode 变量
	funcCode 数据库中对 v2v 充电/放电属性配置
	初始化来自 CCU01Link::RecvUpdataTerPrmAck()

server
NvnbcCf.db
	cf_terminals
		funcCode

*********
## 4. 未启动放电时候,不允许启动充电
client
ChargeDialog::OnChargeBtnClicked()
	Data::CHARGE_SERVICE
	if(DeviceManager::Instance().GetUnchargeDev()->GetWorkState() != UnCharging)
	"请先启动放电!"

*********
## 3. 电能表地址配置检测,不读电表
server

Meter::Meter()
	m_meterList 取消 push,不读电表

SetTerminalForm::on_tBtn_sure_clicked()
	不判断 电能表地址不规范

PaymentSrv:: 计费时不管电表,不计算
	StartBilling()
	StopBilling()
	UpdateTransData()


*********
## 2. 账单的存储
1个放电账单 => 可对应多个充电账单
1个放电 N个充电 => id 相同

*********
## 1. 数据库错误信息

SELECT * FROM cf_errInfos WHERE notes GLOB "整组模块通讯故障";
SELECT * FROM cf_errInfos WHERE notes GLOB "电能表时钟异常";

SELECT * FROM cf_errInfos WHERE type=20;


*********
## 0. Markdown CSS

```css
body {
color:#D6DBDF;
background-color:#444444;
}

code {
background-color:#333333;
}

h1 {
color:#0088CC;
background-color:#444444;
}

h2 {
color:#0088CC;
background-color:#444444;
}

h3 {
color:#0088CC;
background-color:#444444;
}

h4 {
color:#0088CC;
background-color:#444444;
}

```

### css.2017-8-23
```css
h1 {
    color: #F00000; /* 红色 */
}

h2 {
    color: #00aabb; /* 蓝色 */
}

h3 {
    color: #FFC90E; /* 橙黄色 */
}

h4 {
    color: #80F000; /* 亮绿色 */
}

```
