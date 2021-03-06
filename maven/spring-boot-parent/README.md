spring-boot-parent
=============

Parent which allows the usage of Spring Boot in combination with the [pro-vision global-parent](https://github.com/pro-vision/pv-build-tools/tree/develop/maven/global-parent).

Defines fixed versions of Spring Boot dependencies based on the following naming schema:

`<version>-<spring-boot-version>` e.g. `1-2.1.8` is the first release for Spring Boot 2.1.8. 

For major version separate folders are create in this repository.

## Docker

The spring-boot-parent parent provides a ready to use configuration if you want to build Docker images from your Spring Boot Project.

First you need to configure the `docker.image.repository` and `docker.image.prefix` properties in your `pom.xml`:
```
<properties>
  <!-- docker settings -->
  <docker.image.repository>YOUR-DOCKER-REGISTRY</docker.image.repository>
  <docker.image.prefix>pro-vision</docker.image.prefix>
</properties>
```

Depending on the used version, one of the following maven plugins is used:

* [spotify:dockerfile-maven](https://github.com/spotify/dockerfile-maven)
* [com.google.cloud.tools:jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)

### 2.3.x and up

Since 2.3.x the `spring-boot-parent` uses to more advanced `jib-maven-plugin`. It does not require any Dockerfile but is configured in the `pom.xml`.

Add the following build plugin to your `pom.xml`: 
```
<build>
  <plugins>
    <plugin>
      <groupId>com.google.cloud.tools</groupId>
      <artifactId>jib-maven-plugin</artifactId>
      <configuration>
        <skip>false</skip>
      </configuration>
    </plugin>
  </plugins>
</build>
```

When you now run `mvn jib:build` you'll get the required image build. 

For further information see:

* [com.google.cloud.tools:jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin).

### Up to 2.2.x

Add the following build plugins to your `pom.xml`:
```
<build>
  <plugins>
    <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>dockerfile-maven-plugin</artifactId>
      <configuration>
        <skip>false</skip>
      </configuration>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-dependency-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

Afterwards you can provide a Dockerfile which needs to reference your Application's Main Class:
```
FROM adoptopenjdk/openjdk11-openj9:alpine-jre
VOLUME /tmp
ARG DEPENDENCY=target/dependency
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY ${DEPENDENCY}/META-INF /app/META-INF
COPY ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","org.springframework.samples.petclinic.PetClinicApplication"]
```

When you now run `mvn clean install dockerfile:build` you'll get the required image.

For further information see:

* [spotify:dockerfile-maven](https://github.com/spotify/dockerfile-maven)
* [dev-eth0.de: Dockerize Spring Boot Applications](https://www.dev-eth0.de/2019/07/29/dockerize-spring-boot-applications/)


[![Maven Central](https://maven-badges.herokuapp.com/maven-central/de.pro-vision.maven/de.pro-vision.maven.spring.spring-boot-parent/badge.svg)](https://maven-badges.herokuapp.com/maven-central/de.pro-vision.maven/de.pro-vision.maven.spring.spring-boot-parent)

