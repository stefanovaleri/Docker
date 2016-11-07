#Suggested name: centos5-environment:osmesa

FROM engys/centos5-environment:gcc
MAINTAINER ENGYS <engys@engys.com>

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
