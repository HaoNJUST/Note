# Git使用

### 一、本地仓库命令相关

**git的文件状态**

```
工作区：已修改
暂存区：已暂存
Git目录：已提交
```

**快捷键**：

```
清屏：Ctrl+L   clear+E
```

**初始化本地库**

```
在自己的本地项目中，右击空白处打开git的命令行窗口，然后输入：此操作相当于给git操作这个目录下文件的权限
git init
```

**本地库内的查看、提交**

```
git status	查看本地库状态
git add hello.txt 提交到缓存区
git commit -m "第一次提交" hello.txt  将文件提交到本地库,-m是为了输入日志信息

git reflog //ref+log查看日志信息，显示如下：f45792e是版本号
f45792e (HEAD -> master) HEAD@{0}: commit (initial): 第一次提交

git log //查看更详细的日志信息
```

**取消暂存文件**

```
git r
```

**穿梭到历史版本**（在分支内部）：git的本地项目中，只会存一个版本，低层通过指针实现，可以在各个版本之间切换。

```
先查看所有版本日志
git reflog
复制要穿梭的版本号
git reset --hard 版本号
此时可以用cat 文件名查看当前版本的内容
```

**创建一个分支：**

* 新创建的 Git 分支最初是从原始分支（通常是主分支，比如 `main` 或 `master`）的当前状态派生出来的。因此，新分支在创建时会与原始分支的状态一致。

```
当前仓库中存在的分支,并且会在当前使用的分支前面标记一个星号（*)
git branch
查看远程仓库中的分支:
git branch -r
查看所有分支，并显示当前在哪个分支
git branch -v
创建分支:增加一个热修分支
git branch hot-fix
再次查看发现多了一个分支
切换到其他分支
git checkout 分支名
git checkout hot-fix
然后就可以在这个分支上，修改之前的文件，并添加到缓存区，提交到本地库
```

**删除本地仓库的分支**

要删除 Git 中的分支，可以使用 `git branch -d` 命令。以下是具体的步骤：

1. 首先，确保你不在要删除的分支上。你可以切换到另一个分支，比如 `main` 分支：

   ```bash
   git checkout main
   ```

2. 然后，使用 `git branch -d` 命令来删除分支。比如，如果你要删除名为 `feature` 的分支：

   ```bash
   git branch -d feature
   ```

   如果分支包含了未合并的更改，Git 会给出警告并阻止删除。如果你确定要强制删除分支，可以使用 `-D` 选项：

   ```bash
   git branch -D feature
   ```

   这将强制删除分支，即使它包含了未合并的更改。

当分支被删除后，相关的提交历史并不会丢失，它们仍然会保留在 Git 的提交历史中。

**创建分支时出现的问题**

* 本地库刚初始化之后，默认的是master分支，输入git branch main想新建立一个分支，但是报错：fatal: not a valid object name: 'master'。
* 这是因为此时本地库其实还没有master分支，要先commit一次才会真正建立master分支，此时就可以新建立分支了。
* 建立新的分支之后，再把这个master分支删除

**合并分支：**

```
git merge 分支名 ：将指定分支合并到当前分支
比如当前指向的是masetr分支，然后使用命令 :
git merge hot-fix
合并之后，两个分支就是一模一样的了
```

 **合并冲突：**

两个分支在同一个文件的同一个位置有两套不同的修改，git无法决定使用哪一个。

当在一个分支（hot-fix)修改某个文件之后，添加到缓存区、提交到本地库；然后再另一个分支也对文件进行了修改（而且这两处修改不一样），然后也添加到缓存区、提交到本地库；这时想把这两个库合并就会出现冲突，需要自己打开这个文件，手动决定最终修改的样子。

```
手动修改之后，依然需要添加到缓存区、提交到本地库，这时候注意，提交到本地库时，命令后面不要再添加文件名；
git commit -m "merge test" 
而不是：
git commit -m "merge test"  hello.txt
```

手动修改之后，把新的文件都重新添加到暂存区，提交到本地库，然后再合并。

而且合并分支，指挥修改合并的那个分支，不会修改合并过来的分支。

### 二、远程仓库命令相关

**创建远程仓库**：

