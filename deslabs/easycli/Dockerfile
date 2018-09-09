FROM ubuntu:18.04
MAINTAINER Matias Carrasco Kind <mgckind@gmail.com>

#ENVS
ENV HOME /root
ENV SHELL /bin/bash
ENV TERM xterm
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      ca-certificates \
      cmake \
      curl \
      g++ \
      git \
      vim \
      libaio1 \
      bzip2 \
      tmux \
      hdf5-tools \
      libhdf5-serial-dev \
      unzip \
      zlib1g \
      libjson-c3 \
      libjson-c-dev \
      libssl1.0.0 \
      libssl-dev \
      libwebsockets8 \
      libwebsockets-dev \
      pkg-config \
      vim-common \
    && git clone --depth=1 https://github.com/tsl0922/ttyd.git /tmp/ttyd \
    && cd /tmp/ttyd && mkdir build && cd build \
    && cmake -DCMAKE_BUILD_TYPE=RELEASE .. \
    && make \
    && make install \
    && apt-get remove -y --purge \
        cmake \
        g++ \
        libwebsockets-dev \
        libjson-c-dev \
        libssl-dev \
        pkg-config \
    && apt-get purge -y \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/ttyd


#CONDA
RUN apt-get -qq update && apt-get -qq -y install curl bzip2 \
    && curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -bfp /usr/local \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=3 \
    && conda update conda \
    && apt-get -qq -y remove curl bzip2 \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes

ENV PATH /opt/conda/bin:$PATH

RUN conda install  easyaccess -c mgckind 
RUN conda install -y pip 

#PYTHON 3
RUN pip install --upgrade pip
RUN pip install cx_Oracle==5.3
RUN pip install requests
RUN pip install easyaccess==1.4.4 --no-deps --ignore-installed

RUN useradd --create-home --shell /bin/bash des --uid 1001
WORKDIR /home/des

USER des
ENV PATH /opt/conda/bin:$PATH
ENV HOME /home/des
ENV SHELL /bin/bash
ENV TERM xterm

EXPOSE 7681

ENTRYPOINT ["ttyd"]

CMD ["bash"]
