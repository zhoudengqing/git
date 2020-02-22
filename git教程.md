# 1. ubuntu下可以使用apt-get的命令来安装git  
- $ sudo apt-get install git  
  
# 2. git安装完成后要配置用户名和邮件地址  
- git config --global user.name   "zhoudengqing"
- git config --global user.email "1006783171@qq.com"  
  
# 3. 配置完成后可以通过git config --list查看配置信息  
- root@ubuntu:~# git config --list  
- user.name=zhoudengqing  
- user.email=1006783171@qq.com  
- global指定了配置信息作用与整个系统，如果不针对整个系统，可以不加--global选项  
  
# 4. git帮助信息  
- git help <verb>  
- git <verb> --help  
- man git-<verb>  
- 例如，要想获得 config 命令的手册，执行git help config  
- 当然，如果你遇到问题也可以查看git的官方文档，https://git-scm.com/book/zh/v2  

# 5. git仓库创建步骤  
- mkdir git.test (创建一个目录)  
- cd git.test (进入创建的目录)  
- git init (创建一个空的仓库)  
 该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。  
 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪  
 - 有了仓库之后，我们就可以使用git add和git commit向仓库添加要跟踪的文件，和提交修改的内容了  
# 6. git命令
- ## 6.1 git add
  - [] 在仓库里刚新建的文件是不会被跟踪起来的，比如我们使用git status就能查看到文件的状态，需要使用git add才可以
    ```
    root@ubuntu:/home/zhoudq/git.test# ls
    a.c  b.c  c.c  d.c
    root@ubuntu:/home/zhoudq/git.test# git status
    On branch master
    Initial commit
    Untracked files:
    (use "git add <file>..." to include in what will be committed)
        a.c
        b.c
        c.c
        d.c
    nothing added to commit but untracked files present (use "git add" to track)
    root@ubuntu:/home/zhoudq/git.test# 
    ```
  - [] 基本用法：
    git add <path>
    通过git add <path>的方式把path目录下的所有文件添加到git的暂存区，当然这些文件不包含已经被删除的文件。  
    示例：  
    $ git add .  # 将所有修改添加到暂存区  
    $ git add *.c # 将以.cpp结尾的文件的所有修改添加到暂存区  
    $ git add a.c #将a.c的所有修改添加到暂存区  
  - [] Git add 是把文件添加到暂存区，那如果想从暂存区删除呢？可以使用 git rm –cached 把文件从暂存区里移除  
    ```
        root@ubuntu:/home/zhoudq/git.test# git status
    On branch master

    Initial commit

    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)

        new file:   a.c
        new file:   b.c
        new file:   c.c
        new file:   d.c

    root@ubuntu:/home/zhoudq/git.test# git rm --cached a.c 
    rm 'a.c'
    root@ubuntu:/home/zhoudq/git.test# git status
    On branch master

    Initial commit

    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)

        new file:   b.c
        new file:   c.c
        new file:   d.c

    Untracked files:
    (use "git add <file>..." to include in what will be committed)

        a.c

    root@ubuntu:/home/zhoudq/git.test# 
    ```
- ## 6.2 git commit
    - [] git add 只是把文件添加到暂存区而已，并没有真正跟踪起来，需要使用git commit命令提交到仓库才能真正被git跟踪记录
    ```
        root@ubuntu:/home/zhoudq/git.test# git commit -a -m "添加a.c b.c c.c d.c四个文件"
        [master (root-commit) 29560c7] 添加a.c b.c c.c d.c四个文件
        4 files changed, 0 insertions(+), 0 deletions(-)
        create mode 100644 a.c
        create mode 100644 b.c
        create mode 100644 c.c
        create mode 100644 d.c
        root@ubuntu:/home/zhoudq/git.test# git status
        On branch master
        nothing to commit, working directory clean
        root@ubuntu:/home/zhoudq/git.test# 
    ```
- ## 6.3 搭建中央服务器
  - [] 创建git账号和git用户组  
    $ sudo adduser git  #添加git用户  
    $ sudo passwd git   #添加git的密码  
    $ sudo groupadd git #添加git用户组  
    $ sudo usermod -G git git #添加git用户到git用户组
  - [] 创建git仓库  
    $ cd /home/zhoudq     #此目录下存放git的仓库  
    $ mkdir git-test.git # 创建git-test.git目录  
    $ cd git-test.git  
    $ git init --bare # bare选项指示该仓库为裸仓库，没有工作区  
    $ sudo chown -R git:git /home/zhoudq/git-test.git # 修改权限为git用户
