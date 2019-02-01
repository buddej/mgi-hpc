FROM phusion/baseimage:0.11

LABEL maintainer="John Budde <budde@wustl.edu>"

LABEL org.label-schema.schema-version="1.0"
# LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name="buddej/trickle"
LABEL org.label-schema.description="Image for trickle per mcallaway"

RUN apt-get update -y \
  && apt-get install --no-install-recommends -y \
       libnss-sss \
       time \
       tzdata \
       ssh \
       rsync \
       wget \
       curl \
       trickle \
       lftp \
       bsdmainutils \
       jq \
  && apt-get clean all \
  && rm -rf /var/lib/apt/lists/*

ENV TZ "America/Chicago"

CMD ["/bin/bash"]
