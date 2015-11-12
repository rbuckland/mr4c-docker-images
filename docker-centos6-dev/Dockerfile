FROM centos:6

MAINTAINER “Ramon Buckland” <ramon@thebuckland.com>


#
# Special custom YUM repos that have extra packages required by mr4c
#

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
RUN rpm -Uvh http://elgis.argeo.org/repos/6/elgis-release-6-6_0.noarch.rpm

#
# Initial set of Development tools
#
RUN yum install -y \
  tar bzip2 git vim subversion wget zip unzip screen \
  ant ant-optional ivy

#
# we need GCC 4.8+
#
RUN wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtools-2.repo
RUN yum -y install devtoolset-2-gcc devtoolset-2-binutils devtoolset-2-gcc-c++

RUN yum -y install python 
RUN yum -y install rpm-build 

ENV CC  /opt/rh/devtoolset-2/root/usr/bin/gcc  
ENV CPP /opt/rh/devtoolset-2/root/usr/bin/cpp
ENV CXX /opt/rh/devtoolset-2/root/usr/bin/c++

RUN ln -s /opt/rh/devtoolset-2/root/usr/bin/* /usr/local/bin/

VOLUME ["/opt/projects"]
