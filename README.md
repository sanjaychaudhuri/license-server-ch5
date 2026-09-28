## Spring Microservices in Action - Second Edition. Chapter 5

# Introduction
Welcome to Spring Microservices in Action, Chapter 5.  Chapter 5 introduces the Spring Cloud Config service and how you can use it managed the configuration of your microservices.  By the time you are done reading this chapter you will have built and/or deployed:

1.  A Spring Cloud Config server that is deployed as Docker container and can manage a services configuration information using a file system/ classpath or GitHub-based repository.
2.  A licensing service that will manage licensing data used within Ostock.
3.  A Postgres SQL database used to hold the data.

## Initial Configuration
1.	Apache Maven (http://maven.apache.org)  All of the code examples in this book have been compiled with Java version 11.
2.	Git Client (http://git-scm.com)
3.  Docker(https://www.docker.com/products/docker-desktop)

## How To Use

To clone and run this application, you'll need [Git](https://git-scm.com), [Maven](https://maven.apache.org/), [Java 11](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html). From your command line:

```bash
# Clone this repository
$ git clone https://github.com/ihuaylupo/manning-smia

# Go into the repository, by chaning to the directory where you have downloaded the 
# chapter 5 source code
$ cd chapter5

# Build the jars and the container images (works with Docker or Podman on Intel and Apple Silicon)
$ mvn clean package

# To build the jars only, without container images:
$ mvn clean package -Ddocker.skip=true

# Start the stack. If host port 8080 is already in use, prefix with LICENSING_PORT=8081
# The config server listens on 8089: docker-compose.yml sets SERVER_PORT=8089, which
# overrides server.port in bootstrap.yml, and points the licensing service at it via
# SPRING_CLOUD_CONFIG_URI. Running the config server outside compose still uses 8071.
$ docker compose -f docker/docker-compose.yml up
```

# The build command

Runs `docker build` for each service through the exec-maven-plugin defined in each module's pom.xml (the original Spotify dockerfile plugin is unmaintained and does not work on Apple Silicon). The Dockerfiles use `eclipse-temurin:11` images because the `openjdk:11-slim` tag has been removed from Docker Hub.

This is the first chapter we will have multiple Spring projects that need to be be built and compiled.  Running the above command at the root of the project directory will build all of the projects.  If everything builds successfully you should see a message indicating that the build was successful.

# The Run command

This command will run our services using the docker-compose.yml file located in the /docker directory. 

If everything starts correctly you should see a bunch of Spring Boot information fly by on standard out.  At this point all of the services needed for the chapter code examples will be running.

# Profiles
The config server holds `default`, `dev`, `prod` and `test` property files under `configserver/src/main/resources/config`.
`docker/docker-compose.yml` selects the profile through `SPRING_PROFILES_ACTIVE` (defaults to `test`) and starts Postgres with the
matching password: the `test` profile uses password `pass`, stored encrypted in `licensing-service-test.properties`.

```bash
# default: test profile, Postgres password 'pass'
$ docker compose -f docker/docker-compose.yml up

# dev profile, Postgres password 'postgres'
$ PROFILE=dev POSTGRES_PASSWORD=postgres docker compose -f docker/docker-compose.yml up
```

# Postman
Import `ch5-licensing-service.postman_collection.json` to exercise the config server and the licensing service. Run the Licensing Service folder in order; the Create request stores the generated license id for the later requests.

# Database
You can find the database script as well in the docker directory.

## Contact

I'd like you to send me an email on <illaryhs@gmail.com> about anything you'd want to say about this software.

### Contributing
Feel free to file an issue if it doesn't work for your code sample. Thanks.