FROM wspark83/springboot:openjdk8

ARG JAR_FILE=target/springboot-0.0.1-SNAPSHOT.jar

COPY ${JAR_FILE} app.jar

ENTRYPOINT ["java","-jar","/app.jar"]
