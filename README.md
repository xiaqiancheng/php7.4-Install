	
### 【CentOS6】
使用腾讯云源。

备份原先yum源
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
下载新的yum源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.cloud.tencent.com/repo/centos6_base.repo
将文件内的http替换为https
sed -i 's#http#https#g' /etc/yum.repos.d/CentOS-Base.repo


### 【编译php】
./configure --prefix=/usr/local/php74 \
--enable-fast-install \
--disable-fileinfo \
--without-sqlite3 \
--without-pdo_sqlite \
--disable-fileinfo \
--enable-fpm \
--with-config-file-path=/usr/local/php74/etc \
--with-openssl --with-zlib --with-curl \
--enable-ftp --with-gd --with-xmlrpc \
--with-jpeg-dir --with-png-dir \
--with-freetype-dir --enable-gd-native-ttf \
--enable-mbstring --disable-mbregex \
--with-mcrypt=/usr/local/libmcrypt --enable-zip  \
--without-pear --enable-bcmath \
--enable-mysqlnd --with-pdo-mysql=mysqlnd


### 【autoConfig】
[root@wslu-cs wslu]# autoconf
configure.ac:8: error: Autoconf version 2.64 or higher is required
configure.ac:8: the top level
autom4te: /usr/bin/m4 failed with exit status: 63

查看版本
[root@wslu-cs wslu]# rpm -qf /usr/bin/autoconf
autoconf-2.63-5.1.el6.noarch

卸载
[root@wslu-cs wslu]# rpm -e --nodeps autoconf-2.63

安装
[root@wslu-cs wslu]# tar zxvf autoconf-2.69.tar.gz
[root@wslu-cs wslu]# cd autoconf-2.69
[root@wslu-cs wslu]# ./configure --prefix=/usr/
[root@wslu-cs wslu]# make && make install

### 【redis】
tar -zxvf redis-5.2.1.tgz
cd redis-5.2.1/
phpize
./configure --with-php-config=/home/work/study/soft/php/bin/php-config
make && make install

### 【odium】
Step1、下载编译安装libsodium

依次执行命令

tar -zxf libsodium-1.0.18-stable.tar.gz
cd libsodium-stable
./configure --prefix=/usr
make && make check
sudo make install
sudo ldconfig

Step2、下载编译安装sodium

核对导入/usr/lib/pkgconfig路径

echo  $PKG_CONFIG_PATH
export PKG_CONFIG_PATH=/usr/lib/pkgconfig
echo $PKG_CONFIG_PATH

cd php-7.4.28/ext/sodium
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make
make install

### 【fileinfo】
cd php-7.4.28/ext/fileinfo/
编译开始
phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install

### 【pcntl】
cd php-7.4.28/ext/pcntl/
编译开始
phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install

### 【swoole】
./configure --enable-openssl --with-openssl-dir=/usr/local/openssl  --with-php-config=/usr/local/php74/bin/php-config

### 【gd】
tar -zxvf libzip-1.2.0.tar.gz \
cd libzip-1.2.0 \
./configure \
make \
make install

tar -xzvf libgd-2.2.3.tar.gz \
cd libgd-2.2.3 \
./configure --enable-shared \
make \
make install \
make clean \
make distclean

tar -xzvf zlib-1.2.12.tar.gz \
cd zlib-1.2.12 \
./configure --prefix=/usr/local/zlib --enable-shared \
make \
make install

tar -xzvf freetype-2.10.1.tar.gz \
cd freetype-2.10.1\
./configure --prefix=/usr/local/freetype --enable-shared \
make \
make install

tar -xzvf libpng-1.6.31.tar.gz \
cd libpng-1.6.31 \
./configure --prefix=/usr/local/libpng --enable-shared \
make \
make install

tar -xzvf jpegsrc.v9d.tar.gz \
cd jpeg-9d \
./configure --prefix=/usr/local/libjpeg --enable-shared \
make \
make install

cd php7.4
./configure \
--enable-gd \
--with-php-config=/usr/local/php74/bin/php-config \
--with-jpeg=/usr/local/libjpeg \
--with-freetype=/usr/local/freetype \
make && make install

### 【zip】
cd php7.4
./configure \
--with-php-config=/usr/local/php74/bin/php-config
make && make install

### 【更新gcc】
tar -jxvf gcc-4.8.2.tar.bz2

2、下载供编译需求的依赖项
    这个神奇的脚本文件会帮我们下载、配置、安装依赖库，可以节约我们大量的时间和精力。
cd gcc-4.8.0
./contrib/download_prerequisites

3、建立一个文件夹
mkdir gcc-build-4.8.2
cd gcc-build-4.8.2

4、生成Makefile文件
../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib

5、编译安装
make && make intsall




### 【ERROR】
PHP 7.4.x中mbstring的正则表达式功能需要oniguruma。解决错误“No package 'oniguruma' found”。
不使用mbstring的正则功能，即在“--enable-mbstring”后再添加“--disable-mbregex”参数。这样的配置将不再需要oniguruma库。