FROM cloudfoundry/cflinuxfs2

MAINTAINER Rik de Groot <hwdegroot@gmail.com>

ENV JAVA_VERSION="8" \
    JAVA_UPDATE="112" \
    JAVA_BUILD="15" \
    JAVA_HOME="/var/lib/jvm/oracle-java" \
    PATH="$JAVA_HOME/bin:$PATH"

RUN mkdir -p /var/lib/jvm $JAVA_HOME && \
    apt-get update -qq && \
    apt-get install -yqq ca-certificates-java && \
    apt-get update -qq && \
    apt-get -qqy install \
      curl \
      unzip && \
\
    curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/jdk-${JAVA_VERSION}u${JAVA_UPDATE}.tar.gz \
        "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/jce_policy-${JAVA_VERSION}.zip \
        "http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION}/jce_policy-${JAVA_VERSION}.zip" && \
    tar zxf /tmp/jdk-${JAVA_VERSION}u${JAVA_UPDATE}.tar.gz --strip-components=1 -C $JAVA_HOME && \
    unzip /tmp/jce_policy-${JAVA_VERSION}.zip -d /tmp && \
    mv /tmp/UnlimitedJCEPolicyJDK${JAVA_VERSION}/*.jar $JAVA_HOME/jre/lib/security && \
\
    update-ca-certificates -f && \
    echo Europe/Amsterdam > /etc/timezone && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
\
    update-alternatives --install /usr/bin/keytool keytool $JAVA_HOME/bin/keytool 100 && \
    update-alternatives --install /usr/bin/java java $JAVA_HOME/bin/java 100 && \
    update-alternatives --install /usr/bin/javac javac $JAVA_HOME/bin/javac 100 && \
\
    apt-get remove -qqy --auto-remove \
      curl \
      unzip \
      openjdk-7-jre-headless && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/*

