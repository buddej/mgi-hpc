FROM phusion/baseimage:0.11 as builder

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends \
       git \
       make \
       g++ \
       libc-dev \
       libz-dev

WORKDIR /tmp
RUN git clone https://github.com/statgen/libStatGen.git \
  && git clone https://github.com/statgen/fastQValidator.git \
  && cd libStatGen \
  && make all \
  && cd ../fastQValidator \
  && make all

FROM phusion/baseimage:0.11
LABEL maintainer="John Budde <budde@wustl.edu>"

ARG BUILD_DATE
ARG VCS_REF
ARG BUILD_VERSION

LABEL org.label-schema.schema-version="1.0"
LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="buddej/fastqvalidator"
LABEL org.label-schema.description="Image for fastQValidator (Abecasis)"
LABEL org.label-schema.vcs-url="https://github.com/buddej/mgi-hpc/tree/master/fastqvalidator"
LABEL org.label-schema.vcs-ref=$VCS_REF
LABEL org.label-schema.version=$BUILD_VERSION

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
       tzdata \
  && apt-get clean all \
  && rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/* 

ENV TZ "America/Chicago"

COPY --from=builder /tmp/fastQValidator/bin/fastQValidator /usr/bin/
# COPY ./entrypoint.sh /usr/local/bin/

# This has to be written with CMD for running with LSF/bsub, since you cannot bsub a job with no command specified (even a dockerapp)
# This allows passing of additional parameters (e.g. --noeof) if required

# Usage examples (rough -- these do not address passing in input file via ENV variable)
#   Validating a normal .fq.gz
#     sudo docker run -it buddej/fastqvalidator:0.1.6 
#     bsub -Is -q research-hpc -a "docker(buddej/fastqvalidator:0.1.6)" entrypoint.sh
#   Validating an .fq.gz missing the BGZF EOF marker
#     sudo docker run -it buddej/fastqvalidator:0.1.6 entrypoint.sh --noeof
#     bsub -Is -q research-hpc -a "docker(buddej/fastqvalidator:0.1.6)" entrypoint.sh --noeof

# ENTRYPOINT ["fastQValidator","--noeof","--file"]
# CMD ["/usr/local/bin/entrypoint.sh"]
CMD ["/bin/bash"]
