[TOC]
### E:无法获得锁/var/lib/apt/lists/lock - open (11 资源临时不可用)
[参考链接](https://blog.csdn.net/mmmsss987/article/details/103096502)
原因：刚装好的Ubantu系统，内部缺少很多软件源，这时，系统会自动启动软件源更新进程“apt-get”，并且它会一直存活。由于它在运行时，会占用软件源更新时的系统锁（以下称“系统更新锁”，此锁文件在“/var/lib/apt/lists/”目录下），而当有新的apt-get进程生成时，就会因为得不到系统更新锁而出现"E: 无法获得锁 /var/lib/apt/lists/lock - open (11: Resource temporarily unavailable)"错误提示！因此，我们只要将原先的apt-get进程杀死，从新激活新的apt-get进程，就可以让新立德软件管理器正常工作了！

先查看进程是否存在：
```
ps -e | grep apt
```
然后执行：
```
sudo killall apt
sudo killall apt-get
sudo killall synaptic
```
再次查看，继续执行sudo apt-get update.

Qt网络编程：TCP和UDP。
TCP编程需要两个类：QTcpServer和QTcpSocket。





