FROM chatopera/java:1.0.0
MAINTAINER Hai Liang Wang <hain@chatopera.com>

# Set timezone
RUN apt-get update && \
   apt-get install --no-install-recommends -y tzdata && \ 
   ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime && \
   dpkg-reconfigure -f noninteractive tzdata && \
   rm -rf /var/lib/apt/lists/*

# Set the locale
ENV LANG C.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL C.UTF-8


# set ENVs
ENV JAVA_HOME=/opt/java-8-oracle
ENV MAVEN_HOME=/opt/apache-maven
ENV TOMCAT_HOME=/opt/apache-tomcat
ENV PATH=$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin

# create dirs
RUN /bin/bash -c "mkdir -p /{data,logs}"

# build WAR
COPY _m2/settings.xml /root/.m2/settings.xml
COPY app /app
WORKDIR /app
RUN mvn clean package && \
    rm -rf /opt/apache-tomcat/webapps/ROOT && \
    cp target/contact-center-3.9.0.war /opt/apache-tomcat/webapps/ROOT.war && \
    rm -rf /app && \
    rm -rf /root/.m2

EXPOSE 8035 8036 8080 9200 9300
CMD ["/opt/apache-tomcat/bin/catalina.sh", "run"]
