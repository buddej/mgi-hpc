FROM openjdk:8-jre
MAINTAINER John Budde <budde@wustl.edu>

LABEL Image for bwa

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

# This makes the image crazy large -- will find a workaround
# COPY human_g1k_v37_decoy* /usr/local/

COPY ./entrypoint.sh /usr/local/bin/
ENV PICARD /usr/bin/picard.jar

# This has to be written with CMD for running with LSF/bsub, since you cannot bsub a job with no command specified (even a dockerapp)
# This allows passing of additional parameters (e.g. -IGNORE MATE_NOT_FOUND) if required
# Usage examples: 
#   sudo docker run -it buddej/validatesam:0.1.0
#   sudo docker run -it buddej/validatesam:0.1.0 entrypoint.sh -IGNORE MATE_NOT_FOUND
#   bsub -Is -q research-hpc -a "docker(buddej/validatesam:0.1.0)" entrypoint.sh
#   bsub -Is -q research-hpc -a "docker(buddej/validatesam:0.1.0)" entrypoint.sh -IGNORE MATE_NOT_FOUND

# ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["/usr/local/bin/entrypoint.sh"]
