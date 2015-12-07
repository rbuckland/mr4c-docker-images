FROM centos:6


#
# Special custom YUM repos that have extra packages required by mr4c
#

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
RUN rpm -Uvh http://elgis.argeo.org/repos/6/elgis-release-6-6_0.noarch.rpm

#
# Initial set of Development tools
#
RUN yum install -y \
  tar bzip2 git vim subversion wget zip unzip screen sudo \
  ant ant-optional ivy

#
# we don't need sudo because in docker we are root, but mr4c ./deploy_all (further down) uses sudo
# but centos/redhat has issue: http://unix.stackexchange.com/questions/122616/why-do-i-need-a-tty-to-run-sudo-if-i-can-sudo-without-a-password
#
RUN sed -i -e 's/Defaults.*requiretty//' /etc/sudoers

#
# we need GCC 4.8+
#
RUN wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtools-2.repo
RUN yum -y install devtoolset-2-gcc devtoolset-2-binutils devtoolset-2-gcc-c++

ENV CC  /opt/rh/devtoolset-2/root/usr/bin/gcc  
ENV CPP /opt/rh/devtoolset-2/root/usr/bin/cpp
ENV CXX /opt/rh/devtoolset-2/root/usr/bin/c++

RUN ln -s /opt/rh/devtoolset-2/root/usr/bin/* /usr/local/bin/

#
# mr4c wants gdal 1.10 (it does compile with older 1.9 .. but let's not tempt fate)
# these RPMS were found :-)
#

# ADD http://ftp.tmapy.cz/tmapy-twist/centos/6Server/testing/x86_64/RPMS/gdal-java-1.11.2-1.el6.x86_64.rpm /tmp/
ADD extra-dependencies/gdal-java-1.11.2-1.el6.x86_64.rpm /tmp/
# ADD http://ftp.tmapy.cz/tmapy-twist/centos/6Server/testing/x86_64/RPMS/gdal-libs-1.11.2-1.el6.x86_64.rpm /tmp/
ADD extra-dependencies/gdal-libs-1.11.2-1.el6.x86_64.rpm /tmp/
# ADD http://ftp.tmapy.cz/tmapy-twist/centos/6Server/testing/x86_64/RPMS/gdal-devel-1.11.2-1.el6.x86_64.rpm /tmp/
ADD extra-dependencies/gdal-devel-1.11.2-1.el6.x86_64.rpm /tmp/
# ADD http://ftp.tmapy.cz/tmapy-twist/centos/6Server/testing/x86_64/RPMS/gdal-1.11.2-1.el6.x86_64.rpm /tmp/
ADD extra-dependencies/gdal-1.11.2-1.el6.x86_64.rpm /tmp/
RUN yum -y --nogpgcheck localinstall /tmp/gdal*-1.11.2-1.el6.x86_64.rpm && rm /tmp/gdal*.rpm

#
# mr4c Development Dependencies
#
RUN yum -y install blas lapack atlas \
           cppunit cppunit-devel \
           jansson jansson-devel \
           proj proj-devel
#
# maven and any + ivy the redhat / centos way
#

RUN yum -y install apache-ant apache-ivy ant-junit

#
# install CDH Repo and install hadoop client
#
RUN wget http://archive.cloudera.com/cdh5/redhat/5/x86_64/cdh/cloudera-cdh5.repo -O /etc/yum.repos.d/cloudera-cdh5.repo  && \
    yum clean all && yum -y install hadoop-client

#
# log4cxx required development libraries
#
RUN yum install -y apr apr-util apr-devel apr-util-devel

#
# for gdal include headers (and mr4c build will need this)
#
ENV CPLUS_INCLUDE_PATH /usr/include/gdal
ENV C_INCLUDE_PATH /usr/include/gdal

#
# download log4j src and compile it
#
RUN yum -y install libtool
WORKDIR /opt/build
RUN svn checkout http://svn.apache.org/repos/asf/incubator/log4cxx/trunk apache-log4cxx 
# ADD ftp://ftp.pbone.net/mirror/ftp5.gwdg.de/pub/opensuse/repositories/home:/monkeyiq:/centos6updates/CentOS_CentOS-6/noarch/autoconf-2.69-12.2.noarch.rpm /tmp/
ADD extra-dependencies/autoconf-2.69-12.2.noarch.rpm /tmp/
# ADD ftp://ftp.pbone.net/mirror/ftp5.gwdg.de/pub/opensuse/repositories/home:/monkeyiq:/centos6updates/CentOS_CentOS-6/noarch/automake-1.13.4-3.2.noarch.rpm /tmp/
ADD extra-dependencies/automake-1.13.4-3.2.noarch.rpm /tmp/
RUN yum -y --nogpgcheck localinstall /tmp/auto*noarch.rpm
RUN cd apache-log4cxx && \
   ./autogen.sh &&  \
   ./configure && \
   make && \
   make install && \
   echo /usr/local/lib > /etc/ld.so.conf.d/usrlocal.conf && \
   ldconfig

#
# download mr4c
#
RUN mkdir /opt/build/mr4c
WORKDIR /opt/build/mr4c
RUN git clone https://github.com/google/mr4c.git .

#
# build and install mr4c
#

RUN ./build_all 
RUN ./deploy_all

VOLUME ["/opt/projects"]
