FROM openjdk:11-slim

COPY target/spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar /usr/src/petclinic/
WORKDIR /usr/src/petclinic

CMD ["java", "-jar", "-Dspring.profiles.active=mysql", "spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar", "--server.port=80", "--spring.datasource.url=jdbc:mysql://webinar-petclinic.mysql.database.azure.com/petclinic", "--spring.datasource.password=Petc1iniC" ]
