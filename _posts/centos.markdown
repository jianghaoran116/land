# 6.8
// 安装依赖
yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
// 解压下载的git
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
tar -zxvf ...

cd ...
// 编译  
sudo make profix=/usr/local all
// 安装  
sudo make profix=/usr/local install