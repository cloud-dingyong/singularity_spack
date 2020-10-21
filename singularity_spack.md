# singularity_spack --代码部署步骤
spack 自动化部署singularity镜像 

## 1.安装[singularity](https://github.com/cloud-dingyong/singularity_spack/blob/master/singularity_installation.md)镜像


## 2.部署网页的前后端


2.1 下载并解压[elementui.tar.gz]文件，并安装相应依赖



 2.2 修改前端dev.env.js中相应的接口
```
$ API_URL:'"http://authdemo.cloud.hdu.edu.cn"' 
```


    2.3 修改application.properties中Mysql数据库接口



    2.4 修改UserController中的相应运行路径



    **2.5 UserController中脚本语句**

         2.5.1 生成spack基容器 

        ```
        $ sudo singularity  build --sandbox test/  basespack.def 
        ```

        **NOTE:** 通过镜像定义文件创建可修改的singularity沙盒镜像test。

         2.5.2 以root权限在该镜像内安装相应软件包

        ```
        $ sudo singularity exec --writable test/ spack install zip
        ```

        2.5.3 将沙盒镜像转化为img格式

        ```
        $ sudo  singularity build test.img  test/  
        ```
     
## 3.运行项目
