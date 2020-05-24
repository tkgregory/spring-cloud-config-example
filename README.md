## Overview

This repository demonstrates the use of Spring Cloud Config to externalise application configuration into a Git repository.

![Overview diagram](https://app.lucidchart.com/publicSegments/view/4f06e315-4c7d-4f0b-acfa-0e2dfadc8a03/image.png)

## Running

* Create a Git repository in *~/config-repo*

```
mkdir ~/config-repo
cp properties/* ~/config-repo
cd ~/config-repo
git init
git add .
git commit -am "Commit properies"
```

* start Spring Cloud Config service 
```
./gradlew -p configuration-service bootRun
```

The service is now available on *localhost:8888*. You can access properties
using the API e.g. [/a-bootiful-client/default/master](http://localhost:8888/a-bootiful-client/default/master)

* start Spring Cloud Config client application
```
./gradlew -p configuration-client bootRun
```

The service is now available on *localhost:8080*.

## Retrieving property value from Spring Cloud Config client application

An API is exposed at [/message](http://localhost:8080/message) which returns the value of the `message` property
which is passed in using the `@Value` annotation.

You can now change properties or run the application with a different
profile to see how this effects the output of the API call.

## Properties

There are 3 properties files provided:

* *a-bootiful-client.properties* - this file always applies
* *a-bootiful-client-default.properties* - this file applies when the *default* profile is active
* *a-bootiful-client-uat.properties* - this file applies when the *uat* profile is active

### Running the Spring Cloud Config client application with the *UAT* profile

`./gradlew bootRun --args='--spring.profiles.active=dev'`

You should see that now the *a-bootiful-client-uat.properties* file is picked up.

### Updating properties

1. Make a change to the properties file in the Git repository
1. Commit the change
1. Make a POST request to the /refresh endpoint of the Spring Cloud Config Server
```
curl localhost:8080/actuator/refresh -d {} -H "Content-Type: application/json"
```