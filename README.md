# Why & When

![Build Workflow](https://github.com/mirkosertic/flight-recorder-starter/workflows/Build%20Workflow/badge.svg)

This is a Spring Boot 2 Starting exposing the Java Flight Recorder as a Spring Boot Actuator Endpoint.

Normally the Java Flight Recorder is available locally or by JMX remote. Depending on your deployment 
scenario shell or JMX access might not be available for the application server. Here comes this handy
starter into play!

# How

This starter adds a new Spring Boot Actuator endpoint for Java Flight Recorder remote control. This 
RESTful endpoint allows starting and stopping Flight Recording and downloading the .jfr files
for further analysis.

## Starting Flight Recording

The following `cURL` command starts a new Flight Recording and returns the created Flight Recording ID:

```
curl  -i -X PUT -H "Content-Type: application/json" \ 
    -d '{"duration": "60","timeUnit":"SECONDS"}' \
    http://localhost:8080/actuator/flightrecorder

HTTP/1.1 200 
Content-Type: text/plain
Content-Length: 1
Date: Thu, 03 Sep 2020 11:24:53 GMT

1
```

Flight Recording starts for a given period, in this case 60 seconds and stops then.

Every recording session gets its own unique Flight Recording ID. The endpoint returns
this ID as plain text, in this case ID `1`. This ID must be used to download the 
recorded data.

## Downloading results

The following `cURL` command stops the Flight Recording with ID `1` and downloads the `.jfr` file:

```
curl --output recording.jfr http://localhost:8080/actuator/flightrecorder/2
```

## Stopping Flight Recording and discarding results

The following `cURL` command stops the Flight Recording with ID `1` and discards all data:

```
curl -X DELETE http://localhost:8080/actuator/flightrecorder/2
```