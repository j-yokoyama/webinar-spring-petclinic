FROM openjdk:11-slim

COPY *.jar /usr/src/petclinic/
WORKDIR /usr/src/petclinic

CMD ["java", "-jar", "spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar", "--server.port=80" ]
