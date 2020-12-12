# Getting Started

## Windows
## prueba2

### Compile Code
* ./mvnw.cmd clean compile -e

### Test Code
* ./mvnw.cmd clean test -e

### Jar Code
* ./mvnw.cmd clean package -e

### Run Jar
* Local:      ./mvnw.cmd spring-boot:run 
* Background: nohup bash mvnw.cmd spring-boot:run &

### Testing Application
* Abrir navegador: http://localhost:8081/rest/mscovid/test?msg=testing


## Linux

### Compile Code / Test Code / Jar Code
*  gradle build


##Test Code
gradle test

### Run Jar
* Local:   gradle bootRun
* Background: nohup bash gradle bootRun &

### Testing Application
* curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'
