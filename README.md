# Light-api-template

In light-4j document site, it has tutorial introduce how to generate service API from openapi specification from command line:

- [Generate Light-4j API from command line](https://www.networknt.com/tool/light-codegen/openapi-generator/)

From the example (petstore) detail for generate project from command line, please refer to:

- [light-4j model-config petstore sample](https://github.com/networknt/model-config/tree/master/rest/openapi/petstore/1.0.0)


In most of the cases, developer likes to initial the project from IDE directly. And in the development phase, the specification may change very often. It could be
difficult to regenerate project from new openapi specification and merge the existing implemented code.


### Note

By using the default POM, it is for JDK11. If user use JD8, please use the pom_jdk8.xml (change the file name to pom.xml)


##  Project folder structure.

-- configuration     config folder for deployment. The sub-folder will created for different environment (Local/DEV/SIT/UAT)


normally the config folder will include values.ym and keystore and truststore files


-- specification    specification folder include openapi specification and config file for code-generation.


##  Start to develop

Note: the repo we are using is for maven build. If you want to use gradle， please use [gradle api template](https://github.com/mservicetech/light-api-template-gradle).

After the service project set on your IDE, you can run maven build from IDE or from command line:

 -- mvn clean install

The maven plugin below will run light-codegen and generate API project for you:


  ```xml

            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.4.0</version>
                <executions>
                    <execution>
                        <id>light4j-codegen</id>
                        <phase>process-resources</phase>
                        <goals>
                            <goal>exec</goal>
                        </goals>
                        <inherited>false</inherited>
                        <configuration>
                            <executable>java</executable>
                            <arguments>
                                <argument>-jar</argument>
                                <argument>${settings.localRepository}/com/networknt/codegen-cli/${version.light-4j}/codegen-cli-${version.light-4j}.jar</argument>
                                <argument>-f</argument>
                                <argument>openapi</argument>
                                <argument>-o</argument>
                                <argument>./</argument>
                                <argument>-m</argument>
                                <argument>specification/openapi.yaml</argument>
                                <argument>-c</argument>
                                <argument>specification/config.json</argument>
                            </arguments>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

  ```

  - [Maven Pom](https://github.com/mservicetech/light-api-template/blob/master/pom.xml)



The following is config setting for codegen:

### config file:


 ```

 {
   "name": "light-petstore",
   "version": "1.00",
   "groupId": "com.networknt.api",
   "artifactId": "light-petstore-api",
   "rootPackage": "com.networknt.petstore",
   "handlerPackage":"com.networknt.petstore.handler",
   "modelPackage":"com.networknt.petstore.model",
   "overwriteHandler": false,
   "overwriteHandlerTest": false,
   "overwriteModel": false,
   "httpPort": 8080,
   "enableHttp": false,
   "httpsPort": 8443,
   "enableHttps": true,
   "enableRegistry": false,
   "specChangeCodeReGenOnly": false,
   "generateValuesYml": true,
   "regeneratePomFile": false,
   "supportDb": false,
   "generateValuesYml": true,
   "generateEnvVars": {
     "generate": true,
     "skipArray": true,
     "skipMap": true,
     "exclude": [
       "handerl.yml",
       "values.yml"
     ]
   }
 }


 ```

For detail info about the config file, please refer to: [light-codegen config](https://www.networknt.com/references/light-codegen/openapi-kotlin-generator/)



At the first time to run "mvn clean install " to build project, the specChangeCodeReGenOnly should be set as 'false' which will generate the whole project:

 ```
   "specChangeCodeReGenOnly": false,
 ```

After that, we can change the setting to 'true'. Then codegen will only generate code based on the specification change.


 ```
   "specChangeCodeReGenOnly": true,
 ```

Light-4j codegen has option to generate API project to multiple-modules (default is single module) which include following modules:

- client   (client module for upstream API to call current API)

- service   (service module for business service classes)

- model    (data model)
 
- server   (API server)

If user wants to generate the API as multiple modules, simply add the config below to /sepification/config.json file

```json
"multipleModule": true 
```

## Build and verify

 ### Local environment from command line

 Build and start service:

 For local environment we use extended config folder: configuration/local/config;  The config file values.yml has set of values commonly overridden in microservices;
                                                                                           #


 ```
cd ~/workspace
git clone git@github.com:mservicetech/light-api-template.git

cd light-api-template

 mvn clean install

java -jar -Dlight-4j-config-dir=configuration/local/config  target/light-petstore-api-1.00.jar

 ```

Note: if it is first time to generate API project, please run "mvn clean install"  one more time to build the API



 Verify by sending request:

  ```
  https://localhost:8443/v1/pets

  ```

Result:

 ```

[
    {
        "id": 1,
        "name": "catten",
        "tag": "cat"
    },
    {
        "id": 2,
        "name": "doggy",
        "tag": "dog"
    }
]

 ```
