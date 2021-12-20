FROM ubuntu:18.04

LABEL authors="Luca Quartesan"

### mitsuba dependencies
RUN apt-get update \
    && apt-get install -y \
    build-essential \
    libcollada-dom-dev \
    scons \
    git \
    qt4-dev-tools \
    libpng-dev \
    libjpeg-dev \
    libilmbase-dev \
    libxerces-c-dev \
    libboost-all-dev \
    libopenexr-dev \
    libglewmx-dev \
    libxxf86vm-dev \
    libpcrecpp0v5 \
    libeigen3-dev \
    libfftw3-dev \
    wget \
    && apt-get clean \
    && apt-get autoclean \
    && apt-get autoremove

## anaconda
ENV PATH="/root/miniconda3/bin:${PATH}"
RUN apt-get update
RUN apt-get install -y wget && rm -rf /var/lib/apt/lists/*
RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-py37_4.10.3-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-py37_4.10.3-Linux-x86_64.sh -b \
    && rm -f Miniconda3-py37_4.10.3-Linux-x86_64.sh
RUN conda install boost -y

# build mitsuba
WORKDIR /mitsuba
RUN git clone https://www.github.com/Quartenia/mitsuba.git
WORKDIR /mitsuba/mitsuba
RUN cp build/config-linux-gcc.py config.py \
    && scons -j8

# set env variables as setpath.sh
ENV MITSUBA_DIR="/mitsuba/mitsuba"
ENV PYTHONPATH="/mitsuba/mitsuba/dist/python:/mitsuba/mitsuba/dist/python/3.7:$PYTHONPATH"
ENV PATH="/mitsuba/mitsuba/wrapper:/mitsuba/mitsuba/dist:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:$PATH"
ENV LD_LIBRARY_PATH="/root/miniconda3/lib/:/mitsuba/mitsuba/dist:$LD_LIBRARY_PATH"