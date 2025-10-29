
## 安装说明

```bash
# linux编译环境搭建
sudo yum install -y git patch wget perl perl-IPC-Cmd dos2unix mercurial perl-Data-Dumper pcre-devel openssl-devel gcc curl zlib-devel glibc-devel
gcc --version
# 升级gcc版本>7

# git克隆本项目的“ 1.27.1.x ”分支
git clone -b 1.27.1.x https://github.com/LeungHL/openresty


cd openresty
# 编译预编译环境，拉取nginx，openssl等的源码，打补丁等操作，生成可以编译成openresty程序的源代码
sudo make
# 如果换行等其它其它的shell错误问题，一般是因为某些脚本是crlf编码的，可以用dos2unix转一下
# sudo dos2unix util/*
# 成功会有一个openresty-1.27.1.3的目录，里面是openresty的源码
cd openresty-1.27.1.3
#./configure nginx的模块，建议采用configure.sh里面的配置, 默认安装到/usr/local/openresty（按需修改prefix参数即可）
# centos 7,8 增加 CFLAGS 配置
# sudo export CFLAGS="-D_MAP_ANON_SAFE_ -DMAP_ANON=0x1000 -D_XOPEN_SOURCE=700"
sudo ./configure --prefix=/usr/local/openresty \
# --with-cc-opt="$CFLAGS" \
--add-module=$(pwd)/bundle/nginx-1.27.1/ja4-nginx-module/src \
--with-openssl=$(pwd)/openssl-3.5.1 \
--with-ipv6 \
--with-http_v2_module \
--without-mail_pop3_module \
--without-mail_imap_module \
--without-mail_smtp_module \
--with-http_stub_status_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_auth_request_module \
--with-http_secure_link_module \
--with-http_random_index_module \
--with-http_gzip_static_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-select_module 


# 编译，j后面的数字是线程数，加快编译速度，openssl编译比较慢
sudo make -j4

# 编译成功后安装, 默认安装到/usr/local/openresty, 可以通过上面的configure自定义安装路径
sudo make install

```