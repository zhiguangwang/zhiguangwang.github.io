---
title: 搭建GitHub仓库镜像
---

我所在的团队在使用 [GitHub][1]。GitHub在中国大陆没有架设服务器。由于众所周知的原因，从大陆访问GitHub，速度相当感人。

我们fork了 [UnrealEngine][2] 仓库后，发现clone/pull的时候，不但速度难以忍受，还经常断线。于是我在办公室内网环境下做了GitHub的仓库镜像。一开始只支持了UnrealEngine，后来扩充到支持任意用户名下的任意仓库。

本文整理自我在团队内部撰写的文档，讲述搭建镜像的完整步骤。使用的操作系统为 `Ubuntu Server 14.04.4 LTS`。

### 安装最新的Git

用 **PPA** 安装最新的 `git` 和 `git-daemon-sysvinit`，后者以 **SysVinit** 方式管理 **git-daemon** 服务。

    sudo add-apt-repository ppa:git-core/ppa
    sudo apt-get update
    sudo apt-get install git git-daemon-sysvinit

### 创建Git服务用户

    sudo adduser github

后续操作，除了需要 **sudo** 的，均以用户 **github** 来执行：

    sudo su github

### 创建仓库存放目录

    cd ~
    mkdir -p mirror/repos/[username]

### 从GitHub Clone仓库

    cd ~/mirror/repos/[username]
    git clone --mirror https://github.com/[username]/[repo].git

### 配置git-daemon服务

修改 `/etc/default/git-daemon` 为：

```shell
# Defaults for git-daemon initscript
# sourced by /etc/init.d/git-daemon
# installed at /etc/default/git-daemon by the maintainer scripts

#
# This is a POSIX shell fragment
#

GIT_DAEMON_ENABLE=true
GIT_DAEMON_USER=github
GIT_DAEMON_BASE_PATH=/home/github/mirror/repos
GIT_DAEMON_DIRECTORY=/home/github/mirror/repos

# Additional options that are passed to the Daemon.
GIT_DAEMON_OPTIONS="--export-all"
```
其中 `--export-all` 选项将使 **git-daemon** 对 `/home/github/mirror/repos` 下的所有仓库提供服务。

重启git-daemon服务（需要先切换到管理员用户，执行后切换回github用户）

    sudo service git-daemon restart

### 定时更新镜像仓库

### 设置 credential store

如果要镜像的仓库是private的，则需要authentication。因此需要设置一下账号密码的存储：

    $ cd ~/mirror/repos/[username]/[repo].git
    $ git config credential.helper store
    $ git remote update
    Username: <type your username>
    Password: <type your password>

注意，密码（如果开启了2FA则密码是Personal Access Token）将被明文存储在 `~/.git-credentials` 中。我的做法是为团队注册了一个单独的bot用户，设定只读权限，并分配一个只具有仓库访问权的Personal Access Token。

更详细的文档，见 [git-credential-store](https://git-scm.com/docs/git-credential-store)。

#### 更新脚本

编辑 `~/mirror/pull.sh`

```shell
#/bin/bash

set -e

cd ~/mirror

# Iterate through all .git directories
while IFS= read -r -d $'\0' REPO_DIR; do
    pushd $REPO_DIR > /dev/null
    echo -n "$(date +"%F %T") "
    echo -n "$REPO_DIR "
    git remote update
    popd > /dev/null
done < <(find repos/ -name *.git -print0)
```

此脚本会遍历 `~/mirror/repos` 下的所有仓库目录，依次做 `git remote update`。

赋予执行权限：

    chmod u+x ~/mirror/pull.sh

#### 编辑crontab

执行 `crontab -e`，在文件最后添加：

    * * * * * bash -c ~/mirror/pull.sh >> ~/mirror/pull.log 2>&1
    * * 1 * * rm -vf ~/mirror/pull.log

解读

- 第1行将每分钟执行 **pull.sh**，从GitHub获取仓库的最新提交
- 第2行将在每月1日凌晨清理日志文件 **pull.log**

### 使用

对于GitHub上的仓库 `https://github.com/[username]/[repo].git` ，其内网地址为：

    git://[intranet-domain]/[username]/[repo].git

使用的时候，从内网镜像的地址clone，然后把 **Push URL** 修改为GitHub上的仓库地址：

    git remote set-url --push origin https://github.com/[username]/[repo].git

运行 `git remote -v`，检查 **Fetch URL** 和 **Push URL**，将会看到：

    origin    git://[intranet-domain]/rog2/UnrealEngine.git (fetch)
    origin    https://github.com/rog2/UnrealEngine.git (push)

需要注意的是：

- 镜像是 Read-Only 的，无法push到此镜像。
- 镜像使用git协议，无加密和鉴权，在内网部署没有问题，但如果某日想要部署在公网上，必须引入VPN等机制，否则就等于是裸奔了。

[1]: https://github.com
[2]: https://www.unrealengine.com/ue4-on-github
