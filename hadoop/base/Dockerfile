FROM eclipse-temurin:8-jdk-focal

MAINTAINER Dejan Djekic <dejan@functorns.com>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    openjdk-8-jdk \
    net-tools \
    curl \
    netcat \
    gnupg \
    libsnappy-dev \ 
    && rm -rf /var/lib/apt/lists/*

RUN curl -O https://dist.apache.org/repos/dist/release/hadoop/common/KEYS

RUN gpg --import KEYS

ENV HADOOP_VERSION 3.3.6
ENV HADOOP_URL https://www.apache.org/dist/hadoop/common/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION

RUN set -x \
    && curl -fSL "$HADOOP_URL$([ $(uname -m) = "aarch64" ] && echo "-aarch64" || echo "" ).tar.gz" -o /tmp/hadoop.tar.gz \
    && curl -fSL "$HADOOP_URL$([ $(uname -m) = "aarch64" ] && echo "-aarch64" || echo "" ).tar.gz.asc" -o /tmp/hadoop.tar.gz.asc \
    && gpg --verify /tmp/hadoop.tar.gz.asc \
    && tar -xvf /tmp/hadoop.tar.gz -C /opt/ \
    && rm /tmp/hadoop.tar.gz*

RUN ln -s /opt/hadoop-$HADOOP_VERSION/etc/hadoop /etc/hadoop

RUN mkdir /opt/hadoop-$HADOOP_VERSION/logs \
    && mkdir /hadoop-data

ENV HADOOP_HOME=/opt/hadoop-$HADOOP_VERSION
ENV HADOOP_CONF_DIR=/etc/hadoop
ENV MULTIHOMED_NETWORK=1
ENV USER=root
ENV PATH $HADOOP_HOME/bin/:$PATH

ADD entrypoint.sh /entrypoint.sh

RUN chmod a+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
