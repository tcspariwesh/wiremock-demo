# Mocking stateful behavior

This is a simple example of how to use the WireMock tool (wiremock.org) as a standalone mock server. It demos the following features of WireMock:

* using JSON files to configure the mock server
* use of regex in pattern-matching endpoints
* delaying the response
* GETs and PUTs
* mocking stateful behavior (which makes sense when doing PUTs) <-- NEAT!
* using `curl` to verify that we have successfully mocked the endpoints

## How to install this demo

1. If you don't already have it, install the Java Runtime Environment (JRE).
1. Download the wiremock jar file. Link can be found here: http://wiremock.org/docs/running-standalone/
1. Then, if, for example, the jar is called ~/Downloads/wiremock-standalone-2.6.0.jar, do this:

```
$ clone https://github.com/wbraynen/wiremock.git # clone this demo
$ cd wiremock # go inside the wiremock directory you just cloned
$ mv ~/Downloads/wiremock-standalone-2.6.0.jar . # move the wiremock jar from Downloads into current directory
$ mv wiremock-standalone-2.6.0.jar wiremock.jar # rename wiremock jar to simply "wiremock.jar", so the "wiremock" launch shell script is version independent
$ chmod 744 wiremock.sh # make the shell script executable
$ ./wiremock.sh # launch the mock server; the shell script will run the wiremock.jar for you
```

## How to use this demo

### 1. first thing to try

GET /minions/blah
```
$ curl http://localhost:8080/minions/blah
```
"blah" is a minion id. Try any other id and it will work too :) Notice the delay?! If you don't, look inside the `1_GET_minions_anyResource_withDelay.json` file in the `mappings` folder and increase the delay from two to three seconds by changing the value of `fixedDelayMilliseconds` from 2000 to 3000.

### 2. second thing to try

GET /minions/cd3e
```
$ curl http://localhost:8080/minions/cd3e
```
"cd3e" Different mock json, so no delay this time!

### 3. third thing to try

PUT /minions/cd3e three times using the mock server as a state machine:
```
$ curl -X PUT http://localhost:8080/minions/cd3e
$ curl -X PUT http://localhost:8080/minions/cd3e
$ curl -X PUT http://localhost:8080/minions/cd3e
```
Each time you curl this same exact endpoint, the wiremock server will serve you a different json file.

To shut down the mock server, use ./wiremock -k

## What files got served

The JSON files that the wiremock server serves from the mappings directory when you follow the steps above:

1. GET /minions/blah: `1_GET_minions_anyResource_withDelay.json`
1. GET /minions/cd3e: `2_GET_minions_cd3e.json`
1. PUT /minions/cd3e: `3a_PUT_minions_cd3e.json` the first time, `3b_PUT_minions_cd3e.json` the second time, `3c_PUT_minions_cd3e.json` the third time.

## FAQ

Q: Is it OK to use WireMock in an industry setting for free?

A: Yes, last time I checked, WireMock uses the [Apache License](https://github.com/tomakehurst/wiremock/blob/master/LICENSE.txt).
___

Q: Are there other tools aside from WireMock with which to mock web services and REST APIs?

A: Yes, lots.  For example: apimocker.js.  Swagger also has a mock server.
___
