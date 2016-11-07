#Suggested name: centos5-environment:mpi

FROM engys/centos5-environment:osmesa
MAINTAINER ENGYS <engys@engys.com>

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
