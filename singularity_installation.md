# Installing Singularity
[Official installation guide](https://github.com/hpcng/singularity/blob/master/INSTALL.md).

For convenience, I documented my installation process including problems encountered and solutions

## Install system dependencies

```
$ sudo apt-get update && \
  sudo apt-get install -y build-essential \
  libseccomp-dev pkg-config squashfs-tools cryptsetup
```

## 安装 Golang

首先下载golang安装包到 `/tmp`, 然后解压至 `/usr/local`.

```
$ export VERSION=1.13.7 OS=linux ARCH=amd64  # change this as you need

$ wget -O /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz https://dl.google.com/go/go${VERSION}.${OS}-${ARCH}.tar.gz && \
  sudo tar -C /usr/local -xzf /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz
```

最后设置GO的环境:

```
$ echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
  echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
  source ~/.bashrc
```
_**NOTE:** 如果你想从老版本更新GO，须先删除 `/usr/local/go` 文件夹，再重新安装。

_**Problem:** 如果我安装的是最新版本的Go(GO1.14.4),后面的到 "编译 Singularity"这个阶段执行命令'make'时会报错："dial tcp 34.64.4.113:443: i/o timeout".
_**Cause:**  因为Go 1.14设置默认GOSUMDB=sum.golang.org，而这个网站被墙了 
_**Solution:** 1.[cdsn解决方案](https://blog.csdn.net/hhyukJae/article/details/106980818)   --未能成功
               2.尝试虚拟机跟宿主机共享 VPN    --（尝试了，未能成功）
               3.将go版本降低至GO1.13.2 ，成功解决


## 安装 golangci-lint

建议安装，官方的意思是：为了确保一致性并尽早捕获某些类型的问题，golangci-lint提供了一个配置文件。
每个pull request都必须通过那里指定的检查，这些检查将在尝试合并代码之前自动运行。
In order to install golangci-lint, you can run:

```
$ curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh |
  sh -s -- -b $(go env GOPATH)/bin v1.15.0
```


## 下载git项目

```
$ mkdir -p ${GOPATH}/src/github.com/sylabs && \
  cd ${GOPATH}/src/github.com/sylabs && \
  git clone https://github.com/sylabs/singularity.git && \
  cd singularity
```

为了保持稳定版本的Singularity,在编译前须checkout [release tag](https://github.com/sylabs/singularity/tags) :

```
$ git checkout v3.5.2
```
_**Problem:** 官方的引导命令是'git checkout v3.6.0',但是会报错
_**Cause:**  unknow
_**Solution:**  更改为'git checkout v3.5.2'


## 编译 Singularity

```
$ cd ${GOPATH}/src/github.com/sylabs/singularity && \
  ./mconfig && \
  cd ./builddir && \
  make && \
  sudo make install 
```

检查 Singularity 版本:

```
$ singularity version
```

要在不同的文件夹中构建，并将安装前缀设置为不同的路径::
```
$ ./mconfig -b ./buildtree -p /usr/local
```
网上的安装教程这一步是 （我选了这步）
'''
  ./mconfig --prefix=/opt/singularity
'''

For more information on installing/updating/uninstalling the RPM, check out our 
[admin docs](https://www.sylabs.io/guides/3.0/admin-guide/admin_quickstart.html).
