# Matias Carrasco Kind
#
# DESACCESS
#

FROM ubuntu:18.04
MAINTAINER Matias Carrasco Kind <mgckind@gmail.com>

#ENVS
ENV HOME /root
ENV SHELL /bin/bash
ENV TERM xterm
ENV DEBIAN_FRONTEND noninteractive


# BASICS
RUN apt-get update
RUN apt-get install -y git  curl nano wget dialog net-tools build-essential vim unzip libaio1 pkg-config gfortran zlibc zlib1g zlib1g-dev expect strace screen supervisor
RUN apt-get install -y  bzip2 ca-certificates curl git imagemagick libhdf5-serial-dev hdf5-tools python3-dev libmysqlclient-dev mysql-client


# REDIS 
RUN \
  cd /tmp && \
  wget http://download.redis.io/redis-stable.tar.gz && \
  tar xvzf redis-stable.tar.gz && \
  cd redis-stable && \
  make && \
  make install && \
  cp -f src/redis-sentinel /usr/local/bin 


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
RUN pip install tornado
RUN pip install celery
RUN pip install redis cython tables --ignore-installed
RUN pip install -U celery[redis]
RUN pip install -U flower
RUN pip install  Pillow
RUN pip install  pymongo
RUN pip install  pyfits
RUN pip install  prettytable
RUN pip install git+https://github.com/mailgun/expiringdict.git
RUN pip install flower
RUN pip install mysqlclient
RUN pip install schedule 
RUN pip install pyyaml
RUN pip install pycrypto
RUN pip install jsmin
RUN pip install matplotlib
RUN pip install jinja2
RUN pip install jira --ignore-installed
RUN pip install astropy
RUN pip install  kubernetes

ENV C_FORCE_ROOT true

RUN useradd --create-home --shell /bin/bash des --uid 1001
WORKDIR /home/des
RUN mkdir -p /home/des/desaccess
ADD desaccess /home/des/desaccess 
RUN mkdir -p /home/des/desaccess/easyweb/static/internal/emails
RUN mkdir -p /home/des/flower
RUN chown -R des:des /home/des
EXPOSE 8080
EXPOSE 5555
#ADD config /des/desaccess/config 
#CMD ["/usr/bin/supervisord"]
#CMD ["/opt/conda/bin/python, /des/desaccess/public.py"]
#CMD ["/opt/conda/bin/python", "/des/desaccess/deslabs.py"]
USER des
ENV PATH /opt/conda/bin:$PATH
ENV HOME /home/des
ENV C_FORCE_ROOT true
ENV SHELL /bin/bash
ENV TERM xterm
WORKDIR /home/des/desaccess
CMD ["sleep", "3600"]
