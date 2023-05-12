## Git操作
### 上传文件夹到GitHub

* **[参考链接](https://blog.csdn.net/jojowei/article/details/89008657)**
* **概括：**
  * **1.cd 文件夹名**
  * **2.“ git init ” git初始化**
  * **3.git add . ” 提交到缓存区**
  * **4." git commit -m “你的提交信息” "** 是将暂存区里的改动给提交到本地的版本库。
  * **5.“git push ”** 是将本地版本库的分支推送到远程服务器上对应的分支,提交到远程的github仓库 * 
  > 如果" git push "不成功, " git push -u origin master "也不能解决问题，如果是因为github中的README.md文件不在本地代码目录中， 可以通过如下命令进行代码合并git pull --rebase origin master。这里，pull=fetch+merge。合并后再用“git push”就可以上传了。

  > [git pull 更新代码出错参考](https://blog.csdn.net/yjw123456/article/details/119696726)

* 其他方法：
  ```
  $ git commit -m "first commit"
  $ git remote add origin https://github.com/Chanyelo9/vscode-git
  $ git push -u origin master
  ```


### git路径
* where git



