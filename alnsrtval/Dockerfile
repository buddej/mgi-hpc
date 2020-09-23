FROM phusion/baseimage:0.11 as builder

ENV BWA_VERSION 0.7.17
ENV PICARD_VERSION 2.18.21
ENV SAMTOOLS_VERSION 1.9
ENV BEDTOOLS_VERSION 2.27.1

WORKDIR /tmp

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       wget \
       make \
       gcc \
       g++ \
       libz-dev \
       libbz2-dev \
       liblzma-dev \
       ncurses-dev \
       python \
  && wget -q -P /tmp https://github.com/lh3/bwa/releases/download/v${BWA_VERSION}/bwa-${BWA_VERSION}.tar.bz2 \
  && tar xjvf /tmp/bwa-${BWA_VERSION}.tar.bz2 -C /tmp \
  && cd /tmp/bwa-${BWA_VERSION}/ \
  && make \
  && wget -q -P /tmp https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSION}/samtools-${SAMTOOLS_VERSION}.tar.bz2 \
  && tar xjvf /tmp/samtools-${SAMTOOLS_VERSION}.tar.bz2 -C /tmp \
  && cd /tmp/samtools-${SAMTOOLS_VERSION}/ \
  && make \
  && wget -q -P /tmp https://github.com/arq5x/bedtools2/releases/download/v${BEDTOOLS_VERSION}/bedtools-${BEDTOOLS_VERSION}.tar.gz \
  && tar xzvf /tmp/bedtools-${BEDTOOLS_VERSION}.tar.gz -C /tmp \
  && cd /tmp/bedtools${BEDTOOLS_VERSION%%.*}/ \
  && make \
  && wget -q -O /tmp/picard.jar https://github.com/broadinstitute/picard/releases/download/${PICARD_VERSION}/picard.jar

# This makes the image crazy large -- will find a workaround
# COPY human_g1k_v37_decoy* /usr/local/

FROM openjdk:8-jre
LABEL maintainer="John Budde <budde@wustl.edu>"

LABEL org.label-schema.schema-version="1.0"
# LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="buddej/alnsrtval"
LABEL org.label-schema.description="Image for bwa align, samtools sort, picard validate, and more"

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
       tzdata \
  && ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime \
  && echo "America/Chicago" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && apt-get clean all \
  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

ENV BWA_VERSION 0.7.17
ENV SAMTOOLS_VERSION 1.9

COPY --from=builder /tmp/bwa-${BWA_VERSION}/bwa /usr/bin/
COPY --from=builder /tmp/samtools-${SAMTOOLS_VERSION}/samtools /usr/bin/
COPY --from=builder /tmp/bedtools*/bin/bedtools /usr/bin/
COPY --from=builder /tmp/picard.jar /usr/bin/

COPY ./Capture_*.GRCh37.bed /usr/local/lib/
COPY ./entrypoint.sh /usr/local/bin/

ENV COVERED_BED /usr/local/lib/Capture_Covered.GRCh37.bed
ENV PADDED_BED /usr/local/lib/Capture_Padded.GRCh37.bed
ENV BEDTOOLS_VERSION 2.27.1
ENV PICARD_VERSION 2.18.21
ENV PICARD /usr/bin/picard.jar

# ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["/bin/bash"]
