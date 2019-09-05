# sys_develop_fall_2019

重庆大学 2019年秋季系统能力课程改革计划

# Part1 清华 Supervisor

## 虚拟机上运行supervisor
****
在是实现完自己的cpu之后，在系统上可以尝试加载启动程序与轻量级的操作系统。但是在加载系统的时候可能会有各种错误，自己难以看出来，这时候可以借助清华的supervisor-32：32位的监控程序来查看寄存器的状态及内存状态等。监控程序可在学生实现的32位MIPS CPU上运行，来验证CPU功能正确性。

监控程序分成kernel和term两个部分。具体两个部分的详细说明可以参考[supervisor](https://github.com/z4yx/supervisor-mips32)。下面主要介绍如何在虚拟机上面启动supervisor熟悉使用，下面的实验环境都是在ubuntu16.04上进行的。

### qemu
我们需要用qemu虚拟机模拟mips架构的虚拟机，将kernel的程序加载到这个虚拟机上面运行。下面是在ubuntu16.04上面安装qemu的步骤。参考[lyrich博客](https://blog.csdn.net/u013213111/article/details/88871290)
1. 安装SDL 2.0，否则运行起来只有VNC server runing on .....。
```BASH 
sudo apt-get install libsdl2-2.0 
```

```BASH 
sudo apt-get install libsdl2-dev 
```

2. 下载编译QEMU3.0.0
```BASH
wget https://download.qemu.org/qemu-3.0.0.tar.xz
tar xvJf qemu-3.0.0.tar.xz
cd qemu-3.0.0
./configure --enable-kvm --enable-debug --enable-vnc --enable-werror
make
sudo make install flex
sudo make install bison
sudo make install
```
(qemu安装依赖glib)
在进行./configure出现两个错误：
（1）ERROR: glib-2.40 gthread-2.0 is required to compile QEMU
解决方法：sudo apt-get install libglib2.0-dev
（2）ERROR: pixman >= 0.21.8 not present. Please install the pixman devel package.
解决方法：sudo apt-get install libpixman-1-dev
./configure完成后的输出中应该有SDL suppport yes

### kernel
Kernel 使用汇编语言编写，使用到的指令有20余条，有三个版本。首先需要对Kernel的汇编代码进行汇编。
1. 下载[MTI BARE Metal](https://cloud.tsinghua.edu.cn/f/16dde018b00749a4a4de/) 编译器，将bin文件夹添加到系统的PATH中。make的时候才有对应的编译器。
2. 在kernel下面，终端使用命令
```make ON_FPGA=n```
即可开始编译，将生成`kernel.elf`和 ```kernel.bin``` 文件.（如果出现一系列的mips环境错误，说明第一步的bin环境没有配置成功）
3. 终端运行```make sim```会在终端的QEMU中启动监控程序，等待Term程序连接。（实验者需要安装QEMU）

### Term
Term 运行在实验者的电脑上，由python编写的。python3运行term.py文件。这次是使用qemu模拟器。
```BASH
python term.py -t 127.0.0.1:6666
```
（如果出现缺少mips相关工具链很大可能是kernel步骤1没有配置好。）
测试程序查看[supervisor](https://github.com/z4yx/supervisor-mips32)的readme文件最后。验证有没有配置成功。

1. 加入串口外设
2. 编译supervisor
3. 调试用户程序（TLB）
    自己编写TLB测试汇编代码

# Part2 PMON
1. 补充指令
2. 修改CP0
3. 移植PMON

# Part3 ucore os
1. TBD...
<!-- # Part4 Linux -->




