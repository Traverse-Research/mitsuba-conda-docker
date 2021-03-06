FROM ubuntu:20.04

LABEL authors="Luca Quartesan - luca@traverseresearch.nl"
# avoids tz to request your timezone
#test that even without settig the timezone all works

ARG DEBIAN_FRONTEND=noninteractive

### mitsuba dependencies
RUN apt-get update \
    && apt-get install -y \
    build-essential \
    qt5-default \ 
    libqt5opengl5-dev \
    libqt5xmlpatterns5-dev \
    libcollada-dom-dev \
    git \
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
    && apt-get clean \
    && apt-get autoclean \
    && apt-get autoremove

## anaconda


RUN apt-get update
RUN apt-get install -y wget && rm -rf /var/lib/apt/lists/*
RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-py37_4.10.3-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-py37_4.10.3-Linux-x86_64.sh -b \
    && rm -f Miniconda3-py37_4.10.3-Linux-x86_64.sh
ENV PATH="/root/miniconda3/bin:${PATH}"
RUN conda install boost scons -y

# build mitsuba
WORKDIR /mitsuba
RUN git clone --branch scons-python3 https://www.github.com/Quartenia/mitsuba.git
WORKDIR /mitsuba/mitsuba
RUN cp build/config-linux-gcc.py config.py \
    && scons -j8

# set env variables as setpath.sh
ENV MITSUBA_DIR="/mitsuba/mitsuba"
ENV PYTHONPATH="/mitsuba/mitsuba/dist/python:/mitsuba/mitsuba/dist/python/3.7:$PYTHONPATH"
#TODO: the wrapper here is used to connect to a device
ENV PATH="/mitsuba/mitsuba/dist:$PATH"
ENV LD_LIBRARY_PATH="/root/miniconda3/lib/:/mitsuba/mitsuba/dist:$LD_LIBRARY_PATH"