# Linux基本操作

### 文件目录类

* 一些不一样的命令

  ```bash
  # 在命令前加上\可以使用linux的原生命令，不带某些参数了
  
  比如type ls:会显示ls是某个命令的别名；
  
  # 查看所有具有别名的命令
  alias
  ```

  

* 显示当前绝对路径

  ```
  pwd
  ```

* 切换工作目录：cd : ChangeDirectory

  ```bash
  # 切换到指定路径
  cd+绝对路径；
  cd /  #切换到根目录
  
  # 在该目录下进行切换
  cd ./
  
  # 同一级路径相互切换:..代表返回上一级目录
  cd ../路径
  
  # 切换到上一次的路径
  cd - 
  
  # 回到用户的主文件夹，如果是以root用户登陆的，切换的就是/root；如果是以zh用户登陆的，切换的就是/home/zh
  cd
  
  
  ```

* 显示文件：ls

  ```bash
  # 列举出该目录下所有文件
  ls -a:显示所有文件
  ls -l:显示文件的所有信息：权限、所有者
  ls -al:支持两个参数放在一块
  # 每行第一个是-的话，代表是文件；第一行是d代表是目录
  # ll就是ls的别名，可以执行type ll 查看
  
  月份前面的那些数字代表文件大小
  # 显示根目录的所有文件
  ls / 
  ```

* 创建文件夹:mkdir

  ```bash
  # 在当前目录下创建文件夹
  mkdir a
  # 在指定目录下创建文件夹:在a文件夹下创建b文件夹
  mkdir /a/b
  # 创建路径的时候，如果他的父目录不存在，想要在创建的时候，一块创建出来，用到-p参数
  
  ```

* 删除文件夹：rmdir,非空的目录不能删除 

  ```bash
  # 删除该目录下的b文件夹
  rmdir b
  # -p 参数，如果子文件夹是空的，删除之后，父文件夹也是空的，也会把他的父文件夹删除，以此类推
  rmdir -p c/d/e
  ```

* 创建空文件：touch

  ```bash
  # 不指定后缀名默认是文本文件
  touch hello
  # 可以指定文件的路径
  
  # 也可以直接vim + 文件名，但是如果没有内容，推出之后这个文件不会保存
  ```

* 复制文件：cp [选项] source dest ；复制source文件到dest

  ```bash
  # 先返回当前目录的上一级目录，然后找到模板/这个目录，把hello复制到这个目录下面
  cp hello ../模板/
  
  # 如果dest填的是一个文件名，那么就是覆盖这个文件了；系统会提示，是否要覆盖？输入y或者n
  
  
  # -r 递归复制某个文件夹，将目录下的所有文件夹，都复制过去；把a及其下面的文件和目录全部复制过去
  cp -r a/
  ```

* 删除文件：rm

  ```bash
  # 删除文件
  rm hello
  
  # 删除某一目录及其下面的所有文件:递归删除所有文件
  rm -r a
  ```

* 移动文件或者目录：mv 文件 目标目录

  ```bash
  # 如果目标是文件名，就是移动过去然后改名；如果是目录名，就是简单地移动过去；
  
  ```

* 查看文件：cat: catch

  针对一些比较小的文件；

  ```bash
  # cat + 文件名
  cat hello.sh
  # 显示行号
  cat -n hello.sh
  
  ```

  大文件查看：more:文件内容分屏查看器;less也是分屏查看，但是不是一次加载完所有，而是根据需要选择加载，打开大文件更有效率

  ```bash
  # more完之后，进入了一个类似与vi编辑器的页面，当然不能更改文件，但是有一些快捷键
  
  我的more怎么没有进入一个新的页面,还是在当前bash中显示的
  ```


* 查看系统环境变量PATH

  都有哪些：PATH里的路径，系统在执行命令时除了在当前目录下面寻找此程序外，还应到path中指定的路径去找。

  ```bash
  echo $PATH
  
  # 更高大上的显示
  echo $PATH | lolcat
  ```

* 追加内容

  " > " :  是覆盖写;

  " >> " 追加内容到目标文件

  ```bash
  ls -al 文件名
  
  ```

* 软链接

  创建软链接：

  ```bash
  ln -s [元文件或目录] [软链接名]
  
  #不做演示了 
  ```

* 显示历史命令

  ```bash
  # 显示所有输入过的命令
  history
  # 显示最近10条
  history 10
  
  # 删除所有历史命令
  history -c
  
  # 执行历史的第890条命令
  !890
  ```

### 解压压缩类

* ubuntu可能自带unzip/zip 和 gunzip/gzip:

  ```bash
  # 看是否有输出
  which unzip
  which zip
  which gunzip
  which gzip
  ```

  

