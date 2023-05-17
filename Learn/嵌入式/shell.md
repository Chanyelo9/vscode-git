[TOC]
# 课程来源于“B站尚硅谷”
# 一、Shell 概述
Shell是一个命令解释器，接收应用程序/用户命令，然后调用操作系统内核。它提供交互式的文本命令台。
![](${currentFileDir}/20230515113314.png)
Shell还是一个功能强大的编程语言，易编写，易调试，灵活性强。

## 1.Linux提供的Shell解析器：
 > 切换root：sudo -u
 ```
 root@linux:~# cat /etc/shells
 ```
## 2.bash和sh的关系
 ```
 root@linux:~# ls -l /bin/ | grep sh
 root@linux:~# ls -l /bin/ | grep sh
 ```
## 3.默认解析器是：
 ```
 root@linux:~# echo $SHELL
 ```

# 二、Shell 脚本入门
## 1.脚本格式
脚本以 #!/bin/bash 开头（指定解析器）
* ### 基本命令
  ```
  查看权限：ll scripts/
  ```
## 2.第一个Shell脚本：hello.sh
* ### 案例实操
```
root@linux:~# touch hello.sh
root@linux:~# vim hello.sh
//写入内容
#!/bin/bash
echo "hello world"
```
 ## 3.脚本的常用执行方式
  * #### 1.采用bash或sh+脚本的相对路径或绝对路径（不用赋予脚本+x权限）
  ![](${currentFileDir}/20230515151805.png)
  ```
  root@linux:~# bash scripts/hello.sh
  root@linux:~# bash /root/scripts/hello.sh
  ```
  * #### 2.采用输入脚本的相对路径或绝对路径执行脚本（必须具有可执行+x权限）
  ![](${currentFileDir}/20230515152715.png)

  > * ###  注意:
  >   第一种执行方式，本质是bash解析器帮你执行脚本，所以脚本本身不需要执行权限。第二种执行方法，本质是需要自己执行，所以需要执行权限。

  * ### 3.在脚本路径前加上“.”或者source
  ![](${currentFileDir}/20230515153816.png)
  ![](${currentFileDir}/20230515155504.png)

  > * ###  原因:
  > * 1.前两种方式都是在当前Shell中打开了一个子Shell来执行脚本内容，当脚本内容结束，则子Shell关闭，回到父Shell中。
  > * 2.第三种，可以使脚本内容在当前Shell里执行，而无需打开子Shell。
  > * 3.开子shell与不开子shell的区别在于，环境变量的继承关系，如在子shell中设置的当前变量，在父shell中不可见。

# 三、变量
## 1.系统预定义变量
* ### 1）常用环境变量
  $HOME、$PWD、$SHELL、$USER等。
* ### 2）案例
  ![](${currentFileDir}/20230515161249.png)
  也可以用**printenv HMOE**， 不用加$。
  ```
  set | grep 
  ```
## 2.自定义变量
* ### 1）基本语法
  ![](${currentFileDir}/20230515162538.png)
* ### 2）变量定义规则
  ![](${currentFileDir}/20230515162625.png)
  ```
  set | grep my_var 显示所有变量
  ```

  > 单引号全部输出，双引号会辨识变量

  命令都是直接放在/bin目录下的，若想直接运行hello.sh文件，需把文件拷贝cp到bin目录下：
  ```
  # cp hello.sh /bin/
  # hello.sh
  ```
  查看可直接执行的路径：
  ```
  # echo $PATH
  ```

## 3.特殊变量
* ### 3.1 $n
* #### 1）基本语法
   ![](${currentFileDir}/20230515172159.png)
* #### 2）实例操作
  ![](${currentFileDir}/20230515173807.png)
* ### 3.2 $#
* #### 1）基本语法
   ![](${currentFileDir}/20230515173919.png)
* #### 2）实例操作
   ![](${currentFileDir}/20230515173942.png)
