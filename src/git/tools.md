### Git 工具

#### 选择修订版本

```bash
$ git log --abbrev-commit --pretty=oneline
$ git log [version]

分支引用
$ git log [branch]

Git 探测工具
$ git show [branch]

引用日志 [只存在于本地仓库]
$ git reflog
$ git show HEAD@{5}
$ git show master@{yesterday}

祖先引用
$ git show HEAD^
$ git show [version]^
$ git show [version]^^
$ git show [version]~3
$ git show HEAD~3^2

提交区间
$ git log [log not in branch]..[log in branch]
$ git log origin/master..HEAD
==
$ git log origin/master..

$ git log refA..refB
$ git log ^refA refB
$ git log refB --not refA

$ git log refA refB ^refC
$ git log refA refB --not refC

被两个引用中的一个包含但又不被两者同时包含的提交
$ git log master...[branch]
$ git log --left-right master...[branch]
```

> 关于 SHA-1 的简短说明

> 许多人觉得他们的仓库里有可能出现两个 SHA-1 值相同的对象。 然后呢？

> 如果你真的向仓库里提交了一个跟之前的某个对象具有相同 SHA-1 值的对象，Git 发现仓库里已经存在了拥有相同 HASH 值的对象，就会认为这个新的提交是已经被写入仓库的。 如果之后你想检出那个对象时，你将得到先前那个对象的数据。

> 但是这种情况发生的概率十分渺小。 SHA-1 摘要长度是 20 字节，也就是 160 位。 280 个随机哈希对象才有 50% 的概率出现一次冲突 （计算冲突机率的公式是 p = (n(n-1)/2) * (1/2^160)) ）。280 是 1.2 x 10^24 也就是一亿亿亿。 那是地球上沙粒总数的 1200 倍。

> 举例说一下怎样才能产生一次 SHA-1 冲突。 如果地球上 65 亿个人类都在编程，每人每秒都在产生等价于整个 Linux 内核历史（360 万个 Git 对象）的代码，并将之提交到一个巨大的 Git 仓库里面，这样持续两年的时间才会产生足够的对象，使其拥有 50% 的概率产生一次 SHA-1 对象冲突。 这要比你编程团队的成员同一个晚上在互不相干的意外中被狼袭击并杀死的机率还要小。

#### 交互式暂存

```bash
$ git add -i
```

#### 储藏与清理

```bash
$ git stash
$ git stash --all
$ git stash list
$ git stash apply
$ git stash apply stash@{2}
$ git stash apply --index

$ git stash drop stash@{0}

$ git stash pop

不要储藏任何通过 git add 命令已暂存的东西
$ git stash --keep-index

储藏未跟踪文件
$ git stash --include-untracked
$ git stash -u

从储藏创建一个分支
$ git stash branch [branch name]

清理工作目录
$ git clean -f -d 移除工作目录中所有未追踪的文件以及空的子目录
$ git clean -d -n 做一次演习 将要 移除什么
$ git clean -n -d -x
$ git clean -x -i
```
#### 签署工作
(签署工作)[https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work]

#### 搜索

```bash
$ git grep -n [keyword]
$ git grep --count [keyword]
$ git grep -p [keyword] *.c
$ git grep -n  --heading --break [keyword]

日志搜索
$ git log -S ZLIB_BUF_MAX --oneline
$ git log -G [Regexp]

行日志搜索
$ git log -L :git_deflate_bound:zlib.c
$ git log -L '/unsigned long git_deflate_bound/',/^}/:zlib.c
```

#### 重写历史

```bash
$ git commit --amend

$ git rebase -i HEAD~3
```

核武器级选项：filter-branch

```bash
从每一个提交移除一个文件
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD

使一个子目录做为新的根目录
$ git filter-branch --subdirectory-filter trunk HEAD
```

#### 重置揭密

```bash
移动 HEAD 不会改变索引和工作目录
$ git reset --soft

更新索引 回滚到了所有 git add 和 git commit 的命令执行之前 [默认行为]
$ git reset --mixed

更新工作目录 撤销最后的提交 git add 和 git commit 命令以及工作目录中的所有工作
$ git reset --hard

$ git reset [version] [filename]

$ git reset --soft HEAD~2
$ git commit
```

