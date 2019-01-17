FROM phusion/baseimage:0.11 as builder
MAINTAINER John Budde <budde@wustl.edu>

LABEL Image for samtools

ENV SAMTOOLS_VERSION 1.9

WORKDIR /tmp

#       python \
# RUN apt-get update -y \
#   && apt-get install --no-install-recommends -y \
#       make \
#2#2B2D#B       gcc \
#       g++ \
#       libz-dev \
#       libbz2-dev \
#2D2B#       liblzma-dev \
# 2D#      ncurses-dev \#
#       libnss-sss \
 #      time \

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       wget \
       make \
       g++ \
       ncurses-dev \
       libz-dev \
       libbz2-dev \
       liblzma-dev \
  && wget -q https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSION}/samtools-${SAMTOOLS_VERSION}.tar.bz2 \
  && tar xjvf samtools-${SAMTOOLS_VERSION}.tar.bz2 \
  && cd /tmp/samtools-${SAMTOOLS_VERSION}/ \
  && make

#  && cp -av /tmp/samtools-${SAMTOOLS_VERSION}/samtools /usr/bin/ \
#  && ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime \
#  && echo "America/Chicago" > /etc/timezone \
#  && dpkg-reconfigure --frontend noninteractive tzdata \
#  && apt-get clean all \
#  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

FROM phusion/baseimage:0.11
RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
       tzdata \
  && apt-get clean all \
  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

ENV SAMTOOLS_VERSION 1.9
ENV TZ "America/Chicago"

COPY --from=builder /tmp/samtools-${SAMTOOLS_VERSION}/samtools  /usr/bin/
COPY ./entrypoint.sh /usr/local/bin/

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