* ### 3.3 ￥*， ￥@
* #### 1）基本语法
   ![](${currentFileDir}/20230515174315.png)
* ### 3.4 $？
* #### 1）基本语法
   ![](${currentFileDir}/20230515174714.png)
* #### 2）实例操作
  判断hello.sh脚本是否正确执行
  ![](${currentFileDir}/20230515174821.png)

# 四、运算符
* ## 基本语法
  ![](${currentFileDir}/20230515181524.png)
  > 也可以用expr，如expr 1 + 2，需要用空格，用乘法要进行转义\*.
  ```
  **两种表达**
  # a=`expr 5 \* 3`
  # a=$(expr 5\* 3)
  ```
  ### * 条件判断[]里要空格，运算不需要。
# 五、条件判断
* ## 1.基本语法
  ![](${currentFileDir}/20230515191308.png)
  ```
  (1)
  # test $a = Hello
  # echo $?
  (2)
  # [ $a = hello ]
  # echo $?
  ```
* ## 2.常用判断条件
  ### （1）两个整数比较
  ![](${currentFileDir}/20230515191352.png)
  ### （2）按照文件权限进行判断
  ![](${currentFileDir}/20230515192617.png)
  ### （3）按照文件类型进行判断
  ![](${currentFileDir}/20230515192732.png)
  > **补充：**
  ![](${currentFileDir}/20230515193657.png)

# 六、流程控制
* ## 1.基本语法
* ### （1）单分支
  ![](${currentFileDir}/20230515194521.png)
  > 拓展用法
  root@linux:/home# cd /home/; ls -l
  root@linux:/home# 

  **或者**
  ![](${currentFileDir}/20230515194547.png)
  ****
  **多条件时，有两种表达方式：**
  ```
  1. if [ $a -gt 18 ] && [ $a -lt 35 ]; then echo OK; fi
  2.if [ $a -gt 18 -a $a -lt 35 ]; then echo OK; fi
  其中，与是-a，或是-o
  ```
  ****
  **在编写shell时，为避免空指针，可以添加双引号和x：**
  ![](${currentFileDir}/20230516102128.png)
  
* ### （2）多分支
  ![](${currentFileDir}/20230516104153.png)
  > 注意：
  ![](${currentFileDir}/20230516104238.png)

* ## 2.case
* ### （1）基本语法
  ![](${currentFileDir}/20230516105243.png)
  > 注意
  ![](${currentFileDir}/20230516105145.png)

* ## 3.for
* ### （1）基本语法
  ![](${currentFileDir}/20230516110442.png)
* ### （2）案例实操
  从1加到100：
  ![](${currentFileDir}/20230516110546.png)
* ### （3）基本语法2
  ![](${currentFileDir}/20230516114322.png)
  ```
  # for os in linux windows macos; do echo $os; done
  # for i in {1..100}; do sum=$[$sum+$i]; done; echo $sum
  ```
   #### 比较$*和$@区别
  ![](${currentFileDir}/20230516123905.png)
  * 1.
    ![](${currentFileDir}/20230516125440.png)
    ![](${currentFileDir}/20230516125745.png)
  * 2.
    ![](${currentFileDir}/20230516130731.png)
    ![](${currentFileDir}/20230516125702.png)
* ## 4.while
* ### （1）基本语法
  ![](${currentFileDir}/20230516130852.png)
  * #### 可调用高级语法
    案例：从1加到100
    ![](${currentFileDir}/20230516132625.png)

# 七、read读取控制台输入
* ## 1.基本语法
  ![](${currentFileDir}/20230516133150.png)
* ## 2.案例实操
  ![](${currentFileDir}/20230516133802.png)
  ![](${currentFileDir}/20230516133833.png)

