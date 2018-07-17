# 2018SCTF

## re_crackme

![images](./images/sctf01.png)   

tryit是一个natvie函数，native函数只要打开so文件查看函数名，就能定位出对应Java函数的具体位置。

函数体为乱码，通过init动态调试。

###调试过程###

调试工具：gdbserver（因为android_server在x86模拟器上不能用）

搭建环境：


- 下载ndk之后将找到gdbserver之后push到/system/bin中，修改权限777
- 实现端口转发	
- 运行./gdbserver监听成功之后
- 在cmd下运行gdb（ndk中自带gdb.exe)



