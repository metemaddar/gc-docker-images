FROM python:3.9

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# hadolint ignore=DL3008
RUN apt-get update --allow-releaseinfo-change -q && \
    apt-get install -q -y --no-install-recommends \
    bzip2 \
    ca-certificates \
    git \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    mercurial \
    openssh-client \
    procps \
    subversion \
    wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


RUN apt-get update && \
    apt-get install -y software-properties-common && \
    rm -rf /var/lib/apt/lists/*

#Install osm2pgrouting
RUN cd /tmp/ && wget http://ftp.de.debian.org/debian/pool/main/b/boost1.74/libboost-program-options1.74.0_1.74.0-9_amd64.deb \
    && dpkg -i  libboost-program-options1.74.0_1.74.0-9_amd64.deb \
    && wget http://ftp.de.debian.org/debian/pool/main/libp/libpqxx/libpqxx-6.4_6.4.5-2_amd64.deb \
    && dpkg -i libpqxx-6.4_6.4.5-2_amd64.deb \
    && wget http://ftp.br.debian.org/debian/pool/main/o/osm2pgrouting/osm2pgrouting_2.3.7-1_amd64.deb \
    && dpkg -i osm2pgrouting_2.3.7-1_amd64.deb\
    && rm -rf /tmp/*


# Install Osmosis
RUN wget https://bretth.dev.openstreetmap.org/osmosis-build/osmosis-0.47.tgz \
    && mkdir osmosis \
    && mv osmosis-0.47.tgz osmosis \
    && cd osmosis \
    && tar xvfz osmosis-0.47.tgz \
    && rm osmosis-0.47.tgz \
    && chmod a+x bin/osmosis

# Install osm2pgsql
# RUN sudo add-apt-repository -y ppa:osmadmins/ppa

RUN add-apt-repository -y -r ppa:osmadmins/ppa \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv A438A16C88C6BE41CB1616B8D57F48750AC4F2CB \
    && apt-get update -q \
    && apt-get install -y osm2pgsql


# Install osmCtools
RUN apt-get install -y osmctools

# Install osmium
RUN apt-get install -y osmium-tool

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | POETRY_HOME=/opt/poetry python && \
    cd /usr/local/bin && \
    ln -s /opt/poetry/bin/poetry && \
    poetry config virtualenvs.create false

# Install OpenJDK-11
RUN apt-get update && \
    apt-get install -y openjdk-11-jre-headless && \
    apt-get clean;

# Install GDAL
RUN apt-get install -y gdal-bin 


# Install libpostal
WORKDIR /tmp/
RUN apt-get install -y curl autoconf automake libtool python-dev pkg-config
RUN curl -sL https://github.com/openvenues/libpostal/archive/refs/tags/v1.1.tar.gz | tar -xz
WORKDIR /tmp/libpostal-1.1/
RUN ./bootstrap.sh
RUN ./configure
RUN make -j4
RUN make install
RUN ldconfig
WORKDIR /tmp/
RUN rm -rf /tmp/libpostal-1.1