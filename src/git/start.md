### 起步

#### 安装 Git

Linux上安装

```bash
$ sudo yum install git
```

```bash
$ sudo apt-get install git
```

#### 配置 Git

```bash
git config

/etc/gitconfig 文件 包含系统上每一个用户及他们仓库的通用配置 --system 选项

~/.gitconfig 或 ~/.config/git/config 文件 只针对当前用户 --global 选项

.git/config 针对当前仓库

Windows系统中 git会查找 C:\Users\$USER 的 .gitconfig 文件
```

用户信息

```bash
$ git config --global user.name "Jone"
$ git config --global user.email jone@example.com

针对特定项目使用不同用户名和邮件地址时 可以在那个项目目录下运行没有 --global 的选项命令
```

默认文本编辑器

```bash
$ git config --global core.editor vim
```

```bash
$ git config --list
$ git config user.name
```

```bash
$ git help <verb>
$ git <verb> --help
```