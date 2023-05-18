#课程来源于“B站北京讯为电子”
信号：控件发出特定的信号。
槽：即槽函数，可以把槽函数绑定在某一个控件的信号上。
如何关联信号和槽：1.自动关联；2.手动关联。
2.手动关联，使用connect函数：
![](${currentFileDir}/20230518103615.png)

Qt三驾马车：
1.Qt下的串口编程
2.Qt下的网络编程
3.Qt下操作GPIO

Qt打包和部署：
1.把工厂切换到release模式，然后编译。
release模式：几乎没有调试信息。
debug模式：有很多调试信息。
2.找到release模式构建的文件夹。
3.改图标。把图标放到工程所在文件夹中，在Pro文件里面添加：RC_ICONS=favicon.ico。
其中，文件格式为.ico。[ico图标模式转换链接](http://www.ico51.cn/)
4.封包操作，需要Qt操作台：Qt 5.9 for Desktop (MinGW 5.3.0 32 bit)；然后桌面新建文件夹，将exe文件添加到文件夹中。
![](${currentFileDir}/20230518211030.png)

使用windeployqt工具自动将库添加到文件夹中
![](${currentFileDir}/20230518211205.png)





### 快捷键
Ctrl选中，可同时修改文本内容。