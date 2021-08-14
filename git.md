#### git的工作流程

git 标准的工作流程是 工作区 --> 暂存区 --> git仓库 

### git 配置用户信息

~~~
git config --global user.name "longtian"  //配置用户名
git config --global user.email "778707060@qq.com"  //配置邮箱
~~~

### 检查配置信息

~~~
git config --list --global   //查看所有的全局配置项
//查看制定的全局配置项
git config user.name
git config user.email
~~~

### 获取帮助信息

~~~
git help <verb>
git config -h //简易的手册信息
~~~

### 获取git仓库的两种方式

#### 将尚未进行版本的控制的本地目录转换为git仓库

- 在项目目录中，通过鼠标右键 打开 git 命令面板
- 执行 git init 命令将当前的目录转化为 git 仓库，则会自动生成一个 .git 的隐藏文件。

#### 从其他服务器克隆一个已经存在的git仓库

### 工作区文件的4中状态

- 未跟踪（untracked）：不被git 所管理的文件
- 未修改（unmodified）：工作区中文件的内容和git仓库中文件的内容保持一致
- 已修改（modified）：工作区文件的内容好git仓库中文件的内容不一致
- 已暂存（staged）：工作区中被修改的文件被放到暂存区，准备将修改后的文件保存到 git 仓库中
- git 操作的中级结果:让工作区中的文件都处于 “未修改” 的状态

~~~
git status  //查看文件处于什么状态
git status -s  //已精简的形式显示状态 ?? 表示 未跟踪的文件  A 表示 暂存区 红色M 表示已修改未放入暂存区  绿色M表示已修改并放入了暂存区
~~~

### git 基本操作指令

~~~
git add 文件名    //跟踪一个文件  将文件添加到暂存区
git add .     //一次性增加所有的新增和修改到暂存区
git commit -m "文件提交的描述信息"  
~~~

#### 撤销对文件的修改

~~~
git checkout -- 文件名   // 撤销的本质是用 git 仓库中保存的文件，覆盖工作区中指定的文件
~~~

#### 取消暂存的文件

~~~
git reset HEAD 要移除的文件名
git reset HEAD .   //移除所有的文件
~~~

#### 跳过使用暂存区 （工作区 --> git仓库）

~~~
git commit -a -m “描述的消息”
~~~

#### 移除文件

- 从git 仓库和工作区同时移除对应的文件，相当于完全删除

  ~~~
  git rm -f 文件名
  ~~~

- 只从 git  仓库中移除指定的文件，但是保留工作区中对于的文件

  ~~~
  git rm --cached 文件名
  ~~~

#### 忽略文件

一般我们总会有些文件无需纳入git的管理，也不希望他们总出现在未跟踪的文件列表，在这种情况下，我们可以创建一个名为 .gitignore的配置文件，列出要忽略的匹配模式。

文件.gitignore 的格式规范如下：

- 以 # 开头的是注释
- 以 /  结尾的是目录
- 以 / 开头防止递归
- 以 ！ 开头表示取反
- 可以使用 glob 模式进行文件和文件夹的匹配（glob指简化了的正则表达式）
  - glob模式介绍
    1. 星号 * 匹配零个或者多个任意字符
    2. [abc] 匹配任何一个列在方括号中的字符（此案例匹配一个a或匹配一个b或匹配一个c）
    3. 问号 ？ 只匹配一个任意字符
    4. 在方括号中使用短划线分割两个字符，表示所有的这两个字符范围内的都可以匹配（比如[0-9]表示匹配所有0到9的数字
    5. 两个星号 ** 表示匹配任意中间目录（比如 a/**/z 可以匹配a/z、 a/b/z 或者a/b/c/z 等）

- .gitignore 文件例子

  ~~~php
  #忽略所有的 .a 文件
  *.a
  
  #但是跟踪所有的 lib.a ，即使你在前面忽略了 .a 文件
  !lib.a
  
  #只忽略当前目录下的 todo 文件，而不忽略 subdir/todo 
  /todo
  
  #忽略任何目录下名为 build 的文件
  build/
  
  #忽略 doc/notes.txt ，但不忽略 doc/server/arch.txt
  doc/*.txt
  
  #忽略 doc/ 目录及其所有子目录下的 .pdf 文件
  doc/**/*.pdf
  ~~~

### 查看提交的历史

如果希望回顾项目的提交历史，可以使用 git log 这个命令

~~~
#按时间先后顺序列出所有的提交历史，最近的提交排在最上面
git log
#只展示最新的两条提交历史，数字可以按需要填写
git log -2
#在一行上展示最近的两条提交历史信息
git log -2 --pretty=oneline
#在一行上展示最近两条提交历史的信息，并自定义输出的格式
# %h 提交的简写哈希值， %an 作者名字  %ar 作者修订日期，按多久以前的方式显示 %s 提交说明
git log -2 --pretty=format:"%h | %an | %ar | %s"
~~~

#### 回退到指定的版本

~~~
#在一行上展示所有的提交历史
git log --pretty=oneline

#找到需要回退的版本的哈希值，根据哈希值退回到指定的版本
git reset --hard 哈希值

#如果回退到旧版本后，还想回到最新的版本，需要使用这个来查看操作历史
git reflog --pretty=oneline

#回到比较新的版本
git reset --hard 哈希值
~~~

#### 远程仓库访问的方式

- https：零配置，但是每次访问仓库时，需要重复书 github账号和密码才能访问

  ~~~
  git remote add origin https://github.com/xxxx/xxx
  git branch -M main
  git push -u origin main
  ~~~

  - 使用该方式密码需要使用 token ，token我们可以在github 中设置
    - 设置步骤settings->developer setting -> personal access tokens中，密码就使用我们该 token 即可

- SSH：需要进行额外的配置，但是配置成功后，每次访问仓库时，不需要重复输入github的账号和密码，在实际开发中一般使用这种方式

  1. 生成

     - 打开 git 命令面板
     - 输入 ssh-keygen -t  rsa -b 4096 -C "你的注册邮箱"
     - 连续敲击3次回车，即可在 C:\Users\用户名文件夹\.ssh 目录中生成 id_rsa和id_rsa.pub两个文件（路径一般在面板中有提示）

  2. 配置

     - 使用记事本打开生成的id_rsa.pub 文件，复制里面的文本内容
     - 在github中点击 settings->SSH and GPG Keys -> New SSH key
     - 将id_rsa.pub文件中的内容，粘贴到key对应的文本框中

  3. 检测是否配置成功

     ~~~
     #git面板中输入执行
     ssh -T git@github.com
     # 回车后 出现提示 输入 yes
     #回车 出现 successfully 说明成功
     ~~~

  4. 通过 ssh 上传

     ~~~
     git remote add origin git@github.com:longtian1911/zixuebiji.git
     // 如果执行上面代码报错 可以使用  remote rm origin  删除远程git仓库
     git branch -M main
     git push -u origin main
     ~~~

     









































