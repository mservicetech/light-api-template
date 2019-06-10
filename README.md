# Light-api-template

In light-4j document site, it has tutorial introduce how to generate service API from openapi specification from command line:

- [Generate Light-4j API from command line](https://www.networknt.com/references/light-codegen/openapi-generator/)


But sometimes, developer likes to initial the project from IDE directly. And in the development phase, the specification may change very often. It could be
difficult to regenerate project from new openapi specification and merge the existing implemented code.




##  Project folder structure.

-- configuration     config folder for deployment. The sub-folder will created for different environment (Local/DEV/SIT/UAT)

-- specification    specification folder include openapi specification and config file for code-generation.




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
