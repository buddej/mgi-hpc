FROM python:2.7.16-slim-buster as builder

ENV PYTHON_VERSION 2.17.16
ENV DCC_VERSION 0.4.7
ENV HTSLIB_VERSION 1.9

WORKDIR /tmp

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
       tzdata \
       wget \
       bzip2 \
       make \
       g++ \
       libz-dev \
       libbz2-dev \
       liblzma-dev \
       libcurl4-openssl-dev \
       libssl-dev \
       zlib1g-dev \
  && apt-get clean all \
  && rm -rf /var/lib/apt/lists/*

# Build htslib for pysam; download DCC so wget isn't required later
RUN wget -q https://github.com/samtools/htslib/releases/download/${HTSLIB_VERSION}/htslib-${HTSLIB_VERSION}.tar.bz2 \
  && wget -q https://github.com/dieterich-lab/DCC/archive/v${DCC_VERSION}.tar.gz \
  && tar xjvf htslib-${HTSLIB_VERSION}.tar.bz2 \
  && cd htslib-${HTSLIB_VERSION} \
  && make \
  && cd .. \
  && rm -rf htslib-${HTSLIB_VERSION}*

# Use wheel so python packages can be installed quickly later
COPY ./requirements.txt /tmp
RUN pip install -U pip \
  && pip wheel -r ./requirements.txt \
  && pip install --no-index --find-links=/tmp/ -r ./requirements.txt


# Multi-stage build
FROM python:2.7.16-slim-buster

LABEL maintainer="John Budde <budde@wustl.edu>"

LABEL org.label-schema.schema-version="1.0"
# LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="buddej/dcc"
LABEL org.label-schema.description="Image for DCC using python 2.7.16"

WORKDIR /tmp

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
       tzdata \
       libcurl4-openssl-dev \
  && apt-get clean all \
  && rm -rf /var/lib/apt/lists/*

ENV DCC_VERSION 0.4.7
COPY --from=builder /tmp /tmp
RUN pip install -U pip \
  && pip install --no-cache-dir -r /tmp/requirements.txt -f /tmp \
  && tar xzvf v${DCC_VERSION}.tar.gz \
  && rm -fv v${DCC_VERSION}.tar.gz \
  && cd DCC-${DCC_VERSION}/ \
  && python setup.py install \
  && cd \
  && rm -rf /tmp/ \
  && rm -rf /root/.cache/pip/*

ENV PYTHON_VERSION 2.17.16
ENV HTSLIB_VERSION 1.9
ENV TZ "America/Chicago"

CMD ["/bin/bash"]
