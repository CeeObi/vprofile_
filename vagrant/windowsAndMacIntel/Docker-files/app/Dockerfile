#See container first as a development-directory where you should have all the necessary dependencies installed
# MULTISTAGE DOCKERFILE

# Stage 1 - Build the artifact
FROM maven:3.9.9-eclipse-temurin-21-jammy AS ARTIFACT_BUILD_IMAGE
RUN git clone https://github.com/hkhcoder/vprofile-project.git
RUN cd vprofile-project && git checkout containers && mvn install


# Stage 2 - Use the artifact from above to build the customised tomcat-container-image itself.
            #Customized in the sense that we modified its default web directory and replaced it with our artifact as seen below:
FROM tomcat:10-jdk21
LABEL "Project"="Vprofile"
LABEL "Author"="Dima"
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=ARTIFACT_BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
