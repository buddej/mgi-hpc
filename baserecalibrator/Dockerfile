FROM openjdk:8-jre
LABEL maintainer="John Budde <budde@wustl.edu>"

LABEL org.label-schema.schema-version="1.0"
# LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="buddej/baserecalibrator"
LABEL org.label-schema.description="Image for GATK BaseRecalibrator"

WORKDIR /tmp

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
  && ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime \
  && echo "America/Chicago" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && apt-get clean all \
  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

ENV TZ "America/Chicago"
COPY ./GenomeAnalysisTK.jar ./Capture_*.GRCh37.bed /usr/local/lib/
ENV GATK_VERSION 3.6-0
ENV GATK360 /usr/local/lib/GenomeAnalysisTK.jar
ENV COVERED_BED /usr/local/lib/Capture_Covered.GRCh37.bed
ENV PADDED_BED /usr/local/lib/Capture_Padded.GRCh37.bed

COPY ./entrypoint.sh /usr/local/bin/

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
