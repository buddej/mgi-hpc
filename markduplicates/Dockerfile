FROM openjdk:8-jre
LABEL maintainer="John Budde <budde@wustl.edu>"

LABEL org.label-schema.schema-version="1.0"
# LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="buddej/markduplicates"
LABEL org.label-schema.description="Image for picard"

ENV PICARD_VERSION 2.18.21

WORKDIR /tmp

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
  && wget -q -O /usr/bin/picard.jar https://github.com/broadinstitute/picard/releases/download/${PICARD_VERSION}/picard.jar \
  && ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime \
  && echo "America/Chicago" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && apt-get clean all \
  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

ENV PICARD /usr/bin/picard.jar

COPY ./entrypoint.sh /usr/local/bin/

# This can to be written with ENTYRPOINT for running with LSF/bsub, since at least 1 .bam file MUST be specified as a command-line argument
#   bsub just needs a "command" to run -- it doesn't check to see if it is actually executable
# Input files have to be listed this way, since docker will not take a bash array as an env variable
# Usage examples: 
#     sudo docker run -it buddej/markduplicates:0.1.0 /input/file1.bam /input/file2.bam
#     bsub -Is -q research-hpc -a "docker(buddej/markduplicates:0.1.0)" /input/file1.bam /input/file.bam

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
