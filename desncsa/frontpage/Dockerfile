# Dockerfile for des_ncsa
# Matias Carrasco Kind <mgckind@gmail.com>
FROM python:3
# Basic python reqs
RUN pip install --no-cache-dir tornado jira netaddr bcrypt
RUN useradd --create-home --shell /bin/bash des --uid 1001
# Copy code
WORKDIR /home/des
USER des
# clone repo branch y1
RUN git clone -b y1 https://github.com/mgckind/des_ncsa.git
WORKDIR /home/des/des_ncsa
CMD [ "python", "main.py" ]