- ## 6.4 克隆仓库
  - [] windows上打开git bash here
    ```
    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test
    $ git clone git@192.168.31.111:/home/zhoudq/git-test.git
    Cloning into 'git-test'...
    The authenticity of host '192.168.31.111 (192.168.31.111)' can't be established.
    ECDSA key fingerprint is SHA256:ybtukjdxUzvZQOYk6b92rDdp91SsZtcNLS+/SNsfEn0.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '192.168.31.111' (ECDSA) to the list of known hosts.
    git@192.168.31.111's password:
    warning: You appear to have cloned an empty repository.

    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test
    $ ls
    git-test/

    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test

    ```
- ## 6.5 添加文件并推送到中央服务器
    ```
        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
        $ touch.exe c.c d.c

        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
        $ ls
        a.c  b.c  c.c  d.c

        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
        $ git status
        On branch master
        Your branch is up to date with 'origin/master'.

        Untracked files:
        (use "git add <file>..." to include in what will be committed)
                c.c
                d.c

        nothing added to commit but untracked files present (use "git add" to track)

        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
        $ git add c.c d.c

        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
        $ git commit -a -m '添加c.c d.c两个文件'
        [master e574f83] 添加c.c d.c两个文件
        2 files changed, 0 insertions(+), 0 deletions(-)
        create mode 100644 c.c
        create mode 100644 d.c

        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
        $ git push
        git@192.168.31.111's password:
        Enumerating objects: 3, done.
        Counting objects: 100% (3/3), done.
        Delta compression using up to 8 threads
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (2/2), 262 bytes | 87.00 KiB/s, done.
        Total 2 (delta 0), reused 0 (delta 0)
        To 192.168.31.111:/home/zhoudq/git-test.git
        d4639c2..e574f83  master -> master

        10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)

    ```
- ## 6.6 git 工作流程
    - [] git的工作流程一般是这样的：  
    1、在工作目录中添加、修改文件；  
    2、将需要进行版本管理的文件add到暂存区域；  
    3、将暂存区域的文件commit到git仓库；  
    4、本地的修改push到远程仓库，如果失败则执行第5步  
    5、git pull将远程仓库的修改拉取到本地，如果有冲突需要修改冲突。回到第三步  
    因此，git管理的文件有三种状态：已修改（modified）,已暂存（staged）,已提交(committed)
- ## 6.7 git commit --amend
    - [] 把此次提交追加到上一次的commit内容里
    ```
    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
    $ touch.exe readm.md

    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
    $ git add readm.md

    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
    $ git commit --amend
    [master 993aa57] 添加e.c文件
    Date: Wed Feb 12 11:24:42 2020 +0800
    2 files changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 e.c
    create mode 100644 readm.md

    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)

    ```
- ## 6.8 git commit的格式
    ```
    Angular 团队的规范
    message的格式：
        <type>(<scope>): <subject>
        <BLANK LINE> 
        <body> 
        <BLANK LINE> 
        <footer>
    Type指：
    feat: 新特性
    fix: 修改问题
    refactor: 代码重构
    docs: 文档修改
    style: 代码格式修改, 注意不是 css 修改
    test: 测试用例修改
    chore: 其他修改, 比如构建流程, 依赖管理.
    scope: commit 影响的范围，即影响的模块或者组件，比如: route, component, utils, build... 
    subject: commit 的概述, 建议符合 50/72 formatting 
    body: commit 具体修改内容, 可以分为多行, 建议符合 50/72 formatting 
    footer: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接，或者feature等等其余的信息.
    使用git cmmit模板来规范提交
    1. 在~/.gitconfig文件里添加
       [commit]
              template=~/.gitmessage
    2. 添加~/.gitmessage文件
    # head: <type>(<scope>): <subject>
    # - type: feat, fix, docs, style, refactor, test, chore
    # - scope: can be empty (eg. if the change is a global or difficult to assign to a single component)
    # - subject: start with verb (such as 'change'), 50-character line
    #
    # body: 72-character wrapped. This should answer:
    # * Why was this change necessary?
    # * How does it address the problem?
    # * Are there any side effects?
    #
    # footer: 
    # - Include a link to the ticket, if any.
    # - BREAKING CHANGE
    #
    ```
