FROM phusion/baseimage:0.11
MAINTAINER John Budde <budde@wustl.edu>

LABEL Image for trickle per mcallaway

RUN apt-get update -y \
  && apt-get dist-upgrade -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       ssh \
       rsync \
       wget \
       curl \
       trickle \
  && apt-get clean all \
  && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
