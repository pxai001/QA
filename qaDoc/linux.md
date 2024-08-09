1. 文件操作 
   * 创建文件 touch xxx.xxx
   * 删除文件 rm -rf xxx.xxx
   * 移动文件 mv xxx.xxx ./xxx.xxx
   * 拷贝文件：cp -r xxx .xxx ./xxx.xxx
   * 创建删除文件夹：mkdir rmdir
   * 压缩，解压：tar -zcvf xx.tar.gz，tar -zxvf xx.tar.gz；zip xx.zip，unzip xx.zip；
   * 查看结尾：tail -f
   * 查看全部内容：cat xxx
   * 分页查看内容：less xxx， 空格向下翻页，回车向下滚动一行，b向上翻页，y向上滚一行，/xxx 过滤 n下一个匹配项 N 上一个匹配项，G 文件末尾 g文件开头
2. 进程操作 
   * 查看进程 信息 ps -aux | grep xxx
   * kill 关闭进程
3. 线程操作 
   * ps -T -p pid
4. 目录操作
   *  ls -al
   * pwd 当前目录