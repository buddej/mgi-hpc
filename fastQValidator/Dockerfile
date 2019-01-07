FROM phusion/baseimage:0.11
MAINTAINER John Budde <budde@wustl.edu>

LABEL Image for fastQValidator (Abecasis)

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends \
       git \
       make \
       gcc \
       g++ \
       libc-dev \
       libz-dev \
       libnss-sss

WORKDIR /tmp
RUN git clone https://github.com/statgen/libStatGen.git \
  && git clone https://github.com/statgen/fastQValidator.git \
  && cd libStatGen \
  && make all \
  && cd ../fastQValidator \
  && make all

# No longer need cleanup since multi-state build
# RUN cd / \
#   && rm -rf /tmp/* \
#   && apt-get autoremove -y \
#   && apt-get autoclean -y \
#   && apt-get clean \
#   && rm -rf /var/lib/apt/lists/* \

FROM phusion/baseimage:0.11
COPY --from=0 /tmp/fastQValidator/bin/fastQValidator /usr/bin/fastQValidator
RUN apt-get update -y \
  && apt-get install -y --no-install-recommends \
       libnss-sss

CMD ["fastQValidator","--file"]
