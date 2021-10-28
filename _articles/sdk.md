---
title: PLEX SDK Documentation
tags: [sdk, tuio, node]
---



<div class="uk-active" id="overview"></div>
<h2>1. Overview</h2>

The main purpose of this document is to guide for the features of the Plex-SDK. The main features wrapped for this SDK are sending data analytics for plex, sending heartbeat signals to health-check systems and working with applications with face recognition features such as Face Recognition and Estimators.

<h2>2. Requirements</h2><div id="requirements"></div>

```
Node v14.8.0
```

<h2>3. Abbreviations</h2><div id="abbreviations"></div>

The following abbreviations are used throughout this document. In the document, its main purpose is to show details how to work with Plex Platform and its main features through the Plex-SDK.

<strong>API</strong> Application Programming Interface<br><br>
<strong>SDK</strong> Software Development Kit

<h3>3.1 Syntax Conventions</h3>

All the multiline source codes are highlighted as the example below. If the code is inline, just the font is used, like this: <strong>helloWorld()</strong>. When the code is an outline, it is in a box. Reserved words are bold-formatted:


```
console.log(“Hello world!”) 


if (a == b) { /* ... */ }
```

<h3>3.2 Server Response Formats</h3>

| HTTP STATUS CODE | HTTP STATUS TEXT | DESCRIPTION |
|-------|--------|---------|
| 200 | OK | Request has achieved its objective, <br> response will be returned in body. |
| 400 | Bad Request | The request had bad syntax <br> or was inherently impossible to be satisfied. |
| 401 | Unauthorized | The request requires user authentication <br> or the authorization has been refused. |
| 403 | Forbidden | The user doesn’t have the <br> right to access the service. |
| 500 | Internal server error | The server encountered an unexpected <br> condition that prevented it from fulfilling the request. |


<h2>4. Getting Started</h2><div id="getting-started"></div>

Before starting to use the Plex-SDK you have to install it on your project, the easiest way to do it is by installing through Node Package Manager(NPM).
First, run the command below within the context of your project:

```
npm i @plexplatform/sdk
```

After executing the previous command, you are able to import the SDK into your project for executing it, just typing in the below command

```
const plex = require('@plexplatform/sdk’)
```

From now on, you are ready to start using Plex-SDK in your project.

<h2>5. General API</h2><div id="api"></div>

This section introduces the main methods of the Plex SDK and their functionality.

<h3>5.1 Health-check calls</h3>

The objective of this feature as its name suggests is to bring some kind of information that clients of the platform can understand as its entire park of devices are going on not just in the current moment but also in the past with our logging system. This functionality is enabled when you invoke method bellow. You can verify the health status of the devices at Plex.

```
plex.init()
```


<h3>5.2 sendMsg Method(deprecated)</h3>

The main objective of this method is to send a message containing an image data to Plex. The img parameter should be a base64-encoded image.

```
(async) sendMsg(img)
```

Send an image <strong>(img)</strong> to Plex.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| img | base64 | Image to be send to the platform |



As shown above, the method receives just one parameter, this parameter is attached as a payload of POST request.

<h3>5.3 init() Method</h3>

This method is responsible for starting health check service in software using SDK. It is just a wrapper around healthCheck().


```
init()
```


Call this method to start to use the Plex health-check system.

<h3>5.4 imgRecognition(img)</h3>

This method receives an img as parameter. Plex is going to use some face recognition engine to try to detect a person in img. Client needs to use some recognition phyapp  in Plex Journey to perform it.

```
(async) imgRecognition(img)
```


Send img to Plex to recognize face.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| img | base64 | The image to be send to the platform |

<h3>5.5 imgCharacteristics(img)</h3>

This method receives an image (img) as parameter. Plex is going to use some face estimator engine to detect person characteristics such as hair color, gender, age, etc.. Client needs to use some estimator phyapp in Plex journey.

```
(async) imgCharacteristics(img)
```

Send img to Plex to recognize face.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| img | base64 | the image to be send to the platform |


<h3>5.6 imgRegister(img, name)</h3>

This method receives two parameters and persists in the database for future use.
```
(async) imgRegister(img, name)
```

