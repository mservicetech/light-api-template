# Run applications
```shell
java -jar -Dlight-4j-config=configuration/local/config target/light-petstore-api-1.00.jar

java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5001 -jar -Dlight-4j-config=configuration/local/config target/light-petstore-api-1.00.jar

# up to java 5
java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5001 -jar -Dlight-4j-config=configuration/local/config target/light-petstore-api-1.00.jar
```
