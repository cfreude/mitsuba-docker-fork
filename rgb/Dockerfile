# apt works but compile fails
FROM ubuntu:16.04

# apt works but compile fails
#FROM ubuntu:18.04

# some apt 404
#FROM ubuntu:19.04

# Unable to locate package qt4-dev-tools
#FROM ubuntu:20.04

#FROM ubuntu:23.10

LABEL authors="Ben Heasly <benjamin.heasly@gmail.com>, Marceli Wac <marceliwac@gmail.com>"

### headless X server dependencies
#RUN apt-get update \
#    && apt-get install -y \
#    libx11-dev \
#    libxxf86vm-dev \
#    x11-xserver-utils \
#    x11proto-xf86vidmode-dev \
#    x11vnc \
#    xpra \
#    xserver-xorg-video-dummy \
#    && apt-get clean \
#    && apt-get autoclean \
#    && apt-get autoremove

# GCC 6
#RUN apt-get update && \
#    apt-get install build-essential software-properties-common -y && \
#    add-apt-repository ppa:ubuntu-toolchain-r/test -y && \
#    apt-get update && \
#    apt-get install gcc-snapshot -y && \
#    apt-get update && \
#    apt-get install gcc-6 g++-6 -y && \
#    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 60 --slave /usr/bin/g++ g++ /usr/bin/g++-6 && \
#    apt-get install gcc-4.8 g++-4.8 -y && \
#    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8;

### mitsuba dependencies
RUN apt-get update \
    && apt-get install -y \
    build-essential \
    scons \
    git \
    qt4-dev-tools \
    #libpng12-dev \
    libpng-dev \
    libjpeg-dev \
    libilmbase-dev \
    libxerces-c-dev \
    libboost-all-dev \
    libopenexr-dev \
    libglewmx-dev \
    libxxf86vm-dev \
    #libpcrecpp0 \
    libpcrecpp0v5 \
    libeigen3-dev \
    libfftw3-dev \
    libcollada-dom-dev \
    #gcc-6.3 \
    wget \
    && apt-get clean \
    && apt-get autoclean \
    && apt-get autoremove

ENTRYPOINT ["tail", "-f", "/dev/null"]
WORKDIR /mitsuba-ppg

#RUN wget https://ftp.debian.org/debian/pool/main/c/collada-dom/libcollada-dom-dev_2.5.0+ds1-1+b2_amd64.deb \
#    && wget https://ftp.debian.org/debian/pool/main/c/collada-dom/libcollada-dom2.4-dp0_2.4.4+ds1-2+b3_amd64.deb \
#    && dpkg --install collada-dom*.deb

### build Mitsuba
#RUN git clone https://www.github.com/mitsuba-renderer/mitsuba.git
RUN git clone https://github.com/cfreude/practical-path-guiding-fork.git
#RUN cd mitsuba && git checkout 2f9bf64cb49c2b4c1c945e73c50c1c0da9507add && cd ..

ENV MITSUBA_DIR /mitsuba-ppg/practical-path-guiding-fork/mitsuba

WORKDIR /mitsuba-ppg/practical-path-guiding-fork/mitsuba/

COPY ./config-linux-gcc.py $(MITSUBA_DIR)/build/

RUN cp build/config-linux-gcc.py config.py && scons

### set up Mitsuba lib and bin paths, like in Mitsuba's own setpath.sh
ENV PYTHONPATH $(MITSUBA_DIR)/dist/python:/mitsuba/mitsuba/dist/python/2.7:
ENV PATH $(MITSUBA_DIR)/wrapper:$(MITSUBA_DIR)/dist:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LD_LIBRARY_PATH $(MITSUBA_DIR)/dist:

### configure headless X server
### this allows the Mitsba scene importer "mtsimport" to use OpenGL for triagle computations
#COPY xorg.conf /etc/X11/xorg.conf
#ENV DISPLAY :0

### wrapper to start headless xserver when using mtsimport
COPY ./mtsimport-headless.sh $(MITSUBA_DIR)/wrapper/mtsimport

#CMD ["bash"]
