#Suggested name: centos5-environment:gcc

FROM engys/centos5-environment:base
MAINTAINER ENGYS <engys@engys.com>

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
