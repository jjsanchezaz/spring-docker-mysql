# spring-docker-mysql
Complete Spring Boot multi-module example integrated with gradle, docker, docker-compose, mysql and swaggerUI.

## Requirements
Docker is required if you want to make use of the `buildDockerImage` gradle task and `docker-compose.yml` file. 

## Build
To build the jars for each module:
```bash
./gradlew build 
```

To build the docker image:
```bash
./gradlew buildDockerImage
```
Check with `docker images` that the image was created and tagged as `spring-docker-mysql`:
```bash
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
spring-docker-mysql       latest              9442eae8fbcd        26 seconds ago      390MB
```

## Run
```bash
docker-compose up
```
It creates the `dbmysql` and `spring-docker-mysql` containers in the same network.
```bash
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                                            NAMES
91a222fee55a        spring-docker-mysql     "java -Xdebug -Xrunj…"   6 minutes ago       Up 6 minutes        0.0.0.0:7080->7080/tcp, 0.0.0.0:8080->8080/tcp   spring-docker-mysql
39df3ad6e3ce        mysql                   "docker-entrypoint.s…"   4 weeks ago         Up 6 minutes        0.0.0.0:8079->3306/tcp                           dbmysql
```

## Modules
The project is composed by 2 modules. The persistence module contains the model and persistence classes and the service module contains the controller, application and swagger configuration classes.
```bash
spring-docker-mysql
├── persistence
│   └── src
│       └── main
│           └── java
│               └── com.example.springdocker.persistence
│                   ├── PokemonDTO.java
│                   ├── PokemonModel.java
│                   └── PokemonRepository.java
└── service
    └── src
        └── main
            └── java
                └── com.example.springdocker.service
                    ├── Application.java
                    ├── PokemonController.java
                    └── SwaggerConfig.java

```

## SwaggerUI
Default swaggerUI: http://localhost:8080/spring-docker-mysql/swagger-ui.html

PokemonController example: http://localhost:8080/spring-docker-mysql/swagger-ui.html#/pokemon-controller

## Using remote debugger (IntelliJ IDEA)
- Go to **Run/Debug Configurations**
- Create **Remote JVM Debug**
- Configure **host and port** (**localhost** and **7080**) and save the configuration
- Start application, select the configuration and click **Debug**

This will attach your debugger to the running application. 

In case you need to start the debugger since the beginning you must change this line in the **Dockerfile**:
```bash
ENTRYPOINT ["java", "-Xdebug" , "-Xrunjdwp:transport=dt_socket,address=*:7080,server=y,suspend=n", "-jar", "app.jar"]
```
to
```bash
ENTRYPOINT ["java", "-Xdebug" , "-Xrunjdwp:transport=dt_socket,address=*:7080,server=y,suspend=y", "-jar", "app.jar"]
```
This will stop the application from starting until a debugger has been attached

## Examples
### CREATE
```bash
curl --location --request POST 'http://localhost:8080/spring-docker-mysql/pokemon/create' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Mewtwo",
    "type": "Psychic"
}'
```
### GET
```bash
curl --location --request GET 'http://localhost:8080/spring-docker-mysql/pokemon/random'
```
```bash
curl --location --request GET 'http://localhost:8080/spring-docker-mysql/pokemon/1'
```
```bash
curl --location --request GET 'http://localhost:8080/spring-docker-mysql/pokemon/all'
```
### DELETE
```bash
curl --location --request DELETE 'http://localhost:8080/spring-docker-mysql/pokemon/1'
```

