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

## Download and install JAVA
RUN mkdir -p /opt/JAVA
WORKDIR /opt/JAVA

RUN wget \
    --no-check-certificate \
    --no-cookies \
    --header "Cookie: oraclelicense=accept-securebackup-cookie" \
    http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz && \
    tar xf jdk-7u79-linux-x64.tar.gz* && \
    ln -s jdk1.7.0_79 java7

## Download ad compile GCC 4.7
ENV GCC_FOLDER=/opt/GCC/gcc-4.7.2
ENV GCC_BUILD_FOLDER=$GCC_FOLDER/BUILD
ENV GCC_BIN_FOLDER=$GCC_FOLDER/BIN
ENV GCC_SRC_FOLDER=$GCC_SRC_FOLDER/SRC
ENV GCC_PREFIX=$GCC_BIN_FOLDER

RUN mkdir -p $GCC_FOLDER

WORKDIR $GCC_FOLDER
RUN wget http://ftp.gnu.org/gnu/gmp/gmp-6.1.0.tar.bz2 && \
    tar xf gmp-6.1.0.tar.bz2
WORKDIR gmp-6.1.0
RUN mkdir -p $GCC_FOLDER/gmp-6.1.0/build
WORKDIR $GCC_FOLDER/gmp-6.1.0/build

RUN ../configure --prefix=$GCC_PREFIX --build=x86_64-linux-gnu && \
    make -j 6 && \
    make install

WORKDIR $GCC_FOLDER
RUN wget http://www.mpfr.org/mpfr-3.1.4/mpfr-3.1.4.tar.gz && \
    tar xf mpfr-3.1.4.tar.gz
WORKDIR $GCC_FOLDER/mpfr-3.1.4
RUN mkdir build 
WORKDIR $GCC_FOLDER/mpfr-3.1.4/build
RUN ../configure --build=x86_64-linux-gnu --prefix=$GCC_PREFIX --with-gmp=$GCC_PREFIX && \
    make -j 6 && \
    make install

WORKDIR $GCC_FOLDER
RUN wget ftp://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz && \
    tar xf  mpc-1.0.3.tar.gz
WORKDIR $GCC_FOLDER/mpc-1.0.3
RUN mkdir build
WORKDIR $GCC_FOLDER/mpc-1.0.3/build
RUN ../configure --build=x86_64-linux-gnu --prefix=$GCC_PREFIX --with-gmp=$GCC_PREFIX --with-mpfr=$GCC_PREFIX && \
    make -j 6 && \
    make install

RUN mkdir -p $GCC_BUILD_FOLDER && \
    mkdir -p $GCC_BIN_FOLDER

WORKDIR $GCC_FOLDER
RUN wget ftp.gnu.org/gnu/gcc/gcc-4.7.4/gcc-4.7.4.tar.gz && \
    tar xf gcc-4.7.4.tar.gz && \
    mv gcc-4.7.4 SRC

WORKDIR $GCC_BUILD_FOLDER
ENV LD_LIBRARY_PATH=$GCC_PREFIX/lib:$LD_LIBRARY_PATH
ENV LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/
ENV C_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
ENV CPLUS_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
RUN ../SRC/configure --build=x86_64-linux-gnu \
					 --prefix=$GCC_PREFIX \
					 --with-gmp=$GCC_PREFIX \
					 --with-mpfr=$GCC_PREFIX \
					 --with-mpc=$GCC_PREFIX \
					 --enable-checking=release \
					 --enable-languages=c,c++,fortran \
					 --disable-multilib \
					 --program-suffix=-4.7 && \
    make -j 6 && \
    ln -s /usr/lib/x86_64-linux-gnu /usr/lib64 && \
    make install

WORKDIR $GCC_FOLDER
RUN cp -r BIN GCC && \
	tar cvfj GCC.tar.bz2 GCC

ENV PATH=$PATH:$GCC_PREFIX/bin
ENV CXX=$GCC_PREFIX/bin/g++-4.7
ENV CC=$GCC_PREFIX/bin/gcc-4.7

## Download and compile python 2.5.2
RUN mkdir /opt/PYTHON
WORKDIR /opt/PYTHON
RUN wget http://www.python.org/ftp/python/2.5.2/Python-2.5.2.tgz && \
    tar xf Python-2.5.2.tgz
WORKDIR /opt/PYTHON/Python-2.5.2
RUN ./configure && \
    make -j 6 && \
    make install

## Download and compile LLVM
RUN mkdir -p /opt/LLVM-CLANG/BIN
WORKDIR /opt/LLVM-CLANG
RUN wget http://llvm.org/releases/3.4/llvm-3.4.src.tar.gz && \
    tar xf llvm-3.4.src.tar.gz && \
    mkdir build 
WORKDIR /opt/LLVM-CLANG/build
RUN ./../llvm-3.4/configure --enable-shared --prefix=/opt/LLVM-CLANG/BIN && \
    make -j 6 && \
    make install
WORKDIR /opt/LLVM-CLANG
RUN cp -r BIN LLVM && \
    tar cvfj LLVM.tar.bz2 LLVM


## Download and compile OSMesa
RUN mkdir -p /opt/OSMESA/BIN
WORKDIR /opt/OSMESA
RUN wget ftp://ftp.freedesktop.org/pub/mesa/11.1.1/mesa-11.1.1.tar.gz && \
    tar xf mesa-11.1.1.tar.gz
WORKDIR /opt/OSMESA/mesa-11.1.1

RUN ./configure \
    --disable-xvmc \
    --disable-glx \
    --disable-dri \
    --with-dri-drivers= \
    --with-gallium-drivers=swrast \
    --enable-texture-float \
    --disable-egl \
    --with-egl-platforms= \
    --enable-gallium-osmesa \
    --with-llvm-prefix=/opt/LLVM-CLANG/BIN \
    --enable-llvm-shared-libs \
    --enable-gallium-llvm=yes \
    --prefix=/opt/OSMESA/BIN && \
    
    make -j 8 && \
    make install

WORKDIR /opt/OSMESA
RUN cp -r BIN OSMesa && \
    tar cvfj OSMesa.tar.bz2 OSMesa

## Download and compile MPI

RUN mkdir -p /opt/MPI
WORKDIR /opt/MPI
RUN wget --no-check-certificate http://www.open-mpi.org/software/ompi/v1.7/downloads/openmpi-1.7.4.tar.gz && \
    tar xf openmpi-1.7.4.tar.gz
WORKDIR /opt/MPI/openmpi-1.7.4
RUN mkdir BIN && \
    ./configure --enable-mpi-java \
    			--with-jdk-bindir=/opt/JAVA/java7/bin \	
    			--with-jdk-headers=/opt/JAVA/java7/include \
    			--disable-dlopen \
    			--disable-mca-dso \
    			--prefix=/opt/MPI/openmpi-1.7.4/BIN \  
    			CFLAGS=-fgnu89-inline && \
	make -j 6 && \
	make install 

WORKDIR /opt/MPI/openmpi-1.7.4
RUN cp -r BIN MPI && \
    mv MPI /opt/MPI
WORKDIR /opt/MPI
RUN tar cvfj MPI.tar.bz2 MPI 

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
