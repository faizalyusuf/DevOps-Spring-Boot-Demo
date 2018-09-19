FROM openjdk:8u151-jdk-alpine3.7
ARG name
ARG version

EXPOSE 8090

ENV APP_HOME /usr/src/app

COPY ${name}/${version}/*.jar $APP_HOME/app.jar

WORKDIR $APP_HOME

ENTRYPOINT exec java -jar app.jar

