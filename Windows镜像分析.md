## 如何获取进程的PoolTag
参考：[https://forensicskween.com/docs/windows-volshell/](https://forensicskween.com/docs/windows-volshell/)

```shell
dt( "_POOL_HEADER" , 0x000000007fca3820-0x60 , space=addrspace().base)
```

其中0x000000007fca3820为进程的物理地址

## 获取桌面图像信息
可以将在桌面上的进程（<font style="color:rgb(77, 77, 77);">screenshot可查看桌面上运行的程序</font>）用memdump -p [pid] dump出来，再改后缀为data，用GIMP调分辨率和位移，可恢复图像。

未复现成功。

## 关于物理地址和虚拟地址
vol2中

```shell
pslist --physical # 不带后缀是虚拟地址
psxview # 直接是物理地址
```

vol3中

```shell
pslist # 虚拟地址
psscan # 物理地址，可扫描隐藏程序
```

