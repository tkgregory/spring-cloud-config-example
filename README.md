## Running

1. start Spring Cloud Config service 
```
./gradlew -p configuration-service bootRun
```
1. start Spring Cloud Config client application
```
./gradlew -p configuration-client bootRun
```

## Retrieving property value from Spring Cloud Config client application

Make a GET request to [/message](http://localhost:8080/message)