Send img to Plex to register face.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| img | base64 | The image to be send to the platform |
| name | string | The name to be send to the platform |

<h3>5.7 getOriginCode(uuid, callback)</h3>

This method is used to get an originCode  related to UUID if the same exists. 

```
getOriginCode(uuid, callback)
```


<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| uuid | string | This is the device unique identifier |
| callback | function | This is an function in order to receive return |


<h2>6. Authentication API</h2><div id="api-auth"></div>

The main objective of this section is to bring some light about authentication in the Plex.
Before starting to authenticate in Plex you need to contact the Plex team in order to get a CA, Client certificate and a valid ClientKEY.
As soon as you have this information in hands, you can go ahead with the authentication process.
The below is the only method necessary to authenticate in Plex.

<h3>6.1 Auth(rootCa, clientCert, clientKey)</h3>

This method receives three parameters and tries to authenticate in Plex.

```
auth(rootCA, clientCert, clientKey)
```


More  information about the parameters, please see below.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| rootCA | URI | The absolute path to RootCA certificate |
| clientCert | URI | The absolute path to Client certificate |
| clientKey | String | String with password key |

As you can see below. In the last three lines we are referencing the parameters of the .env file in order to keep the project better organized.

```
require(‘dotenv’).config()

sdk.auth(proccess.env.ROOT_CA,
                 proccess.env.CLIENT_CERT, 
                 proccess.env.CLIENT_KEY) 
```

<h2>7. Estimator API</h2><div id="api-estimatior"></div>

Several Plex features get started by receiving some details about personal characteristics or even a person's recognition for solving this kind of problems; we have added an Estimator API that helps construct these solutions.

<h3>7.1 verifyFaceEmotions(checkFaceEmotions)</h3>

This method checks face emotions in image. Please, check structure of this method, below:

```
verifyEmotions(checkFaceEmotions)
```

This method receives a boolean value to verify or to bypass these characteristics.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| checkFaceEmotions | boolean | true or false |

<h3>7.2 verifyFaceAttributes(checkFaceAttributes)</h3>

As named this method is responsible for configuring face attributes checking. This method receives a Boolean as parameter true to enable or false to bypass this checking.

```
verifyFaceAttributes(checkFaceAttributes)
```
This method receives a boolean value to verify or to bypass these characteristics.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| checkFaceAttributes | boolean | true or false |

<h3>7.3 buildEstimatorInstance (schema)</h3>

Method used to generate a schema or payload instance. This method receives a string to generate a schema or nothing to create an instance object.

```
buildEstimatorInstance(schema)
```

After calling <strong>verifyFaceAttributes</strong>, we need to call <strong>verifyFaceEmotions</strong> to generate a schema or payload instance. This method receives a string with a JSON Schema to generate a schema or not to create an instance object.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| schema | string | JSON Schema |

<h3>7.4 createSchema()</h3>

This is a simple method, it does not receive any parameter, just call this method in a fluent interface but we will speak about it in the next section.

```
createSchema()
```


After calling <strong>buildEstimatorInstance</strong>, we need to call this method to create the schema.

<h3>7.5 build()</h3>

This method should be invoked just if we need to create an instance object after calling buildEstimatorInstance() without parameters, to create an instance object.

```
build()
```

<h2>8. Fluent Interface and Estimator API and its case of using</h2><div id="api-use"></div>

