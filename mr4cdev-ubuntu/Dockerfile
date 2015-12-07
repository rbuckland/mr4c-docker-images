FROM ubuntu:15.10

#
# Development tools
#
RUN apt-get -y update && apt-get -y install \
  git vim subversion wget zip unzip screen \
  openjdk-7-jdk openjdk-7-jre-headless openjdk-7-jre-lib \
  ant ant-optional ivy

#
# link Ant Ivy in
#
RUN ln -s -T /usr/share/java/ivy.jar /usr/share/ant/lib/ivy.jar

#
# install CDH Repo and install hadoop client
#
RUN wget http://archive.cloudera.com/cdh5/one-click-install/trusty/amd64/cdh5-repository_1.0_all.deb && \
    dpkg -i cdh5-repository_1.0_all.deb && \
    rm -f cdh5-repository_1.0_all.deb && \
    apt-get update && apt-get -y install hadoop-client

#
# mr4c required development libraries
#
RUN apt-get -y install \
  libgdal1-dev libgdal1h build-essential autoconf libproj-dev libproj0 libcppunit-dev libcppunit-1.13-0 libjansson-dev libjansson4  \
  libaprutil1 libaprutil1-dev libapr1-dev libapr1

#
# for gdal include headers (and mr4c build will need this)
#
ENV CPLUS_INCLUDE_PATH /usr/include/gdal
ENV C_INCLUDE_PATH /usr/include/gdal

#
# download log4j src and compile it
#
RUN mkdir -p /opt/build && cd /opt/build && svn checkout http://svn.apache.org/repos/asf/incubator/log4cxx/trunk apache-log4cxx && \
   cd apache-log4cxx && \
   ./autogen.sh &&  \
   ./configure && \
   make && \
   make install && \
   ldconfig

#
# download mr4c
#
RUN mkdir /opt/build/mr4c && \
    cd /opt/build/mr4c && \
    git clone https://github.com/google/mr4c.git .

#
# build and install mr4c
#
RUN cd /opt/build/mr4c && \
    ./build_all && \
    ./deploy_all

VOLUME ["/opt/projects"]
