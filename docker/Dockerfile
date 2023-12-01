FROM ghcr.io/graalvm/graalvm-ce:21.3.3 as java

WORKDIR /opt

USER root

COPY expertiza-1.0-SNAPSHOT.jar .

COPY license.xml .

EXPOSE 80

ENTRYPOINT ["java","-jar","-Dspring.profiles.active=production","expertiza-1.0-SNAPSHOT.jar"]