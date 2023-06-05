### 服务器

#### Git 协议

1. 本地协议
2. HTTP协议
3. SSH协议
4. Git协议

```bash
本地协议

$ git clone /opt/git/project.git
or
$ git clone file:///opt/git/project.git
```

```bash
HTTP协议

$ git clone https://example.com/gitproject.git
```

```bash
SSH协议

$ git clone ssh://user@server/project.git
or
$ git clone user@server:project.git
```

```bash
Git协议

监听端口 9418
```

#### 在服务器上搭建 Git

```bash
克隆裸仓库 一个不包含当前工作目录的仓库

$ git clone --bare my_project my_project.git
==
$ cp -Rf my_project/.git my_project.git

$ scp -r my_project.git user@git.example.com:/opt/git
$ git clone user@git.example.com:/opt/git/my_project.git
```

#### 生成 SSH 公钥

```bash
$ ssh-keygen

authorized_keys
id_dsa.pub
```
#### 配置服务器

```bash
$ sudo adduser git
$ su git
$ cd
$ mkdir .ssh && chmod 700 .ssh
$ touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys

$ cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys

$ cd /opt/git
$ mkdir project.git
$ cd project.git
$ git init --bare

$ cat /etc/shells   # see if `git-shell` is already in there.  If not...
$ which git-shell   # make sure git-shell is installed on your system.
$ sudo vim /etc/shells  # and add the path to git-shell from last command

$ sudo chsh git  # and enter the path to git-shell, usually: /usr/bin/git-shell
```

### 新建仓库并推送到远程

~~~bash
# create a new repo

$ git init
$ git add ./
$ git commit -m 'commit info'
$ git remote add origin repo
$ git push -u origin master

# or push an existing repository

$ git remote add origin repo
$ git push -u origin master
~~~