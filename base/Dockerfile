#Suggested name: centos5-environment

FROM centos:5
MAINTAINER ENGYS <engys@engys.com>

RUN yum -y update && \
	yum -y groupinstall "Development Tools" && \
    yum install -y \
    wget \
    cmake \
    ncurses-devel \
    freeglut-devel \
    libX11-devel \
    libXmu-devel \
    libXi-devel \
    libXt-devel \
    libXtst-devel \ 
    libXrender

## JAVA
RUN mkdir -p /opt/JAVA
WORKDIR /opt/JAVA

RUN wget \
    --no-check-certificate \
    --no-cookies \
    --header "Cookie: oraclelicense=accept-securebackup-cookie" \
    http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz && \
    tar xf jdk-7u79-linux-x64.tar.gz* && \
    ln -s jdk1.7.0_79 java7

## GIT
RUN rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm && \
    yum install -y git-core

## CMAKE
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$GCC_PREFIX/lib:$GCC_PREFIX/lib64:$GCC_PREFIX/libexec
RUN mkdir -p /opt/CMAKE/BIN
WORKDIR /opt/CMAKE
RUN wget --no-check-certificate http://www.cmake.org/files/v2.8/cmake-2.8.10.2.tar.gz && \
    tar xf cmake-2.8.10.2.tar.gz
WORKDIR /opt/CMAKE/cmake-2.8.10.2
RUN ./bootstrap  --prefix=/opt/CMAKE/BIN && \
	make -j 6  && \
    make install