- ## 6.9 git的commitizen
    ```
    1. 下载对应版本的nodejs包，并安装
        https://nodejs.org/en/download/
    2. 使用 npm 工具进行全局安装，
        npm install commitizen -g
    3. 然后在项目目录里，运行下面命令，使其支持 Angular 的 Commit message 格式，
        commitizen init cz-conventional-changelog --save --save-exact
        以后，凡是用到 git commit 命令，一律改用 git cz ，这时候就会出现选项，来生成符合规范的 commit message 。
    4. 如果我们希望每个使用 git 的项目都遵循这个标准，可以使用下面命令进行全局设置。
        安装 cz-conventional-changelog ，
        npm install -g cz-conventional-changelog
    5. 创建一个 .czrc 文件在你的 home 目录，并将 path 指向上面所安装的 commitizen 适配器，
        echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
    6. 现在我们可以在每个 git 项目中使用 git cz 提交我们的 commit message 了，当然我们还可以配置Commitlint做自动检测，检查不通过的可以拒绝提交，比较绝吧。
    7. 如果我们所有的commit信息都是按照这个格式填写的，在发布版本时，我们就可以使用以下命令生成changelog了
        conventional-changelog -p angular -i CHANGELOG.md -s
    ```
- ## 6.9 推送到远程分支
    ```
    git push命令用于将本地分支的更新，推送到远程主机。它的格式与git pull命令相仿。
        git push <远程主机名> <本地分支名>:<远程分支名>
    注意，分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。例如：
        git push origin master：refs/for/master
    如果省略远程分支名，则表示将本地分支推送与之存在”追踪关系”的远程分支(通常两者同名)，如果该远程分支不存在，则会被新建。
        git push origin master
    上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。
    如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。
        git push origin :master   # 等同于 git push origin --delete master
    上面命令表示删除origin主机的master分支。

    如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
        git push origin
    上面命令表示，将当前分支推送到origin主机的对应分支。

    如果当前分支只有一个追踪分支，那么主机名都可以省略。
        git push

    ```
- ## 6.9 git commit合并
    ```
    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
    $ git log --oneline
    993aa57 (HEAD -> master) 添加e.c文件
    e574f83 (origin/master) 添加c.c d.c两个文件
    d4639c2 添加a.c b.c两个文件

    10067@LAPTOP-22M95BJU MINGW64 ~/Desktop/git-test/git-test (master)
    $ git rebase -i d4639c2
    ```
    - [] 执行后会弹出界面
    ```
    pick e574f83 添加c.c d.c两个文件
    pick 993aa57 添加e.c文件

    # Rebase d4639c2..993aa57 onto d4639c2 (2 commands)
    #
    # Commands:
    # p, pick <commit> = use commit
    # r, reword <commit> = use commit, but edit the commit message
    # e, edit <commit> = use commit, but stop for amending
    # s, squash <commit> = use commit, but meld into previous commit
    # f, fixup <commit> = like "squash", but discard this commit's log message
    # x, exec <command> = run command (the rest of the line) using shell
    # b, break = stop here (continue rebase later with 'git rebase --continue')
    # d, drop <commit> = remove commit
    # l, label <label> = label current HEAD with a name
    # t, reset <label> = reset HEAD to a label
    # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
    # .       create a merge commit using the original merge commit's
    # .       message (or the oneline, if no original merge commit was
    # .       specified). Use -c <commit> to reword the commit message.
    #
    # These lines can be re-ordered; they are executed from top to bottom.
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    #
    # However, if you remove everything, the rebase will be aborted.
    #
    # Note that empty commits are commented out
    ```
    - [] 修改成如下界面内容
    ```
    pick e574f83 添加c.c d.c两个文件
    squash 993aa57 添加e.c文件
    ```
    - [] 保存退出后弹出如下界面,继续保存退出
    ```
    # This is a combination of 2 commits.
    # This is the 1st commit message:

    添加c.c d.c两个文件

    # This is the commit message #2:

    添加e.c文件

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    #
    # Date:      Wed Feb 12 10:57:28 2020 +0800
    #
    # interactive rebase in progress; onto d4639c2
    # Last commands done (2 commands done):
    #    pick e574f83 添加c.c d.c两个文件
    #    squash 993aa57 添加e.c文件
    # No commands remaining.
    # You are currently rebasing branch 'master' on 'd4639c2'.
    #
    # Changes to be committed:
    #       new file:   c.c
    #       new file:   d.c
    #       new file:   e.c
    #       new file:   readm.md
    #
    ```
