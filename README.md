# mips_compile_win
注：本repo不包含代码，仅仅是对[binutils2.22](https://ftp.gnu.org/gnu/binutils/binutils-2.22.tar.gz)交叉编译出能在windows平台上运行的mips汇编链接等操作的可执行文件。

交叉编译平台：kali linux子系统，4.4.0-18362-Microsoft。

编（cai）译（keng）过程（release内有现成的二进制文件，也可自己尝试编译，不保证成功）

```bash
$wget https://ftp.gnu.org/gnu/binutils/binutils-2.22.tar.gz
$tar xzvf binutils-2.22.tar.gz
$cd binutils-2.22
$bash ##bash进入子系统
$sudo apt-get install mingw-w64 ##确保交叉编译工具被安装，不同平台或许不一样

$../binutils-2.22/configure --target=mips-elf --prefix=/opt/mips-gnu-tools --enable-multilib --host=i686-w64-mingw32 --build=i686-pc-linux-gnu --disable-werror
$make
##如果报错，请注释/binutils/bucomm.h 中set_time函数的声明（老旧编译器的弥天大坑，会产生conflict声明）后再次make
$sudo make install##产生的目标文件在/opt/mips-gnu-tools下

```

测试程序：

```assembly
#test.S
.text
.globl _start
_start:
    addiu   $10,    $0,     0x0001
    nop
    slti    $11,    $12,    0x0002
    
```

汇编、链接、反汇编：

```bash
$as test.S -o test.o -EL -mips1
$ld test.o -o test -EL -Ttext 0xbfc00000
$objdump -D test
```

测试截图：

![Qqd33n.png](https://s2.ax1x.com/2019/12/19/Qqd33n.png)