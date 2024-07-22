*开始之前你可能需要 Google 了解的概念： assembler, BIOS*

**目的： 创建一个文件 BIOS 将文件识别为一个可引导的磁盘**

这是非常令人兴奋的，我们将创建我们自己的引导扇区！ 

理论基础
------

计算机上电启动后，首先运行主板 BIOS 程序， BIOS 并不知道如何加载操作系统，所以 BIOS 把加载操作系统的任务交给引导扇区。
因此，引导扇区必须放在一个已知的标准位置，这个位置就是磁盘的第一个扇区(cylinder 0, head 0, sector 0) 该扇区一共占用 512 字节。

为了确保 “磁盘是可引导的” BIOS 会检查扇区的第 511 和 512 字节是不是 `0xAA55`.

下面就是最简单的引导扇区：

```
e9 fd ff 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
[ 29 more lines with sixteen zero-bytes each ]
00 00 00 00 00 00 00 00 00 00 00 00 00 00 55 aa
```

你看到的基本上都是 0，最后以 16 位的 `0xAA55` 结束。(注意字节顺序， x86 是小端结构)。开头的三字节表示计算机进入一个无限循环。

最简单的引导扇区
-------------------------

你可以使用一个二进制编辑器编写以上 512 字节数据，或者简单的编写一段汇编代码：

```nasm
; Infinite loop (e9 fd ff)
loop:
    jmp loop 

; Fill with 510 zeros minus the size of the previous code
times 510-($-$$) db 0
; Magic number
dw 0xaa55 
```

编译
`nasm -f bin boot_sect_simple.asm -o boot_sect_simple.bin`

> OSX 提醒： 如果这里出现错误，请重新阅读 00 章节

我知道你一定迫不及待的想运行一下 (我也是!)，那么我们开始运行它：

`qemu boot_sect_simple.bin`

> 有些系统下，你可能需要运行 `qemu-system-x86_64 boot_sect_simple.bin` 如果出现 SDL 错误， 传递 --nographic 或者 --curses 参数给 qemu 程序。

你将会看到一个窗口打印 "Booting from Hard Disk..." 然后就没有然后了。你最后一次看到无限循环还非常的兴奋是什么时候？ ;-)
