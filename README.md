# singularity_spack
spack 自动化部署singularity镜像

## 1.生成spack基容器 
 
```
$ sudo singularity build basespack.sif basespack.def
```

**NOTE:** 通过镜像定义文件创建singularity镜像。

## 2.展示可以安装的软件包list

```
$ spack list
```

_** 目前进度：**

1.*问题(已解决)：* 在用'spack install 软件包名' 出现编译器Bug 

*方法：* 在新系统上重新安装singularity镜像及spack 

*原因：* 环境版本的原因


2.*问题(已解决)：* 'spack install 软件包名'后续报错：'Read-only file system: '/opt/spack/var/spack/junit-report'' 

*方法：*  1.[尝试修改挂载权限](https://www.cnblogs.com/jxldjsn/p/11337990.html) --未能成功

          2.翻查singularity用户手册得知创建的sif为压缩只读格式。
          尝试用'-- sandbox'沙箱选项创建 用于交互式开发的根目录可写格式的镜像。 

*原因：* :翻查singularity用户手册得知创建的sif为压缩只读格式。 【泪目.jpg】卡在这可太久了！！



**思路:** 展示架构选择：我目前的理解是  在使用镜像定义文件创建的基容器时，'From:centos:7 ' 这一栏暂时先空着，等基容器创建完毕，再用 'spack arch'
来展示架构选择，如'centos , ubuntu ,suse11'

```
$ spack arch
```

_** 目前进度：**

3.*问题(未解决)：* 用'spack arch ' 结果显示为 'linux-centos7-sandybridge' 

*方法：* 修改定义文件 

*原因（猜测）：* 使用镜像定义文件创建的基容器时，已经设定架构，如：'From:centos:7 '导致无法再为容器选择架构


## 4.根据 2、3的选择，生成自动安装脚本

**思路:** spack的用户手册正在学习




## 5.通过点击生成命令，完成软件包的安装，容器镜像的生成




## 6.生成一个下载链接
