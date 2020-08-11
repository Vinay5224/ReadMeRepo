# SaaS Provisioning API

[![Sotero](https://bitbucket.org/soterosoft/provisioning-api/downloads/favicon.png)]()
Java API used to build CloudFormation Stack and deploy tenant services via Helm in EKS. Tenant architecture and additional details are available on [Confluence](https://soterosoft.atlassian.net/wiki/spaces/SOT) under *Tenant Design* and *Provisioning API*

## Features

- provision a tenant ✔
- deploy Migrator and SqlProxy services to Kubernetes ✔
- fetch deployment status ✔
- upgrade services ✔
- scale tenant ✔
- Vault Namespace integration ✔
- tenant node group separation ✘
- deploy Tenant API as a separate service ✘
- rollback service ✘

## Requirements

For building and running the application you need:

- [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Maven 3](https://maven.apache.org)
- [Docker](https://docker.com)
- [Helm v2](https://v2.helm.sh/)

## Application configuration

### Set up metastore

>IMPORTANT: You need to have CloudFormation stack deployed for tenant, and a VPC Stack Name

- install MySql locally to your workstation
- create `sotero_metastore` database
- create schema by running `src/main/java/resources/metastore/01_create_schema.sql`
- populate the schema with `src/main/java/resources/metastore/02_init_schema.sql` by substituting values in `vpc` table with your own stack values (physical and logical id)

### Configuration

Configuration values are stored in `application.yaml` under `src/main/java/resources` folder. Each configuration value will be loaded as environment variable via helm `values.yaml` when deployed.

For local development, create your own configuration based on `application.yaml.properties` and use values that reflect your local/remote environment.

## Run the application locally

You can use the [Spring Boot Maven plugin](https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html) like so:

shell
mvn spring-boot:run


## OpenAPI specification

Application will run on port 8080, where OpenAPI Swagger will be shown, documenting endpoints, including examle GET/POST requests.

## Build and push

* You can run `docker_build_and_push.sh`, which basically does the same as the steps outlined below.

* obtain values for application-{env}.yaml
* build app with 
shell
mvn clean package -DskipTests=true

* build image with:
shell
docker build --build-arg VERSION="${VERSION_TAG}" -t soterosoft/provisioning-api:"${VERSION_TAG}" .

* push image with:
shell
docker push soterosoft/provisioning-api:"${VERSION_TAG}"


## Deploying the app to Kubernetes

Please follow instructions under `helm/README.md`

## Contribution
- README.md is updated
- A successful build has been deployed on Docker Hub
- All newly implemented APIs have been documented
- All newly added properties are documented in SoteroConfig.java

### Coding Style

We use [Java Google Style](https://google.github.io/styleguide/javaguide.html) along with [CheckStyle](https://github.com/checkstyle/checkstyle)

For IntelliJ, you can use one of the following plugins:

- [Google Java Format](https://github.com/google/google-java-format)
- [IntelliJ Java Google Style](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)

### Commit message

- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)

## Copyright