- ## 6.9 修改你的提交
    - [] 调用git add README.md文件到暂存区，然
后调用git commit –-amend把当前暂存区里的内容合并到上一
次commit里，  
而且还可以修改上一次提交的message信息
    - [] 修改任意提交的message
    ```
    $ git log --oneline
    01db098 (HEAD -> master) readm.md添加内容world
    96d0770 添加c.c d.c两个文件 修改readm.md文件
    d4639c2 添加a.c b.c两个文件
    $ git rebase –i d4639c2 # 打算修改d4639c2之后所有commit 的message
    输入上面一条命令后，会弹出下文这个窗口，上半部分是一些pick命令，下半部分是一些提示内容，不过有意思的是此时的commit内容的排列和git log里的排列是反的，也就是倒序的。如果我们仅仅修改commit message，需要把打算修改的commit的对应pick命令修改为reword，然后保存。
    之后会弹出编辑commit message的内容，如果我们
    有多处commit需要被修改的话会多次弹出vim编辑窗口。
    如果在修改前所有的commit都已经push到远程仓库的话，我们需要使用git push --force强制推送到远程仓库。
    ```
    ```
    pick 96d0770 添加c.c d.c两个文件 修改readm.md文件
    pick 01db098 readm.md添加内容world

    # Rebase d4639c2..01db098 onto d4639c2 (2 commands)
    #
    # Commands:
    # p, pick <commit> = use commit
    # r, reword <commit> = use commit, but edit the commit message
    # e, edit <commit> = use commit, but stop for amending
    # s, squash <commit> = use commit, but meld into previous commit
    # f, fixup <commit> = like "squash", but discard this commit's log message
    # x, exec <command> = run command (the rest of the line) using shell
    # b, break = stop here (continue rebase later with 'git rebase --continue')
    # d, drop <commit> = remove commit
    # l, label <label> = label current HEAD with a name
    # t, reset <label> = reset HEAD to a label
    # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
    # .       create a merge commit using the original merge commit's
    # .       message (or the oneline, if no original merge commit was
    # .       specified). Use -c <commit> to reword the commit message.
    #
    # These lines can be re-ordered; they are executed from top to bottom.
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    #
    # However, if you remove everything, the rebase will be aborted.
    #
    # Note that empty commits are commented out

    ```
    ```
    上文仅仅说明了修改历史提交的message，但是如果在某个历史提交中少提交内容，比如上文某个源码文件里的内容修改不完整，提交很多天经过严格测试才发现，那么我们就可以使用git rebase的edit命令修改提交内容了。
        $ git rebase –i bdc6778948a 
    上面的命令后我们可以使用edit命令替换pick命令。
    保存后，此时git rebase会停止工作，以便我们可以编辑文件和commit message，修改并提交后，可以继续其它的edit命令。在这个过程中我们会使用到以下这两条命令
        $ git commit –-amend
        $ git rebse --continue    
    如果在修改前所有的commit都已经push到远程仓库的话，我们需要使用git push --force强制推送到远程仓库
    ```
- ## 6.10 查看commit的内容
    - [] git log
    - [] git log --oneline #每条日志显示一条
    - [] git log -length #显示前面的length条日志
    - [] git log -p #显示一些统计信息以及文件的改动内容和行信息
    - [] git log --stat  #显示提交的作者 日期 message 和文件内容统计信息
    - [] git shortlog  #显示每个author提交commit和多少条commit
    - [] 按日期
       $ git log --after="2018-7-1"    # 2018年7月1好之后的所有日志
       $ git log –-before="2014-7-1"
    - [] 按作者
       $ git log --author="Dounin"  
    - [] 按照提交信息
       $ git log --grep=“issue”  # 按照提交本中是否包含issue的日志
    - [] 按文件
       $ git log -- ./src/http/modules/ngx_http_xslt_filter_module.c
    - [] 按照内容
       $ git log -S “ngx_free” # 即所有文件中包含了 ngx_free字符串的修改
    - [] 按照范围 
       $ git log <since>..<until> # 比如 git log master..feature这可以显示出自从master分支fork之后，feature分支上所有的提交
    - [] git show commit-id  #显示commit-id的提交内容，包括所有文件的修改信息
