FROM ubuntu:14.04
MAINTAINER Yi Jin "http://jinyi.me" <y.jin@me.com>

RUN apt-get update \
  && apt-get install -y curl net-tools build-essential git wget unzip python python-setuptools python-dev python-numpy libfontconfig \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# JAVA
ENV JAVA_HOME /usr/jdk1.8.0_31
ENV PATH $PATH:$JAVA_HOME/bin
RUN curl -sL --retry 3 --insecure \
  --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
  "http://download.oracle.com/otn-pub/java/jdk/8u31-b13/server-jre-8u31-linux-x64.tar.gz" \
  | gunzip \
  | tar x -C /usr/ \
  && ln -s $JAVA_HOME /usr/java \
  && rm -rf $JAVA_HOME/man
RUN easy_install py4j

# MAVEN
ENV MAVEN_VERSION 3.3.1
ENV MAVEN_HOME /usr/apache-maven-$MAVEN_VERSION
ENV PATH $PATH:$MAVEN_HOME/bin
RUN curl -sL http://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
  | gunzip \
  | tar x -C /usr/ \
  && ln -s $MAVEN_HOME /usr/maven

# ZEPPELIN
ENV ZEPPELIN_HOME         /zeppelin
ENV ZEPPELIN_CONF_DIR     $ZEPPELIN_HOME/conf
ENV ZEPPELIN_NOTEBOOK_DIR $ZEPPELIN_HOME/notebook
ENV ZEPPELIN_PORT         8080

RUN git clone https://github.com/apache/incubator-zeppelin.git $ZEPPELIN_HOME

# INSTALL & CLEAN
WORKDIR $ZEPPELIN_HOME
COPY ./install.sh $ZEPPELIN_HOME/install.sh

RUN chmod +x ./install.sh \
  && sync \
  && ./install.sh \
  && rm -rf /root/.m2 \
  && rm -rf /root/.npm
EXPOSE 8080 8081 8888
COPY ./nifi-jar/* /nifi-jar/
COPY ./spark-env.sh /usr/spark/conf
RUN chmod +x /usr/spark/conf/spark-env.sh
CMD "bin/zeppelin.sh"