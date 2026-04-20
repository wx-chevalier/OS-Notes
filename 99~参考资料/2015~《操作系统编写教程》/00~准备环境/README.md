_开始之前你可能需要 Google 了解的概念： linux, mac, terminal, compiler, emulator, nasm, qemu_

**目的：安装运行教程代码所需要的软件**

我在一台 Mac 电脑上工作，但是 Linux 是一个更好的选择因为 Linux 一般都提供了你需要的标准工具。

在 Mac 上，[安装 Homebrew](http://brew.sh) 然后运行 `brew install qemu nasm`

如果你已经安装了 Xcode ，不要使用开发工具里面的 `nasm` ，大多数情况下，这个程序都没办法正常工作，我们会一直使用 `/usr/local/bin/nasm`

在有些系统上 qemu 分成好几个二进制文件，你可能会使用 `qemu-system-x86_64 binfile`