This section explains how to use the Fluent Interface and Estimator API included in Plex SDK. The Estimator API was built using the Fluent Pattern, which consists in a chain of methods calling each other, basically. For a better understanding of this pattern, please refer to [Fluent Interface](https://java-design-patterns.com/patterns/fluentinterface).

<h3>8.1 Fluent Pattern in Estimator API</h3>

In this subsection, we explain as a estimator api makes use of the fluent interface pattern to turn the api more readable and developer-friendly.

As we can see in the below snippet of code, it sounds as if you are answering questions and in the final step you get what you want.

```
const schema = EstimatorBuilder
                    .verifyCheckFaceAttributes(true)
                    .verifyCheckFaceEmotions(true)
                    .buildEstimatorInstance()
                    .createSchema()
```

We have two different uses of estimator API in the snippet above. Starting in line 10 until the 14th line, we are creating a schema of this object.  Still in the snippet, from the 16 to 20th line we are creating an object instance. Anyway, the foundation is the same, if you have any doubts about methods and its use, check the sections about Estimator API, in the previous sections.

<h2>9. Analytics API</h2><div id="api-analytics"></div>

This section covers the Analytics API, since configuring some fields to send analytics data to plex.

<h3>9.1 sendAnalytics(callback)</h3>

This method is responsible for sending analytics data to Plex.

```
(async) sendAnalytics(callback)
```

This method is responsible for sending data to the Plex.

<h3>9.2 setAnalyticsClientAbandon(abandon)</h3>

This method is responsible to configure the abandon field in the data analytics. Abandon fields describe if experience was aborted in some moment.

```
setAnalyticsClientAbandon(abandon)
```

Set analytics abandon of a client.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| abandon | string | JSON Schema |

<h3>9.3 setAnalyticsClientCharacteristics(gender, age)</h3>

This method is responsible to configure some characteristics of a person in interaction (experience).

```
setAnalyticsClientCharacteristics(gender, age)
```
Set characteristics of a client. This method receives a string and an int as parameters.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| gender | string | male or female |
| age | int | an int number |

<h3>9.4 setAnalyticsEndTime()</h3>

This method does not receive any parameter. You need to call this method to configure end time of interaction.
```
setAnalyticsEndTime()
```
<h3>9.5 setAnalyticsFunelStep(step)</h3>

This method does not receive any parameter. You need to call this method to configure end time of interaction.

```
setAnalyticsFunelStep(step)
```

Set funnel step of client in experience.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| step | int | 1 - visitor |
| step | int | 2 - lead | 

<h3>9.6 setAnalyticsFunnelStepSales(step)</h3>

Set the funnel step of the client considering sales metrics. (1 = visitor; 2 = lead;  3 = won). Visitors are all clients who started an interaction with the experience. Leads are those ones who performed all the steps of the experience. Wons are all the leads who finished successfully the desired action, like a purchase, to contract a service, and others.

```
setAnalyticsFunelStepSales(step)
```

Set funnel step of a client’s sale process in experience.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| step | int | 1 - visitor |
| step | int | 2 - lead | 
| step | int | 3 - won | 


<h3>9.7 setAnalyticsClientName(name)</h3>

This method is responsible to configure the name field in object data analytics. It is used in some cases when you already have detected the person. Ex: (Face Recognition systems)

```
setAnalyticsClientName(name)
```

Set the name of a client.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| step | string | Client name |

<h3>9.8 setAnalyticsClientProfile(profile)</h3>

This method can be used to configure the profile field in the data analytics object. 

```
setAnalyticsClientProfile(profile)
```

Set the profile of a client.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| profile | string | The profile (e.g. moderado, arrojado) |

<h3>9.9 setAnalyticsClientSuccess(success)</h3>

This method can be used to configure the success field in the analytics object. What is considered success is a business rule that should be decided based on client wishes.
setAnalyticsClientSuccess(success)


Set the success status of a client.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| checkFaceEmotions | boolean | true or false |


<h3>9.10 setAnalyticsClientId(id)</h3>

This method is responsible for setting up the id field in the data analytics object.

```
setAnalyticsClientId(id)
```

Set the id of a client.

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| id | string | Unique identifier of a client. |

<h3>9.11 Analytics API  - Example of use</h3>

Here is an example of using an analytics API to send data to the Plex. In the below example we are using the several methods described in the previous topics.

```

sdk.setAnalyticsClientId(“456789910”)
sdk.setAnalyticsClientName(“João”)
sdk.setAnalyticsClientProfile(“moderado”)
sdk.setAnalyticsFunelStep(1)
sdk.setAnalyticsStartTime()
sdk.setAnalyticsEndTime()
sdk.setAnalyticsLeadTime()
sdk.setAnalyticsSaleTime()
sdk.setAnalyticsClientSuccess(true)
sdk.setAnalyticsClientAbandon(true)
sdk.setAnalyticsClientCharacteristics(“name”, 30)

sdk.sendAnalytics()
```

As you can see in the snippet above, after calling several methods, in the last line, we are calling the sendAnalytics() method to execute the final step.

<h2>10 Monitoring API</h2><div id="api-monitoring"></div>

<h3>10.1 updataMonitoringData(data, callback)</h3>

This method is used to update monitoring data in the Plex. You need to supply two parameters, see below for more details.

updateMonitoringData(data, callback)

<strong>Parameters:</strong>

| Name | Type | Description |
|-------|--------|---------|
| data | object | This is an object with four properties |
| callback | function | This is an function in order to receive return |

As you can see in the next snippet of code. We have constructed an object with the needed properties then invoked the updateMonitoringData method, passing the created object.

```
data = { 
 	    client: “COMPANY X”,    
 	    placeId: “5345456543f4545g656563s34545”,
 	    roomId: “6s7dfsdf6sdfsd66s6df8sd7fsdf87”,
        measurement: “599”
}

        Sdk.updateMonitoringData(data, (resp) => {
     	console.log(resp)
})

```
<h2>11. TUIO API</h2><div id="api-tuio"></div>

According to TUIO Official Website (tuio.org), “TUIO is an open framework that defines a common protocol and API for tangible multitouch surfaces. The TUIO protocol allows the transmission of an abstract description of interactive surfaces, including touch events and tangible object states. This protocol encodes control data from a tracker application (e.g. based on computer vision) and sends it to any client application that is capable of decoding the protocol. There exists a growing number of TUIO enabled tracker applications and TUIO client libraries for various programming environments, as well as applications that support the protocol”. This section explains the methods which can be used to detect TUIO messages through Plex SDK. 

<h3>11.1 Elements</h3>


There are two elements which can be detected: cursors, that represends selectors like a finger, for example, and objects, that represends surfaces with position, size and rotation.


| Cursor | Description |
|-|-|
| si | Section ID (everytime that a cursor is removed and added, the section ID changes) |
|   ci   | Cursor ID (unique identifier of the cursor) |
| xp | X-axis position |
| yp | Y-axis position |

<br>

| Object | Description |
|-|-|
| xp | X-axis position |
| yp | Y-axis position |
| si | Section ID |
| sym | Symbol ID (unique identifier of an object) |
| a | Angle of rotation |



<h3>11.2 tuio.init(host, srcPort, destPort)</h3>

This method lets you initialize a TUIO client. A client receives TUIO UDP signals through the port defined by srcPort and makes this available through a websocket server with host defined by host param and port defined by destPort. This server can be listened for any frontend application or through using the plex TUIO events methods.

```
tuio.init(host, srcPort, destPort)
```

Initialize a TUIO client.

<strong>Parameters:</strong>

| Name | Type | Description |
|-|-|-|
| host | string | Host of TUIO server |
| srcPort | int | Source port of TUIO sender |
| destPort | int | Destination port of TUIO server |

<h3>11.3 tuio.connect()</h3>

This method starts a TUIO client processing.

```
tuio.connect()
```
<h3>11.4 tuio.on(eventName, callbackFunction)</h3>

This method attaches a callback function to an eventName, according the table below:


| eventName | callbackFunction |
|-|-|
| addTuioCursor | function(cursor) |
| updateTuioCursor | function(cursor) |
| removeTuioCursor | function(cursor) |
| addTuioObject | function(object) |
| updateTuioObject | function(object) |
| removeTuioObject | function(object) |
| onRefresh | function(time) |
| updateDirection | function(object, directions) |
| collision | function(object, collisions) |



<h3>11.5 tuio.off(eventName, callbackFunction)</h3>

This method desattaches a callback function to an eventName.

```
tuio.off(eventName, callbackFunction)
```

<strong>Parameters:</strong>

| Name | Type | Description |
|-|-|-|
| eventName | string | name of the event |
| callbackFunction | function | Callback to desattache |

<h3>11.6 tuio.trigger(eventName)</h3>

This method forces an invocation of an event.

```
tuio.trigger(eventName)
```

<strong>Parameters:</strong>

| Name | Type | Description |
|-|-|-|
| eventName | string | name of the event |