* push 推送：将本地库的代码推送到远程库中；
* clone 克隆：将远程库的代码完整地下载到自己的本地库中；
* pull  拉取：将远程库的代码（通常是更新过的），再拉取到自己的本地库，让自己的本地库也有最新版本；

```bash
在github上，创建的仓库会生成一个远程库链接；创建远程库别名：链接太长，给链接起别名；远程仓库的别名和远程仓库的名字保持一致，不然容易忘记；
git remote add 别名 链接

使用ssh别名，http有时候连不上：
git remote add origin_ssh git@github.com:your-username/your-repository.git

#git remote add git-demo https://github.com/HaoNJUST/demo01.git

查看给远程库的（本地库）的别名；
git remote -v
```

**重新给远程仓库命名**

```
git remote rename old_name new_name
```

**将本地仓库所有修改过的文件提交到暂存区，再提交到工作区**

```

git status
# 文件是红色

git add .
# 再次git status，文件变绿色

git commit -m '提交说明'
# 再次再次git status， 显示nothing to commit, working tree clean

```

**将本地库项目推送到远程库中**：按分支推送，需要指定推送哪个分支。

```
git push 目标远程库的别名（或者链接） 本地库的分支
git push git-demo master
```

可以将本地的master分支的内容提交到远程库的main分支中

```
git push demo1 master:main
```

**拉取远程库到本地**：远程库的代码被更新了，为了保证自己的本地库和远程库内容一致

```
git pull 目标远程库的别名（或者链接） 远程库的分支
```

**克隆远程库到本地**

```
git clone 远程库的链接
完成了三件事：拉取代码、初始化本地库、创建别名；别名默认是o
```

### 三、创建仓库的注意事项

**一些问题:**

```bash
git的默认分支是master，github仓库的默认分支是main，所以等于提交的时候，远程仓库上有了两个分支，而且还合并不了。

所以想要远程仓库只有一个main分支的话，就在git里新建一个叫main的分支，删除原来的master分支。然后切换到这个分支，对文件进行提交到暂存区，工作区，然后推送到远程库。
。
```

上面的是笨蛋方法。

**设置git初始分支名称**：

配置全局初始分支的默认名称，这样在执行`git init`时，初始分支名就是自己配置的了

```bash
git config --global init.defaultBranch <defaultBranch>

#例
git config --global init.defaultBranch main

```

执行`git init`时指定初始分支名称

```bash
git init -b <branch-name> .
#例
git init -b main .

```

直接修改分支名称：(仓库已经初始化（未推送）)

```bash
git branch -m <oldbranch> <newbranch>
#例
git branch -m master main

```

仓库已经推送

```bash
# Step1 修改本地分支名称：
git branch -m <oldbranch> <newbranch>
#例
git branch -m master main

# Step2 删除远程分支：
git push origin --delete <branch-name>   
#例
git push origin --delete master
#要删除的分支可能是默认分支、受保护分支等，
#如果删除不成功，需要先在仓库管理平台设置，
#然后再执行命令。

# Step3 推送本地分支到远程仓库：
git push -u origin <branch-name>
#例
git push -u origin main

```

**删除本地仓库的分支**

要删除 Git 中的分支，可以使用 `git branch -d` 命令。以下是具体的步骤：

1. 首先，确保你不在要删除的分支上。你可以切换到另一个分支，比如 `main` 分支：

   ```bash
   git checkout main
   ```

2. 然后，使用 `git branch -d` 命令来删除分支。比如，如果你要删除名为 `feature` 的分支：

   ```bash
   git branch -d feature
   ```

   如果分支包含了未合并的更改，Git 会给出警告并阻止删除。如果你确定要强制删除分支，可以使用 `-D` 选项：

   ```bash
   git branch -D feature
   ```

   这将强制删除分支，即使它包含了未合并的更改。

当分支被删除后，相关的提交历史并不会丢失，它们仍然会保留在 Git 的提交历史中。

**上传.md文件**: 命名一个md文件为README，名字不要写错。一旦推送完成，你的 README.md 文件就会出现在 GitHub 仓库中，并且会自动显示为项目的 README 文件。如果你已经有一个 README.md 文件，它将会被替换为你刚刚推送的文件。

