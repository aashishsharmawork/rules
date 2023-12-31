## Weather Advice DMN

Description

A DMN service to return weather advice.

Demonstrates DMN on Kogito, including REST interface code generation.

## Installing and Running

### Prerequisites

You will need:

- Java 11+ installed
- Environment variable JAVA_HOME set accordingly
- Maven 3.8.1+ installed

When using native image compilation, you will also need:

- [GraalVM 19.3.1](https://github.com/oracle/graal/releases/tag/vm-19.3.1) installed
- Environment variable GRAALVM_HOME set accordingly
- Note that GraalVM native image compilation typically requires other packages (glibc-devel, zlib-devel and gcc) to be installed too. You also need 'native-image' installed in GraalVM (using 'gu install native-image'). Please refer to [GraalVM installation documentation](https://www.graalvm.org/docs/reference-manual/aot-compilation/#prerequisites) for more details.

### Compile and Run in Local Dev Mode

```
mvn clean compile quarkus:dev
```

### Package and Run in JVM mode

```
mvn clean package
java -jar target/quarkus-app/quarkus-run.jar
```

or on Windows

```
mvn clean package
java -jar target\quarkus-app\quarkus-run.jar
```

### Package and Run using Local Native Image

Note that this requires GRAALVM_HOME to point to a valid GraalVM installation

```
mvn clean package -Pnative
```

To run the generated native executable, generated in `target/`, execute

```
./target/dmn-quarkus-example-runner
```

Note: This does not yet work on Windows, GraalVM and Quarkus should be rolling out support for Windows soon.

## OpenAPI (Swagger) documentation

[Specification at swagger.io](https://swagger.io/docs/specification/about/)

You can take a look at the [OpenAPI definition](http://localhost:8080/openapi?format=json) - automatically generated and included in this service - to determine all available operations exposed by this service. For easy readability you can visualize the OpenAPI definition file using a UI tool like for example available [Swagger UI](https://editor.swagger.io).

In addition, various clients to interact with this service can be easily generated using this OpenAPI definition.

When running in either Quarkus Development or Native mode, we also leverage the [Quarkus OpenAPI extension](https://quarkus.io/guides/openapi-swaggerui#use-swagger-ui-for-development) that exposes [Swagger UI](http://localhost:8080/swagger-ui/) that you can use to look at available REST endpoints and send test requests.

## Test DMN Model using Maven

Validate the functionality of DMN models before deploying them into a production environment by defining test scenarios in Test Scenario Editor.

To define test scenarios you need to create a .scesim file inside your project and link it to the DMN model you want to be tested. Run all Test Scenarios, executing:

```sh
mvn clean test
```

See results in surefire test report `target/surefire-reports`

## Example Usage

Once the service is up and running, you can use the following example to interact with the service.

### POST /Flight_recognition

Returns recognitions from the given inputs

Given inputs:

```json
{
  "Flight": {
    "flight time": "09:00",
    "flight date": "2023-08-28",
    "passengers": [
      {
        "seat": "22A",
        "name": "Tim",
        "status": "360",
        "million miler": true,
        "last received recognition": "2021-12-20",
        "missed connection": true
      },
      {
        "seat": "22B",
        "name": "Steve",
        "status": "Diamond",
        "million miler": false,
        "last received recognition": "2022-01-01",
        "missed connection": false
      }
    ],
    "distance": 200
  },
  "Passenger": "Test Passenger"
}
```

Curl command (using the JSON object above):

```sh
    curl -X 'POST' \
      'http://localhost:8080/flight_recognition' \
      -H 'accept: application/json' \
      -H 'Content-Type: application/json' \
      -d '{
      "Flight": {
        "flight time": "09:00",
        "flight date": "2023-08-28",
        "passengers": [
          {
            "seat": "22A",
            "name": "Tim",
            "status": "360",
            "million miler": true,
            "last received recognition": "2021-12-20",
            "missed connection": true
          },
          {
            "seat": "22B",
            "name": "Steve",
            "status": "Diamond",
            "million miler": false,
            "last received recognition": "2022-01-01",
            "missed connection": false
          }
        ],
        "distance": 200
      },
      "Passenger": "Test Passenger"
    }'
```

Example response:

```json
{}
```

## Deploying with Kogito Operator

In the [`operator`](operator) directory you'll find the custom resources needed to deploy this example on OpenShift with the [Kogito Operator](https://docs.jboss.org/kogito/release/latest/html_single/#chap_kogito-deploying-on-openshift).

# rules1