* gzip/gunzip : linux自带的压缩工具，后缀名.gz文件

  一个单纯的仅仅用来压缩的文件，不保留源文件

  ```bash
  gzip hello.txt
  
  # 解压
  gunzip hello.txt
  ```

* zip/unzip更常用的压缩命令

  保留源文件而且可以递归地压缩，即对目录的压缩

  zip [options] xxx.zip  

  unzip [options] xxx.zip

  ```bash
  # zip选项 ： -r 压缩目录
  
  # unzip选项：-d 指定解压后文件的存放目录
  
  # 直接解压到当前目录
  unzip 全新课件.zip
  
  # 在当前目录下新建一个名为ppt的文件夹，将所有内容压缩到了
  unzip -d ./ppt 全新课件.zip
  ```

* tar命令用法：

  注意：

  tar命令本身只是打包，而不是压缩，即扩展名.tar只表示该文件是一个打包文件，而不是压缩文件。只有加了一些参数如z，j等，才在打包的基础上进行压缩；

  建包、解包，分别有一个独一无二的参数，不能同时出现两个；建立压缩包、解压缩包分别有一个独一无二的参数，不能同时出现两个。

  参数出现的顺序和后面的参数要对应

  建包：对文件或者目录进行归档，**关键参数 c**；注意这里的归档只是将众多文件打包在一起，没有给他们压缩，文件后缀名是.tar，就是备份操作

  ```
  tar cvf 文件名.tar 要备份的文件或者目录
  
  c:create 表示建包
  verbose：冗长的，详细的，表示命令执行时会显示许多信息
  f：file，指定tar包的文件名
  ```

  解包：在当前目录中对包文件.tar进行解包，**关键参数x**;

  ```
  tar xvf 文件名.tar
  
  x:extraxt 提取、取出，表示解包的意思
  ```

  建立压缩包

  ```
  tar zcvf 文件名.tar.gz 要打包的文件或者目录
  tar jcvf 文件名.tar.gz 要打包的文件或者目录
  
  z表示在“cvf”打包的基础上进行压缩，j是另一种压缩命令；
  ```

  解压缩包

  ```
  tar zxvf 文件名.tar.gz [-C 指定解压到哪一个目录]
  ```

  

### 日期类

* 显示当前时间

  月份%m; 分钟%M；秒数%S；时间戳%s；时间戳：1970.1.1开始到当前时间所有的秒的计数；

  ```bash
  # 2023年 12月 17日 星期日 16:12:58 CST
  date
  
  # 加参数起到了截取参数的作用
  # 2023
  data +%Y
  
  # 2023-12
  date +%Y-%m
  
  # 1702801271
  date +%s
  ```

* 还可以加参数，显示以前或者之后的时间

* 查看日历

  ```bash
  # 显示当前月份
  cal
  
  # 显示当年一年日历
  cal -y
  
  # 显示上月、当月、下月日历；不能推广
  cal -
  ```


### 搜索查找类

* find [搜索范围] [选项]

  ```bash
  # 默认在当前路径查找
  fine -name filename
  
  # 在指定目录下找文件:一下搜出来很多
  find /home/zh -name info
  
  # *是通配符，长用来查找固定后缀名的文件
  find /home/zh/文档/ -name "*.txt"
  
  # 查找某一个用户的所有文件
  find /home -user zh
  
  ```

* locate: 找出所有包含关键词的路径和文件

  在事先建立的locate数据库中查找，无需遍历整个硬盘，查找速度快

  这个数据库一般每天自动更新一次，也可以手动更新：updatedb

  ```bash
  # 查找时一般先更新
  updatedb
  locate vnote
  
  # 输出
  /home/zh/.config/VNote/VNote/vnotex.json
  /home/zh/.config/VNote/VNote/vnotex.log
  /home/zh/.local/share/VNote/VNote/vnotex.json
  /home/zh/.local/share/VNote/VNote/docs/en/about_vnotex.txt
  /home/zh/.local/share/VNote/VNote/docs/zh_CN/about_vnotex.txt
  /home/zh/software/vnote-linux-x64_v3.17.0.AppImage
  /home/zh/下载/vnote-linux-x64_v3.17.0.zip
  
  ```

* which :查看命令在哪个位置，有输出则代表有这个命令

  ```bash
  which ls
  /usr/bin/ls
  
  which which
  /usr/bin/which
  ```

* grep过滤查找以及 “ | ”符号

  grep 选项 查找内容 源文件;在某个文件中查找关键字

  ```bash
  # -n显示行号
  
  # 查找initial-setup-ks.cfg文件中boot出现的所有行
  grep -n boot initial-setup-ks.cfg
  ```

  |：管道操作符，表示将前一个命令的处理结果输出传递给后面的命令处理

  ```
  # 在列出的所有文件中，找出后缀名为.sh的
  ls | grep .sh
  
  ```

  