```
echo "# demo2" >> README.md
  git init
  git add README.md
  git commit -m "first commit"
  git branch -M main
  git remote add origin https://github.com/HaoNJUST/demo2.git
  git push -u origin main
  
  
  在 `git push -u origin main` 命令中，`-u` 参数的含义是将远程分支和本地分支关联起来，并设置远程分支为默认上游分支。

具体来说，`-u` 参数的作用是将本地的 `main` 分支推送到远程仓库的 `origin` 主机上，并且将本地的 `main` 分支与远程的 `main` 分支关联起来。这样一来，以后在使用 `git push` 命令时，Git 就会默认将本地的 `main` 分支推送到远程的 `main` 分支，而不需要再指定远程分支的名称。

因此，使用 `-u` 参数可以简化后续的推送操作，使得 Git 能够更智能地处理默认的推送行为。
```

### 四、为一台主机配置ssh免密登录

常用命令：

```
# 配置基本信息
git config --global user.name "Junk Chen"
git config --global user.email junkchen@vip.qq.com


git config user.name

git config --list

```

==windows==

1.确保该主机没有ssh配置：C:\Users\22989目录下没有.ssh文件，在该目录下打开git，然后生成ssh key

```
输入：ssh-keygen -t rsa -C "xxx@xxx.com"
然后敲三次回车，然后发现该目录下.ssh文件夹出来了，里面的 id_rsa是私钥，id_rsa.pub是公钥
```

2.拿到公钥，先确保自己在.ssh目录下，可以使用 cd ~/.ssh 进入这个目录，也可以试试 cd .ssh

```
cat id_rsa.pub
复制输出内容
```

3.把当前主机的公钥复制到你的github账号上去

* 点击头像-settings-SSH and GPG kets,然后 右侧绿色按钮 New SSH key
* 为这个密钥起个名字，可以起这台主机的名字，然后把刚刚复制的公钥粘贴进来

4.成功之后Push和clone代码，使用的连接是ssh了，不再是http了。

**ubuntu**

1.先进入.ssh, 然后输出ls，看是否有文件

```
cd .ssh
ls
```

2.如果有id_rsa  id_rsa.pub就说明配置过了。

没有的话，执行命令：

```
ssh-keygen -t rsa -C "你的邮箱"
# 一直按回车
```

3.得到id_rsa.pub里面的内容，复制到新建的key中

```
cat id_rsa.pub
复制输出内容
```

4.和windows一样

### 五、多个本地仓库与远程仓库连接的问题

1.在本地仓库和远程仓库连接时，必须先要进行同步，否则这个本地库将无法推送内容去远程库。

 * 这种情况通常发生在多人协作开发的情况下，其中一个人已经将他们的代码推送到了远程仓库，而你的本地代码落后于远程代码。为了避免冲突，Git 不允许你直接将本地代码推送到远程仓库。相反，你需要先将远程代码合并到你的本地分支中，然后再将合并后的代码推送到远程仓库。

```
git pull git@github.com:HaoNJUST/demo2.git main
将远程仓库的main分支合并到本地所在的分支中；
```

2.执行上面这个命令之后可能会报错：'fatal: refusing to merge unrelated histories

* 解决方法：在其代码模块后加上这个参数，强制合并，忽视提交的历史，之后通过代码模块的不同进行合并即可

```
git pull git@github.com:HaoNJUST/demo2.git main --allow-unrelated-histories
强制合并，忽视提交的历史，
```

3.可以在本地Git仓库中同时为HTTPS和SSH连接设置别名

```bash
# 为HTTPS连接添加远程仓库别名
git remote add origin_https https://github.com/your-username/your-repository.git

# 为SSH连接添加远程仓库别名
git remote add origin_ssh git@github.com:your-username/your-repository.git
```

在 Git 中，远程仓库别名是唯一的，不允许两个不同类型（例如 HTTPS 和 SSH）的远程仓库共享相同的别名。

4.删除远程仓库的别名：为什么需要这步，因为最开始给http起了远程仓库的别名，但是http有时连接不上，所以重新给ssh起别名，想要名字和远程仓库名字一样，那就得删除原来的http的别名

```
git remote remove origin_https
或者
git remote rm origin_https
```

