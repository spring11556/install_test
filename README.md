# install_test
centos6.x  i686/x86_64  install  devtoolset-x (upgrade GCC)
## centos6.x  i686/x86_64  install  devtoolset-x (upgrade GCC)

Let's take the example of installing devtoolset-4 on CentOS 6.10 i686, and adjust the corresponding parameters and paths for other installations.

> Prerequisite: Install `scl` by running the command:
>
> ```bash
> yum -y install centos-release-scl
> ```
>
> I successfully installed `scl` directly by following the "Upgrade to GCC 4.8" section in the article [here](https://www.cnblogs.com/52fhy/p/12547521.html).
>
> ```bash
> wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtoolset-2.repo
> yum -y install devtoolset-2-gcc devtoolset-2-gcc-c++ devtoolset-2-binutils
> 
> root@~/gdb-9.2 $ yum list installed | grep scl
> scl-utils.i686          20120927-29.el6_9
> scl-utils-build.i686    20120927-29.el6_9
> ```

The URL https://repo.cloudlinux.com/cloudlinux/6.10/sclo/devtoolset-4/i386/ contains various RPM packages for CentOS 6.10. Here's how you can add it as a source for CentOS 6.10 and install all the RPM packages:

1. Create a new source configuration file with any name you prefer, such as `cloudlinux.repo`:

   ```bash
   cd /etc/yum.repos.d/
   vi cloudlinux.repo
   ```

2. Enter the following content in the editor to define the new source:

   ```bash
   [cloudlinux]
   name=CloudLinux 6.10 Repository
   baseurl=https://repo.cloudlinux.com/cloudlinux/6.10/sclo/devtoolset-4/i386/
   enabled=1
   gpgcheck=0
   ```

   This will create a source named `cloudlinux` that contains RPM packages for CloudLinux 6.10.

   - `name`: The name of the source, which can be customized.
   - `baseurl`: The URL where the RPM packages are located.
   - `enabled`: Enable the source by setting it to `1`.
   - `gpgcheck`: Whether to check the GPG signature of the RPM packages. Here, it is set to `0` to skip the check.

   Please note that since this source is a third-party source, I've set `gpgcheck` to `0` to skip GPG signature verification. This is for demonstration purposes, and in actual usage, it is recommended to perform appropriate GPG signature verification to ensure the security of the packages.

3. Now, you can use the newly added source to install all the RPM packages within it. Use the following commands:

   ```bash
   yum --disablerepo=\* --enablerepo=cloudlinux list available
   yum --disablerepo=\* --enablerepo=cloudlinux install *
   ```

   This will install all the RPM packages from the added source. Please note that this may involve downloading and installing a large number of packages, which may take some time.

4. Upgrade to GCC 5.2 using devtoolset-4:

   ```bash
   yum --disablerepo=\* --enablerepo=cloudlinux install  devtoolset-4-gcc devtoolset-4-gcc-c++ devtoolset-4-binutils
   
   scl enable devtoolset
   
   root@/etc/yum.repos.d $ gcc --version
   gcc (GCC) 5.3.1 20160406 (Red Hat 5.3.1-6)
   Copyright (C) 2015 Free Software Foundation, Inc.
   This is free software; see the source for copying conditions.  There is NO
   warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
   ```

> \> Reference: [Centos6安装gcc4.8及以上版本 ](https://www.cnblogs.com/52fhy/p/12547521.html) (Chinese)
