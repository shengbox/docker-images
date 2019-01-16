FROM frolvlad/alpine-oraclejdk8:slim

RUN mkdir -p /deployments

# JAVA_APP_DIR is used by run-java.sh for finding the binaries
ENV JAVA_APP_DIR=/deployments \
JAVA_MAJOR_VERSION=8

RUN echo -e "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main\n\
https://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/community" > /etc/apk/repositories

RUN apk --update add bash ttf-dejavu && \
      rm -rf /var/cache/apk/*

RUN mkdir -p /usr/share/fonts/chinese
COPY hei.ttf /usr/share/fonts/chinese/
COPY simsun.ttc /usr/share/fonts/chinese/

RUN chmod 755 /usr/share/fonts/chinese/hei.ttf \
&& chmod 755 /usr/share/fonts/chinese/simsun.ttc

# Agent bond including Jolokia and jmx_exporter
ADD agent-bond-opts /opt/run-java-options
RUN mkdir -p /opt/agent-bond
ADD agent-bond.jar /opt/agent-bond/agent-bond.jar
RUN chmod 444 /opt/agent-bond/agent-bond.jar \
&& chmod 755 /opt/run-java-options
ADD jmx_exporter_config.yml /opt/agent-bond/
EXPOSE 8778 9779

# Add run script as /deployments/run-java.sh and make it executable
COPY run-java.sh /deployments/
RUN chmod 755 /deployments/run-java.sh

CMD [ "/deployments/run-java.sh" ]