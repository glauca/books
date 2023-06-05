### 自定义 Git

#### 配置 Git

客户端基本配置
```bash
$ git config --global core.editor emacs
$ git config --global core.editor vim

设定提交信息模板
$ git config --global commit.template ~/.gitmessage.txt
$ git commit

关闭分页
$ git config --global core.pager ''
$ git config --global core.pager more/less

关掉 Git 的终端颜色输出
$ git config --global color.ui false

格式化与多余的空白字符
    提交时自动地把回车和换行转换成换行 检出代码时，换行会被转换成回车和换行
$ git config --global core.autocrlf true
    提交时把回车和换行转换成换行，检出时不转换
$ git config --global core.autocrlf input
    把回车保留在版本库中
$ git config --global core.autocrlf false

提交了有空白问题的文件，但还没推送到上游 让 Git 在重写补丁时自动修正它们
$ git rebase --whitespace=fix
```

服务器端配置

```bash
如果你变基已经被推送的提交，继而再推送，又或者推送一个提交到远程分支，而这个远程分支当前指向的提交不在该提交的历史中，这样的推送会被拒绝。
$ git config --system receive.denyNonFastForwards true
$ git config --system receive.denyDeletes true
```

#### Git 属性

.gitattributes 文件 针对特定的路径配置某些设置项

二进制文件

```bash
*.pbxproj binary

# docx2txt http://docx2txt.sourceforge.net
*.docx diff=word
$ git config diff.word.textconv docx2txt


# exiftool 图像比较
*.png diff=exif
$ git config diff.exif.textconv exiftool
```

关键字展开

```bash
$ git config --global filter.indent.clean indent
$ git config --global filter.indent.smudge cat
```

导出版本库

```bash
export-ignore
export-subst

$ git archive
```

合并策略

```bash
$ git config --global merge.ours.driver true
```

#### Git 钩子

#### 使用强制策略的一个例子

服务器端钩子

update 脚本会为每一个提交的分支各运行一次，它接受三个参数：
+ 被推送的引用的名字
+ 推送前分支的修订版本（revision）
+ 用户准备推送的修订版本（revision）

```bash
$ git rev-list 538c33..d14fc7
$ git rev-list --parents 538c33..d14fc7
$ git cat-file commit ca82a6
$ git cat-file commit ca82a6 | sed '1,/^$/d'
```

Example of ACL [链接地址](https://git-scm.com/book/en/v2/Customizing-Git-An-Example-Git-Enforced-Policy)

```bash
$ git log -1 --name-only --pretty=format:'' 9f585d
```