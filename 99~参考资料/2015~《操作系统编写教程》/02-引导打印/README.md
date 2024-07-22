*开始之前你可能需要 Google 了解的概念：interrupts, CPU
registers*

**目的： 让之前的引导程序输出一些文本**

我们对之前的无限循环引导扇区做点改进，让它能够打印点东西在屏幕上。我们通过发出中断来完成这个功能。

在这个例子中我们将会向寄存器 `al` (`ax` 的低端部分) 写入 “hello” 单词的每个字符，把 `0x0e`
写入 `ah` (`ax` 的高端部分) 然后发出中断 `0x10`，这是视频服务的通用中断。

`0x0e` 写入 `ah` 告诉视频中断我们想要以 tty 模式显示 `al` 寄存器中的内容。

我们只会设置 tty 模式一次尽管实际上我们不能保证 `ah` 中的内容是不变的。在我们的程序休眠的时候，其他的进程可能仍然在 CPU 上执行，也许它们没有正确的清空 `ah` 甚至遗留一些垃圾数据在里面。

在这个例子中，我们不需要考虑这个问题，因为我们的引导扇代码是唯一能够在 CPU 上执行的程序。

我们的新引导扇区代码看起来是这个样子：

```nasm
mov ah, 0x0e ; tty mode
mov al, 'H'
int 0x10
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
int 0x10 ; 'l' is still on al, remember?
mov al, 'o'
int 0x10

jmp $ ; jump to current address = infinite loop

; padding and magic number
times 510 - ($-$$) db 0
dw 0xaa55 
```

你可以使用 `xxd file.bin` 命令检查二进制数据

当然你已经知道我们的编译和运行步骤：

`nasm -fbin boot_sect_hello.asm -o boot_sect_hello.bin`

`qemu boot_sect_hello.bin`

现在你的启动引导程序会打印 'Hello' 然后停止在无限循环中。