#### 高级合并

```bash
$ git merge --abort

忽略空白
$ git merge -X ignore-all-space [branch]
$ git merge -X ignore-space-change [branch]

手动文件再合并
> Git 在索引中存储了所有这些版本，在 “stages” 下每一个都有一个数字与它们关联。 Stage 1 是它们共同的祖先版本，stage 2 是你的版本，stage 3 来自于 MERGE_HEAD，即你将要合并入的版本（“theirs”）。
$ git show :1:hello.rb > hello.common.rb
$ git show :2:hello.rb > hello.ours.rb
$ git show :3:hello.rb > hello.theirs.rb

$ git merge-file -p hello.ours.rb hello.common.rb hello.theirs.rb > hello.rb

$ git diff --ours
$ git diff --theirs -b
$ git diff --base -b
$ git clean -f

其他类型的合并
$ git merge -X ours [branch]
$ git merge -s ours [branch]
```

#### Rerere

```bash
$ git config --global rerere.enabled true
```

#### 使用 Git 调试

```bash
$ git blame -L 12,22 [filename]
$ git blame -C -L 12,22 [filename]

二分查找
$ git bisect start
$ git bisect bad
$ git bisect good v1.0
$ git bisect good
$ git bisect bad
$ git bisect reset
```

#### 子模块

子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

```bash
开始使用子模块
$ git submodule add https://github.com/chaconinc/DbConnector

$ git diff --cached DbConnector
$ git diff --cached --submodule
$ git commit -am 'added DbConnector module'

克隆含有子模块的项目
默认会包含该子模块目录，但其中没有任何文件
$ git clone https://github.com/chaconinc/MainProject
$ git submodule init
$ git submodule update

==

$ git clone --recursive https://github.com/chaconinc/MainProject

在包含子模块的项目上工作
进入子模块然后抓取 默认更新子模块仓库的 master 分支
$ git submodule update --remote DbConnector

$ git submodule update --remote --merge

$ git submodule update --remote --rebase

发布子模块改动
$ git push --recurse-submodules=check
$ git push --recurse-submodules=on-demand
```
[子模块](https://git-scm.com/book/en/v2/Git-Tools-Submodules)


#### 打包

````bash
$ git bundle create repo.bundle HEAD master

$ git clone repo.bundle repo

查看做了哪些更改
$ git log --oneline master ^origin/master
创建新的包文件
$ git bundle create commits.bundle master ^9a466c5
验证并导入新包文件
$ git bundle verify ../commits.bundle

查看包里可以导入哪些分支
$ git bundle list-heads ../commits.bundle

从包中取出 master 分支到我们仓库中的 other-master 分支
$ git fetch ../commits.bundle master:other-master
````

#### 替换

#### 凭证存储

Git 拥有处理凭证系统来的一些选项：

+ 默认所有都不缓存。 每一次连接都会询问你的用户名和密码。

+ “cache” 模式会将凭证存放在内存中一段时间。 密码永远不会被存储在磁盘中，并且在15分钟后从内存中清除。

+ “store” 模式会将凭证用明文的形式存放在磁盘中，并且永不过期。 这意味着除非你修改了你在 Git 服务器上的密码，否则你永远不需要再次输入你的凭证信息。 这种方式的缺点是你的密码是用明文的方式存放在你的 home 目录下。

+ 如果你使用的是 Mac，Git 还有一种 “osxkeychain” 模式，它会将凭证缓存到你系统用户的钥匙串中。 这种方式将凭证存放在磁盘中，并且永不过期，但是是被加密的，这种加密方式与存放 HTTPS 凭证以及 Safari 的自动填写是相同的。

+ 如果你使用的是 Windows，你可以安装一个叫做 “winstore” 的辅助工具。 这和上面说的 “osxkeychain” 十分类似，但是是使用 Windows Credential Store 来控制敏感信息。 可以在 [https://gitcredentialstore.codeplex.com](https://gitcredentialstore.codeplex.com) 下载。

```bash
$ git config --global credential.helper cache
$ git config --global credential.helper store --file ~/.my-credentials

[credential]
    helper = store --file /mnt/thumbdrive/.git-credentials
    helper = cache --timeout 30000
```