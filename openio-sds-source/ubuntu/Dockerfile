FROM ubuntu:16.04

ARG TAG_OIO_SDS=master
ARG TAG_SWIFT3=master
ARG TAG_SWIFT=stable/ocata
ARG TAG_OIO_SWIFT=master

ENV SDS /home/openio/

RUN apt-get update && \
    apt-get install -y curl && \
    echo "deb http://mirror.openio.io/pub/repo/openio/sds/16.10/Ubuntu/ xenial/" > /etc/apt/sources.list.d/openio-sds.list && \
    curl http://mirror.openio.io/pub/repo/openio/APT-GPG-KEY-OPENIO-0 | apt-key add - && \
    apt-get update && \
    apt-get install -y \
        cmake \
        build-essential \
        git \
        memcached \
        flex bison \
        libcurl4-gnutls-dev \
        libglib2.0-dev \
        libapreq2-dev \
        libsqlite3-dev \
        libjson-c-dev \
        apache2 \
        apache2-dev \
        libapache2-mod-wsgi \
        liblzo2-dev \
        libffi-dev \
        libzmq3-dev \
        libattr1-dev \
        libzookeeper-mt-dev \
        libisal2 \
        libjerasure2 \
        openio-asn1c \
        openio-gridinit \
        liberasurecode-dev \
        python-dev \
        python-pbr \
        python-setuptools \
        libleveldb-dev \
        python-virtualenv \
        libssl-dev \
        beanstalkd \
        redis-server \
        python-pip && \
    apt-get clean && \
    adduser openio

RUN su - openio -c "\
    cd /home/openio && \
    virtualenv oio && \
    . oio/bin/activate && \
    echo 'export LD_LIBRARY_PATH=/home/openio/lib:$LD_LIBRARY_PATH' >> .profile && \
    git clone https://github.com/open-io/oio-sds.git -b $TAG_OIO_SDS && \
    mkdir build && \
    cd build && \
    cmake \
        -DCMAKE_INSTALL_PREFIX=${SDS} \
        -DLD_LIBDIR=lib \
        -DAPACHE2_MODDIR=${SDS}/lib/apache2 \
        -DAPACHE2_LIBDIR=/usr/lib/apache2 \
        -DAPACHE2_INCDIR=/usr/include/apache2 \
        ../oio-sds && \
    make && \
    make install && \
    cd ../oio-sds && \
    python setup.py install && \
    pip install -r all-requirements.txt && \
    pip install requests \
    "

RUN su - openio -c "\
    cd /home/openio && \
    . oio/bin/activate && \
    git clone https://github.com/openstack/swift.git -b $TAG_SWIFT && \
    cd swift && python setup.py install && cd .. \
    "

RUN su - openio -c "\
    cd /home/openio && \
    . oio/bin/activate && \
    git clone https://github.com/open-io/swift3.git -b $TAG_SWIFT3 && \
    cd swift3 && python setup.py install && cd .. && \
    pip install lxml && \
    git clone https://github.com/open-io/oio-swift.git -b $TAG_OIO_SWIFT && \
    cd oio-swift && python setup.py install && cd .. \
    "


COPY swift_s3.cfg /home/openio

EXPOSE 5000

CMD exec \
    service beanstalkd start & \
    service memcached start & \
    su - openio -c "\
    cd /home/openio && \
    . oio/bin/activate && \
    oio-reset.sh -v -f ./oio-sds/etc/bootstrap-preset-SINGLE.yml && \
    cd /home/openio/oio-swift && \
    python runserver.py ../swift_s3.cfg -v \
    "