# 八、函数
## 1.系统函数（也可称命令替换）
* ### **在脚本中调用系统函数/系统命令**
    使用$( 系统命令 )
  * #### 定义日志文件使用脚本自动生成：
    ![](${currentFileDir}/20230516151915.png)
  * #### ${var}，即是加一个大括号来限定变量名称的范围，看清定义的变量的范围，不加也行。
    ![](${currentFileDir}/20230516165338.png)
* ### 1）basename
  * #### （1）基本语法
    ![](${currentFileDir}/20230516150445.png)
  * #### （2）案例
    ![](${currentFileDir}/20230516150601.png)

* ### 2）dirname
  * #### （1）基本语法
    ![](${currentFileDir}/20230516152946.png)
  * #### （2）案例
    ![](${currentFileDir}/20230516154922.png)
## 2.自定义函数
* ### 1）基本语法
  ![](${currentFileDir}/20230516155520.png)
* ### 2）技巧
  ![](${currentFileDir}/20230516155652.png)
* ### 3）案例
  ![](${currentFileDir}/20230516160940.png)

# 九、综合应用
![](${currentFileDir}/20230516162112.png)
```
# vim daily_archive.sh
# chmod u+x daily_archive.sh
# ./daily_archive.sh /root/scripts
# mkdir /root/archive
# ./daily_archive.sh /root/scripts
# crontab -l
# crontab -e
编辑内容：0 2 * * * /root/scripts/daily_archive.sh /root/scripts
```

# 十、正则表达式
![](${currentFileDir}/20230516170610.png)
## 支持扩展：grep -E
  ![](${currentFileDir}/20230516174109.png)
  其中表示0-9可以重复9次，[0,-9]{9}
## 1.常规匹配
![](${currentFileDir}/20230516171005.png)
## 2.常用特殊字符
* ### （1）特殊字符：^
  ![](${currentFileDir}/20230516171126.png)
* ### （2）特殊字符：$
  ![](${currentFileDir}/20230516171424.png)
* ### （3）特殊字符：.
  ![](${currentFileDir}/20230516171527.png)
* ### （4）特殊字符：*
  ![![](${currentFileDir}/20230516171527.png)](${currentFileDir}/20230516172103.png)
* ### （5）字符区间：[]
  ![](${currentFileDir}/20230516172811.png)

# 十一、文本处理工具
## 1.cut
![](${currentFileDir}/20230516195111.png)
* ### 1）基本用法
  ![](${currentFileDir}/20230516195309.png)
* ### 2）选项参数说明
  ![](${currentFileDir}/20230516195247.png)
* ### 3）案例
  ![](${currentFileDir}/20230516201533.png)
  ![](${currentFileDir}/20230516201616.png)
  ![](${currentFileDir}/20230516202157.png)
## 2.awk
![](${currentFileDir}/20230516203554.png)
* ### 1）基本用法
  ![](${currentFileDir}/20230516203639.png)
* ### 2）选项参数说明
  ![](${currentFileDir}/20230516203708.png)
* ### 3）案例
  ![](${currentFileDir}/20230516204515.png)
  * #### (1)(2)可以用cat代替，不用输入filename，直接用cat打开
   ```
   root@linux:~/scripts# cat /etc/passwd | grep ^root | cut -d ":" -f 7
   /bin/bash
   root@linux:~/scripts# cat /etc/passwd | awk  -F ":" '/^root/{print $7}'
   /bin/bash
    ```
  ![](${currentFileDir}/20230516205756.png)
* ### 4）awk的内置变量
  ![](${currentFileDir}/20230516210053.png)
* ### 5）案例
  ![](${currentFileDir}/20230516211157.png)
* ### 6）awk VS grep
  显示行号：
  ![](${currentFileDir}/20230516211618.png)
  其中：^$ 表示空行。
  
  
  ![](${currentFileDir}/20230516212212.png)

# 十二、综合应用：发送消息
![](${currentFileDir}/20230516213228.png)
![](${currentFileDir}/20230517120514.png)

# 完结撒花，历时两天 5/15/2023——5/16/2023

