# Build in gradle

## Preconditions
- Install JDK 8 (from oracle or open-jdk)
- cd {light api template folder}

## Generate code based on specification
```shell
# **nix or MacOs
./gradlew codeGen

# windows
gradlew codeGen
```

## Build application
```shell
# **nix or MacOs
./gradlew clean build

# windows
gradlew clean build
```

## Run applications
```shell
java -jar -Dlight-4j-config-dir=configuration/local/config target/light-petstore-api-1.00.jar

java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5001 -jar -Dlight-4j-config-dir=configuration/local/config target/light-petstore-api-1.00.jar

# up to java 5
java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5001 -jar -Dlight-4j-config-dir=configuration/local/config target/light-petstore-api-1.00.jar
```