- ## 6.11 版本回退
  - 尚未commit的
    - [] git checkout命令就是从本地仓库中或暂存区检出文件，并且覆盖工作目录的内容。比如：  
    - $ git checkout branches/stable-1.14  # 检出到branches/stable-1.14分支上，即用1.14分支的内容覆盖了工作区所有内容  
    $ git checkout 9bfbacdd  # 检出到9bfbacdd（commit id）上，即用这个commit 内容覆盖了工作区所有内容  
    $ git checkout main.cpp # 从暂存区中检出内容，并且覆盖main.cpp文件内容，级尚未添加到暂存区的修改会被丢弃掉
  - 已经commit尚未push到remote仓库
    - 使用git reset(尽量少用)命令来做，reset参数如下意思：  
    --soft – 缓存区和工作目录都不会被改变  
    --mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影  
    --hard – 缓存区和工作目录都同步到你指定的提交  
    git reset --hard HEAD~{n}就是把HEAD指针回退n个版本（commit），并且使用该commit的内容覆盖掉工作区的内容，  
    即丢弃了前面n个commit的修改和当前工作区的修改
  - 已经提交到remote仓库
    - 如果没有其它同事基于我们的commit做修改的话，使用git revert的方法是比较好的，可能也有人认为为什么不能使用git reset呢？  
    因为git reset会删掉commit记录，所以并不是一种良好的做法
    - git revert d061cb3 #  因为4bff67b是晚于d061cb3的，如果这两个修改的内容有依赖，是会有冲突的，当然如果#  想取消这次回退可以使用，git revert --abort  
    fix conflict  # 手动去解决冲突  
    git commit  # 然后提交，此时使用git log会发现原理的git commit记录还在，但是增加了一  
    revert的记录  
    git push    # 推送至远程库
    - 如果有其它同事基于我们的commit做修改的话，我们回退版本的时候不想把他的提交给回退掉  
    我想回滚到5ff5433b这里，但是其它人有个提交我不能回滚掉，要保存他的代码，  
    而且如果有多个同事的修改在在5ff5433b之后的，那怎么办呢？  
    比较好的做法就是从5ff5433b拉取一个新的分支（分支名是reset_to_5ff5433b）,  
    因为这个新分支不包含我要回滚的代码，此时我们可以把其它同事的修改手动合并到reset_to_5ff5433b分支，  
    接着切换到master分支上，使用git merge reset_to_5ff5433b去合并分支到master上
    $ git checkout -b reset_to_5ff5433b 5ff5433bd1fe4  # 从5ff5433bd1fe4处创建分支，即代码是5ff5433bd1fe4处的代码  
    $ git show 9d531db98276 # 查看master分支上的其它同事的提交（比如KING），把他的修改在新分支上再修改一遍  
    $ git commit –a –m “reverted 5ff5433b”   
    $ git checkout master # 切换到master分支  
    $ git merge reset_to_5ff5433b  
    $ git push #推送到远程仓库
- ## 6.12 删除不需要的文件
    - git rm -rf timer.cpp timer.h  
    git commit –m “remove timer mudlue”  
    git push
- ## 6.13 查看指定文件的修改
    - git log --oneline filename   #显示文件的所有修改记录
    - git log –p filename  # 显示所有commit的修改
    - git show commit-id filename   # 显示某个commit里文件的修改
    - git diff filename   # 查看本地对某个文件做了那些具体修改
    - git diff commit-id filename   # 显示与某个commit间所有的差异，commit-id可以替换成HEAD，比如HEAD~2
    - git diff commit-id1 commit-id2   # 显示两个commit所有的差异
- ## 6.14 多个客户端之间的同步
    - 有同事A和B，同事A机器的IP是192.168.2.128，同事B机器的IP是192.168.2.129，现在同事A和同事B想项目同步。此时我们可以使用ssh协议同步，在同事A的机器上：  
    git remote add ubuntu_3 ssh://zhoudengqing@192.168.2.129/home/zhoudq/git-test.git  
    即ubuntu_3是远程机器的名字，可以任意取，但是不能再是origin了，因为origin已经用于中央仓库了，  
    192.168.2.129是同事B的机器IP地址，/home/zhoudq/git-test.git是同事B的repo的绝对路径。  
    这样我们可以使用git pull和git push去推送代码了，比如在同事B机器上commit一些内容，然后在同事A上使用：  
    git pull ubuntu_3 master    # 拉取远程ubuntu_3上的master分支代码  
    git push ubuntu_3 master    # 把本地的commit推送到远程ubuntu_3上的master，但是不幸的是                                push的时候出现了下图所示的  
                                # error信息，原因是同事B的仓库是不是一个裸仓库，它是有工作区间的，那么我们push的结果  
                                # 是不会反应在工作区间的，也即在远程仓库的目录下对应的文件还是之前的内容。  
                                # 此时必须在同事B的机器上的.git/config文件里添加：
                                            [receive]
                                                    denyCurrentBranch = ignore  
                                # 此时在同事B的机器上还看不到内容，但是可以看到commit记录了，需使用git reset --hard #  才能看到内容
