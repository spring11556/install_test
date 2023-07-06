## 0x00centos6.x i686/x86_64 devtoolset-x

以 centos6.10 i686安装 devtoolset-4为例，其他安装的搭配组合调整相应参数和路径即可

> 前提是安装了`scl`
>
> ```
> yum -y install centos-release-scl
> ```
>
> 我是通过文章【https://www.cnblogs.com/52fhy/p/12547521.html】中【升级到gcc 4.8】部分，直接就安装成功了scl
>
> ```
> wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtoolset-2.repo
> yum -y install devtoolset-2-gcc devtoolset-2-gcc-c++ devtoolset-2-binutils
> 
> root@~/gdb-9.2 $ yum list installed | grep scl
> scl-utils.i686          20120927-29.el6_9
> scl-utils-build.i686    20120927-29.el6_9
> ```

https://repo.cloudlinux.com/cloudlinux/6.10/sclo/devtoolset-4/i386/ 包含了各种各样的centos6.10的rpm包，怎么添加进centos6.10的源 并把其中rpm安装

要将 https://repo.cloudlinux.com/cloudlinux/6.10/sclo/devtoolset-4/i386/ 添加为 CentOS 6.10 的源，并安装其中的所有 RPM 包，可以按照以下步骤进行操作：

1. 创建一个新的源配置文件，可以使用任何你喜欢的文件名，比如 `cloudlinux.repo`：

   ```bash
   cd /etc/yum.repos.d/
   vi cloudlinux.repo
   ```

2. 在编辑器中输入以下内容来定义新的源：

   ```bash
   [cloudlinux]
   name=CloudLinux 6.10 Repository
   baseurl=https://repo.cloudlinux.com/cloudlinux/6.10/sclo/devtoolset-4/i386/
   enabled=1
   gpgcheck=0
   ```

   这将创建一个名为 `cloudlinux` 的源，其中包含 CloudLinux 6.10 的 RPM 包。

   - `name`：源的名称，可以自定义。
   - `baseurl`：RPM 包所在的 URL。
   - `enabled`：启用源，设置为 `1`。
   - `gpgcheck`：是否检查 RPM 包的 GPG 签名，此处设置为 `0`，表示不检查。

   请注意，由于该源是第三方源，我将 `gpgcheck` 设置为 `0` 来跳过 GPG 签名的检查。这是出于演示目的，实际使用时建议进行合适的 GPG 签名验证以确保软件包的安全性。

3. 现在你可以使用新添加的源来安装其中的所有 RPM 包。使用以下命令：

   ```bash
   yum --disablerepo=\* --enablerepo=cloudlinux list available
   yum --disablerepo=\* --enablerepo=cloudlinux install *
   ```

   这将使用新添加的源安装该源中的所有 RPM 包。请注意，这可能会涉及大量的软件包下载和安装，时间可能会较长。

4. 使用devtoolset-4升级到gcc 5.2

   ```bash
   yum --disablerepo=\* --enablerepo=cloudlinux install  devtoolset-4-gcc devtoolset-4-gcc-c++ devtoolset-4-binutils
   
   scl enable devtoolset-4 bash
   
   root@/etc/yum.repos.d $ gcc --version
   gcc (GCC) 5.3.1 20160406 (Red Hat 5.3.1-6)
   Copyright (C) 2015 Free Software Foundation, Inc.
   This is free software; see the source for copying conditions.  There is NO
   warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
   ```

> 参考：[Centos6安装gcc4.8及以上版本 ](https://www.cnblogs.com/52fhy/p/12547521.html) https://www.cnblogs.com/52fhy/p/12547521.html