- ## 6.15 处理突发事件
    - 现在我要切到master分支上去修改BUG了，那在切换到master分支前我们可以暂存修改，即： 
            git stash   #  暂存修改  
            git stash pop  # 从缓存里取出修改  
    调用git stash后就可以使用git checkout master分支上去修复bug了，修复完了之后再git checkout FT-12345后git stash pop
    - $ git stash   # 将工作区的修改保存到缓存区，默然取名为：
        WIP on <branch_name> ： <latest_commit_id> <latest_commit_message>  
        $ git stash save <name>   # 将工作区的修改保存到缓存区，且取名为name  
        $ git stash pop    # 取出缓存区栈顶（即最近一次）的内容，并且会删除此次pop的内容  
        $ git stash list   # 查看缓存里所有存储的修改  
        $ git stash apply stash@{X} #  取出stash里的内容，X为序号，但是不会删除stash@{X}  
        $ git stash drop stash@{X}  # 删除stash@{X}  
        $ git stash clear    # 删除缓存区里所有的记录
- ## 6.16 指定不需要git管理的文件
    - Git使用一个.gitignore文件来定义规则，用于忽略那些文件不提交到Git中去。  
    - .gitignore可以提交到远程仓库中，这样这些规则就可以共享给所有的开发人员了，在.gitignore文件中每一行都定义了忽略的规则，比如：
        *.[oa]
        *~  
    第一行就是表示忽略所有的以.o或者.a结尾的文件，第二行表示忽略所有的以“~”结尾的文件
    - git规则
      - 1）空格不匹配任意文件，可作为分隔符，可用反斜杠转义  
        2）以“＃”开头的行都会被 Git 忽略。即#开头的文件标识注释，可以使用反斜杠进行转义。  
        3）可以使用标准的glob模式匹配。所谓的glob模式是指shell所使用的简化了的正则表达式。  
        4）以斜杠“/”开头表示目录；“/”结束的模式只匹配文件夹以及在该文件夹路径下的内容，但是不匹配该文件；  
        “/”开始的模式匹配项目跟目录；如果一个模式不包含斜杠，则它匹配相对于当前 .gitignore 文件路径的内容，  
        如果该模式不在 .gitignore 文件中，则相对于项目根目录。比如：  
        /TODO            表示仅仅忽略repo跟目录下的TODO文件，不包含子目录下的TODO文件  
        /build/          表示忽略整个build目录及其下的所有文件  
        bin/             表示忽略当前目录下的bin目录及其下所有的文件，bin文件不会被忽略  
        build.sh         表示忽略当前目录下的build.sh文件5）以星号“*”通配多个字符，即匹配多  任意字符；使用两个星号“**” 表示匹配任意中间目录。比如：  
        doc/*.txt        表示忽略当前doc目录下的所有的以.txt结尾的文件，但是doc/designarch.txt文件不会被忽略掉  
        /*.c             表示忽略根目录下的所有的.c文件，  
        build/*.obj      表示忽略build目录下的所有.obj文件，但是build/http/http_core.obj不会忽略掉，  
        build/**/*.obj   表示忽略build目录，及其子目录，及其子目录的子目录和子子目录等等下的所有.obj文件。  
        **/*.a           表示忽略所有目录下的.a文件, *.a  
        /bin/*           表示忽略/bin/下的全部文件，不包含/bin/rtmp/hello文件  
        bin/*            表示忽略所有文件路径带有bin字符串的文件，比如/bin/, /bin/subdir/等目录下的内容都会被忽略  
        6）以问号"?"通配单个字符，即匹配一个任意字符；  
        7）以方括号"[]"包含单个字符的匹配列表，即匹配任何一个列在方括号中的字符。  
        比如[abc]表示要么匹配一个a，要么匹配一个b，要么匹配一个c；如果在方括号中使用短划线分隔两个字符，  
        表示所有在这两个字符范围内的都可以匹配。比如[0-9]表示匹配所有0到9的数字，[a-z]表示匹配任意的小写字母）  
        8）以叹号"!"表示不忽略(跟踪)匹配到的文件或目录，即要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。  
        需要特别注意的是：如果文件的父目录已经被前面的规则排除掉了，那么对这个文件用"!"规则是不起作用的。  
        也就是说"!"开头的模式表示否定，该文件将会再次被包含，如果排除了该文件的父级目录，则使用"!"也不会再次被包含。可以使用反斜杠进行转义。  
        下两条规则一起就是忽略掉/mtk/目录下所有的文件，但是不忽略/mtk/protocol.txt文件  
        /mtk/*  
        !/mtk/protocol.txt  
        下面这两条规则表示忽略掉/mtk/目录下所有内容，也忽略掉/mtk/protocol.txt文件  
        /mtk/  
        !/mtk/protocol.txt    
- ## 6.17 解决项目之间的依赖
    - 我们可以使用git的submodule来管理这些依赖，submodule允许父项目可以有很多的独立的
    Git子项目，  
    这些子项目可以单独提交/push/pull等管理，而且父项目中的提交不会影响到子项目。这有很多好处  
    1） 把子模块单独作为一个git项目，他们可以独立开发，他们的错误并不会影响到父项目  
    2） 团队分模块工作在不同git项目上，减少了代码提交的依赖，减少了很多更新操作
    - 添加submodule
        - git submodule add <url> <path>    # url是子模块的repo地址，path是在父项目中的存放路径
        - git submodule add git@106.52.144.219:lee/ringbuffer.git server/src/ringbuffer
        - 添加sub module后，会发现在父项目中出现一个.gitmodules文件，文件里的内容如上所示，记录名字，路径和url，然后我们需要把这几个文件添加到git版本管理里：  
            git add .  
            git commit –a –m “add bloomfilter and ringbuffer submodule”  
            git push
        - 一个项目仓库中并不真正保存了子模块的代码文件，他只是保存了这些配置信息而已，当我们clone仓库的时候，  
        并不会在clone出子模块的代码，仅仅是有一个文件目录而已，里面没有任何文件，有两种方法拉取子模块的代码：  
        1）git clone时加上 --recurse-submodules参数，如：git clone <url> --recurse-submodules  
        2）git pull后，执行git submodule update --init --recursive
        - 仓库都是由commit记录的，是由版本的，那么我们在使用submodule的时候，也是一样的，我们仅仅是指向他的特定版本，  
        比如我们使用git submodule status可以查看到指向的“版本”（某个commit记录），  
        如果我们在bloomfilter这个项目上由更新是不能自动反应到这个主repo上的，  
        bloomfilter已经更新到了a1532ce了，但是主repo的submodule并没有指向这里，那么也就不能自动“checkout”最新代码了。那如何做？
        - 子模块有新的提交了，我们如何更新？就以上文所述，我们的bloomfilter项目已经更新了，那如何更新0voice_im里这个submodule的版本？方法如下：  
        cd server/src/bloomfilter  
        git pull  
        git checkout commit-id  # 手动移动submodule的指针到某个commit-id上，然后我们在主repo上：git submodule status，可以看到 # submodule更新了(前面有个‘+’号)，此时我们应该提交这个更新，这样别人才能获取到这个更新操作  
        git commit server/src/bloomfilter –m “move bloomfilter submodule to a1532ce02”  # 提交对submodule的更新  
        git push     # 推送更新到远程仓库
        - 上文，我们更新了submodule的内容了，也把submodule的这个“指向”更新推送到了远程仓库了，那其它的开发人员如何“同步”这种更新呢？  
        git pull    # 拉取最新变化  
        git submodule update - -remote   #更新子模块为远程项目的最新版本
        - submodule让我们看起来以为是一个仓库，我们想当然的想在里面做一些修改，可是并非如此，我们cd到该子目录后，  
        使用git status会发现“HEAD detached at a1532ce”,这其实就告诉了我们，当前并没有处于一个分支上，并不能去提交修改，  
        当然我们可以使用git checkout master/dev的方式切换到某个分支上，做一些修改，然后提交submodule的更新，  
        尽管可以这么做，但是依然不建议大家如此。
- ## 6.18 备份仓库
    - 为了容灾，在大厂里都会对git的仓库进行备份，那如何做的呢？
    - 1）设置ssh的免密登录方式：  
        ssh-kengen –t rsa  # 以rsa算法生成密钥对  
        vim ~/.ssh/id_rsa.pub # 把id_rsa.pub的内容拷贝后放在git仓库的/root/.ssh/authorized_keys里  
        chmod 400 /root/.ssh/authorized_keys   # 在git服务器上设置文件的权限为400   
    - 2）书写以下脚本：我们使用ssh协议进行git clone， --mirror是拷贝镜像的意思（不能省掉，因为git仓库有很多的分支和tag信息）  
        ```
        giturl="root@192.168.31.111:/srv/"
        reslist=$(ssh root@192.168.31.111 "cd /srv ; ls")
        for res in ${reslist[@]};
        do
            cd ${res}
            git clone --mirror ssh://${giturl}${res}
        done
        ```
    - 3）通过crontab添加定时任务：  
        crontab –e   # 在定时任务中添加: 0 0 * * * sh /srv/backup_remote_git.sh，然后保存
        systemctl restart cron  # 重启cron服务，如果在centos是systemctl restart crond  
- ## 6.19 分支
    - git branch   # 查看分支  
    git branch develop  # 创建develop分支  
    git checkout –b feature/FT-123456  # 创建FT-123456的一个feature分支  
    git checkout develop   # 切换分支  
    git merge feature/FT-123456   # 合并分支  
    git branch –d feature/FT-123456   # 删除FT-123456的feature分支  
    git push –u origin hotfix/ISSUE-345678    # 推送分支
    - Master ： 稳定压倒一切，禁止尚review和测试过的代码提交到这个分支上，Master上的代码是可以随时部署到线上生产环境的。  
    - Develop ：开发分支，我们的持续集成工作在这里，code review过的代码合入到这里，我们以下要讲的BUG fix和feature开发都可以基于develop分支拉取，修改完之后合入到develop分支。
    - Feature ：功能开发和change request的分支，也即我们每一个feature都可以从devlop上拉取一个分支，开发、review和测试完之后合入develop分支。
    - Hotfix ：紧急修改的分支，在master发布到线上出现某个问题的时候，算作一个紧急布丁。从master分支上拉取代码，修改完之后合入develop和master分支。
    - Release ：预发布分支，比如0.1、0.2、1.12版本，我们一般说的系统测试就是基于这些分支做的，如果出现bug，则可以基于该release分支拉取一个临时bug分支。
    - Bug ： bug fix的分支，当我们定位、解决后合入develop和Release分支，然后让测试人员回归测试，回归测试后由close这个bug
- ## 6.20 创建分支
    - 创建分支：
    git branch dev
    - 分支切换
    git checkout dev
    - 或者：
    git checkout -b dev #创建dev分支并切换到dev分支
    - 分支合并  
    比如现在在master分支，可以调用git merge dev把dev的内容合并到master分支上
- ## 6.21 创建标签
    - 当我们发布一个重要的版本的时候，我们可以使用git tag去创建一些标签，标识版本的对应的代码。
     tag分两种，一种是轻量级的，一种是含注解的标签：  
     git tag v0.10.15  
     git tag v0.10.13 612931c0f   #对不同的commit打标签
     git tag –a v0.10.9 –m “bumped version to 0.10.9” 89de7802
    - 签署标签  
    ubuntu安装gnupg：sudo apt install gnupg  
    Centos安装gnupg：yum install gnupg  
    gpg --gen-key   # 生成密钥  
    gpg --list-keys   # 查看秘钥  
    创建带GPG签名的git tag：  
    Git tag -s v0.10.9 –u “lizhiyong”  –m “bumped version v0.10.9” 89de7802
    - 查看tag内容  
    使用git show tag_name去查看tag的内容：  
    git tag   # 列举所有的tag  
    git show v0.10.13   # 查看v0.10.13的内容
    - 推送标签到远程服务器  
    git push origin tag_name   # 推送tag到远程服务器  
    git push origin --tags   # 推送所有的tag到远程服务器
    - 检出tag代码  
    git checkout tag_name   # 检出tag的代码  
    git tag –d tag_name    # 删除tag  
    git push origin :refs/tags/tag_name   # 删除远程tag
















