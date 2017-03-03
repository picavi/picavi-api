# Picavi API

## Abstract

The Picavi API is a standard interface for logistic warehouse's processes. The technical interface is based on JSON-RPC over HTTP. An implementation of this JSON-RPC Web Service enables a backend ERP-System or WMS to connect and communicate with Picavi Smart Glasses.

This document provides an overview of the Picavi Core API. Furthermore reference implementations with namespace and methods are given.

## Version/Change History

* Picavi API 1.0 - May, 2016

## Glossary and Acronyms

* API: Application Programming Interface that specifies how a software component interacts with another.
* ERP: Enterprise Resource Planning, a category of business-management software.
* HMD: Head-Mounted Display, a display device, worn on the head.
* HTTP: Hypertext Transfer Protocol, an application protocol for distributed, collaborative, hypermedia information systems, the foundation of data communication in the World Wide Web.
* JSON: JavaScript Object Notation is a lightweight data-interchange format.
* RPC: Remote Procedure Call. A computer program executes a method in another address space (commonly on another computer on a shared network), coded like a local method invocation, without the details of the remote interaction.
* WMS: Warehouse Management System, a software designed to support a Warehouse or Distribution Center.

## License
[![Creative Commons License](https://i.creativecommons.org/l/by-nd/4.0/88x31.png)](http://creativecommons.org/licenses/by-nd/4.0/)  
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](http://creativecommons.org/licenses/by-nd/4.0/).

## Copyright

Copyright © 2016-2017, Picavi GmbH  

## Quick Links
- 1 [Introduction][]
- 2 [Methods][]
  - 2.1 [System][MethodsSystem]
    - 2.1.1 [system.login][]
    - 2.1.2 [system.logout][]
  - 2.2 [Basic][MethodsBasic]
    - 2.2.1 [basic.requestJobData][]
    - 2.2.2 [basic.processJobData][]
    - 2.2.3 [basic.requestContextData][]
    - 2.2.4 [basic.processContextData][]
    - 2.2.5 [basic.requestData][]
    - 2.2.6 [basic.processData][]
  - 2.3 [Order Picking][MethodsOrderPicking]
    - 2.3.1 [orderPicking.getPickingList][]
    - 2.3.2 [orderPicking.submitPickingPosition][]
- 3 [Types][]
  - 3.1 [System][TypeSystem]
    - 3.1.1 [Credentials][]
    - 3.1.2 [Configuration][]
    - 3.1.3 [Client][]
  - 3.2 [Basic][TypeBasic]
    - 3.2.1 [Job][]
    - 3.2.2 [JobStep][]
    - 3.2.3 [Destination][]
- 4 [Info, Warning, Error][]

## 1 Introduction

To enable an ERP-System or WMS to connect and communicate with Picavi Smart Glasses, software developers need to implement a JSON-RPC over HTTP Web Service. The full specification of the JSON-RPC protocol is found at [JSON-RPC 2.0 Specification](http://www.jsonrpc.org/specification). The properties of the request object, parameter structures and the response object are described in the specification. Examples can be found in the [JSON-RPC Wikipedia article](https://en.wikipedia.org/wiki/JSON-RPC). Given in that article is also a list of reference implementations for JSON-RPC for different programming languages. Important: The Picavi API only uses POST HTTP requests.

For order picking, the web service shall be able to process the subsequent methods, and respond accordingly. Additional methods might be neccessary for extended business processes or new use cases. With regards to the pre-defined errors in the reserved range, the web service shall also return business errors within the range of reserved types defined by this API.

## 2 Methods
The Core API consists of the namespaces "system" and "basic" with their respective methods.
Additional namespaces are for customer specific use-cases, and examples like "orderPicking" serve as a reference.

To control the Client, every Response can have a property of Type [Client][].

### 2.1 System

#### 2.1.1 system.login
##### Arguments
  - credentials: ([Credentials][], `required`)

##### Return
  - sessionId: (`string`, `required`)
  - client: ([Client][])
  - configuration: ([Configuration][])

##### Example
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "system.login",
  "params": {
      "credentials": {
        "user": "9999",
        "password": "5f4dcc3b5aa765d61d8327deb882cf99",
        "station": "Kom12"
      }
  },
  "id": "8252"
}
```

###### Response
```json
{
  "jsonrpc": "2.0",
  "result": {
    "sessionId": "A64D586FE474",
    "configuration": {
      "language": "DE",
      "handedness": "right"
    }
  },
  "id": "8252"
}
```

#### 2.1.2 system.logout
This method is optional.

##### Arguments
  - sessionId: (`string`, `required`)

##### Example
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "system.logout",
  "params": {
    "sessionId": "A64D586FE474"
  },
  "id": "8252"
}
```
###### Response
```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": "8252"
}
```

### 2.2 Basic

#### 2.2.1 basic.requestJobData

##### Arguments

  - sessionId: (`string`, `required`)
  - request: (String, `required`)

##### Example (next)
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "basic.requestJobData",
  "params": {
    "sessionId": "A64D586FE474",
    "request": "next"
  },
  "id": "8252"
}
```
###### Response
```json
  {
    "jsonrpc": "2.0",
    "result": {
      "job": {
        "jobIdent": "447849",
        "jobSteps": [
          { "jobStepIdent": "1", "locationIdent": "KOM470004203", "locationName": "KOM-470-004-2-03", "itemIdent": "1-05010033", "itemName": "55 33", "destinationIdent": "257593", "destinationName": "UPSK84", "sourceIdent": "2049659", "sourceName": "", "targetQuantity": 2, "unit": "Stk", "info": "1 NP, 0 AM", "hasSerial": false, "status": "new"},
          {	"jobStepIdent": "2", "locationIdent": "KOM470007107", "locationName": "KOM-470-007-1-07", "itemIdent": "1-03280010", "itemName": "54 KK 10", "destinationIdent": "257593", "destination_name": "UPSK84", "sourceIdent": "2057517", "sourceName": "", "targetQuantity": 5, "unit": "Stk", "info": "1 NP, 0 AM", "hasSerial": false, "status": "new"},
          {	"jobStepIdent": "3", "locationIdent": "KOM800014101", "locationName": "KOM-800-014-1-01", "itemIdent": "1-02450008", "itemName": "49 A 1/8", "destinationIdent": "257594", "destination_name": "UPSKN9", "sourceIdent": "1993965", "sourceName": "", "targetQuantity": 1, "unit": "Stk", "info": "0 NP, 1 AM", "hasSerial": true, "status": "new"}
      	]
      }
    },
    "id": "8252"
  }
```
#### 2.2.2 basic.processJobData

##### Arguments

  - sessionId: (`string`, `required`)
  - request: (String, `required`)
  - jobStep([JobStep][])

##### Return

##### Example (submitStep)
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "basic.processJobData",
  "params": {
    "sessionId": "A64D586FE474",
    "request": "submitStep",
    "jobStep": {
      "jobStepIdent": "1",
      "status": "processed",
      "pickedQuantity": 5
    }
  },
  "id": "8252"
}
```
###### Response
```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": "8252"
}
```

#### 2.2.3 basic.requestContextData
**Context** means process specific data not related to any specific job.

##### Arguments
  - sessionId: (`string`, `required`)
  - request: (String, `required`)

##### Return
- request specific

##### Example (printerList)
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "basic.requestContextData",
  "params": {
      "sessionId": "A64D586FE474",
      "request": "printerList"
  },
  "id": "8252"
}
```
###### Response
```json
{
  "jsonrpc": "2.0",
  "result": {
    "deviceList": [
      {"deviceIdent": "1", "deviceName": "Kyocera 3051 ci", "deviceType": "printer" },
      {"deviceIdent": "2", "deviceName": "Brother MFC-8950DW", "deviceType": "printer" }
    ]
  },
  "id": "8252"
}
```

#### 2.2.4 basic.processContextData
##### Arguments

  - sessionId: (`string`, `required`)
  - request: (String, `required`)

##### Example
upcoming

#### 2.2.5 basic.requestData
**Data** means non process and non job specific data, with regards to **Job** and **Context**.

##### Arguments

  - sessionId: (`string`, `required`)
  - type: (String, `required`)
  - request: (String, `required`)

##### Example
upcoming

#### 2.2.6 basic.processData
##### Arguments

  - sessionId: (`string`, `required`)
  - type: (String, `required`)
  - request: (String, `required`)

##### Example
upcoming

### 2.3 Order Picking
This is an example for a use-case specific namespace with respective methods.

#### 2.3.1 orderPicking.getPickingList
##### Arguments

  - sessionId: (`string`, `required`)
  - pickingListIdent: (`string`)

##### Return

  - pickingList: (PickingList)

##### Example
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "orderPicking.getPickingList",
  "params": {
    "sessionId": "A64D586FE474",
    "pickingListIdent": "16-4711"
  },
  "id": "8252"
}
```
###### Response
```json
  {
    "jsonrpc": "2.0",
    "result": {
      "pickingList": {
        "pickingListIdent": "16-4711",
        "pickingPositions": [
          { "pickingPositionIdent": 1, "binLocationIdent": "PHZ-01-02-03-04", "itemName": "banana", "targetQuantity": 4, "quantityUnit": "coil", "status": "new"},
          {	"pickingPositionIdent": 2, "binLocationIdent": "PHZ-02-19-01-01", "itemName": "apple", "targetQuantity": 2, "quantityUnit": "coil", "status": "new"},
          {	"pickingPositionIdent": 3, "binLocationIdent": "PHZ-08-04-02-03", "itemName": "unicorn", "targetQuantity": 1, "quantityUnit": "unit", "status": "new"}
      	]
      }
    },
    "id": "8252"
  }
```

#### 2.3.2 orderPicking.submitPickingPosition

##### Arguments

  - sessionId: (`string`, `required`)
  - pickingListIdent: (`string`, `required`)
  - pickingPosition: (PickingPosition)

##### Return

##### Example
###### Request
```json
{
  "jsonrpc": "2.0",
  "method": "orderPicking.submitPickingPosition",
  "params": {
    "sessionId": "A64D586FE474",
    "pickingListIdent": "16-4711",
    "pickingPosition": {
      "pickingPositionIdent": 2,
      "status": "processed",
      "pickedQuantity": 1
    }
  },
  "id": "8252"
}
```
###### Response
```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": "8252"
}
```


## 3 Types
### 3.1 System
#### 3.1.1 Credentials
Login information.

##### Properties
- user: (`string`, `required`)
- password: (`string`)
- station: (`string`) - workingplace
- deviceIdent: (`string`) - serialnumber Smart Glasses

##### Example
```json
  {
    "user": "9999",
    "password": "5f4dcc3b5aa765d61d8327deb882cf99",
    "station": "Kom12",
    "deviceIdent": "132A377E3F43C4"
  }
```

#### 3.1.2 Configuration
Change configuration by the server.

##### Properties
  - language: (`string`)
  - handedness: (`string`) - "right" or "left" sets the layout of the ppc (Picavi Power Control) for right-hander or left-hander

##### Example
```json
  {
    "language": "DE",
    "handedness": "right"
  }
```

#### 3.1.3 Client
Client control commands and display information send by the server.

##### Properties
  - action: (`string`) - What to Do

##### Example
```json
  {
    "action": "startPicking"
  }
```

### 3.2 Basic
#### 3.2.1 Job

##### Properties
  - jobIdent: (`string`)
  - multiDestination: (`boolean`) - set true if picks have multi-Destination
  - jobSteps: ([JobStep][], `required`)

##### Example
```json
{
  "jobIdent": "447849",
  "jobSteps": [
    {	"jobStepIdent": "1", "locationIdent": "KOM470004203", "locationName": "KOM-470-004-2-03", "itemIdent": "1-05010033", "itemName": "55 33",
      "destinationIdent": "257593", "destinationName": "UPSK84", "sourceIdent": "2049659", "sourceName": "",
      "targetQuantity": 2, "unit": "Stk", "info": "1 NP, 0 AM", "hasSerial": false, "status": "new"},
		{	"jobStepIdent": "2", "locationIdent": "KOM470007107", "locationName": "KOM-470-007-1-07", "itemIdent": "1-03280010", "itemName": "54 KK 10",
      "destinationIdent": "257593", "destination_name": "UPSK84", "sourceIdent": "2057517", "sourceName": "",
      "targetQuantity": 5, "unit": "Stk", "info": "1 NP, 0 AM", "hasSerial": false, "status": "new"},
		{	"jobStepIdent": "3", "locationIdent": "KOM800014101", "locationName": "KOM-800-014-1-01", "itemIdent": "1-02450008", "itemName": "49 A 1/8",
      "destinationIdent": "257594", "destination_name": "UPSKN9", "sourceIdent": "1993965", "sourceName": "",
      "targetQuantity": 1, "unit": "Stk", "info": "0 NP, 1 AM", "hasSerial": true, "status": "new"}
	]
}
```
#### 3.2.2 JobStep
##### Properties
  - jobStepIdent: (`string`, `required`)
  - locationIdent: (`string`, `required`)
  - locationName: (`string`)
  - itemIdent: (`string`)
  - itemName: (`string`)
  - destinationIdent: (`string`)
  - destinationName: ('string')
  - destinations: (`array`[[Destination][]]) - only if multiDestination is true
  - sourceIdent: (`string`)
  - sourceName: (`string`)
  - targetQuantity: (`number`, `required`)
  - pickedQuantity: (`number`)
  - unit: (`string`)
  - info: (`string`)
  - hasSerial: (`boolean`) - if item has serialnummer and the number had to scan
  - status: (`string`) - pick status (new, started, skipped, processed)

##### Example
```json
  {
    "jobStepIdent": "1",
    "locationIdent": "KOM470004203",
    "locationName": "KOM-470-004-2-03",
    "itemIdent": "1-05010033",
    "itemName": "55 33",
    "destinationIdent": "257593",
    "destinationName": "UPSK84",
    "sourceIdent": "2049659",
    "sourceName": "",
    "targetQuantity": 2,
    "unit": "Stk",
    "info": "1 NP, 0 AM",
    "hasSerial": false,
    "status": "new"
  }
```
#### 3.2.3 Destination
##### Properties
  - destinationIdent(`string`, `required`)
  - destinationName(`string`)
  - targetQuantity (`number`, `required`)

##### Example
```json
  {
    "destinationIdent": "257593",
    "destinationName": "UPSK84",
    "targetQuantity": 2
  }
```
## 4 Info, Warning, Error

### Ranges

#### Types
  - error -10000 to -19999
  - warning 10000 to 19999
  - info 20000 to 29999

#### Groups
within every range there are some areas
  - 0-999 Global
  - 1000-1499 Initialization
  - 1500-1999 Order Picking

##### Properties

  - client: ([Client][])
  - messageDialog: (MessageDialog) - only at "warning"

### Example
#### Info
```json
  {
    "jsonrpc": "2.0",
    "error": {
      "code": 21501,
      "message": "No Order found",
    },
    "id": "8252"
  }
```

#### Warning
```json
  {
    "jsonrpc": "2.0",
    "error": {
      "code": 12001,
      "message": "Wrong location",
      "data": {
        "messageDialog": { "key": "confirm", "text": "Really use this location?", "action": "request", "actionKey": "force"}
      }
    },
    "id": "8252"
  }
```

####  Error
```json
  {
    "jsonrpc": "2.0",
    "error": {
      "code": -10011,
      "message": "Login or password invalid",
      "data": {
        "client": { "action": "scanLogin"}
      }
    },
    "id": "8252"
  }
```
[Introduction]: #1-introduction
[Methods]: #2-methods
[MethodsSystem]: #2-1-system
[system.login]: #2-1-1-system-login
[system.logout]: #2-1-2-system-logout
[MethodsBasic]: #2-2-basic
[basic.requestJobData]: #2-2-1-basic-requestjobdata
[basic.processJobData]: #2-2-2-basic-processjobdata
[basic.requestContextData]: #2-2-3-basic-requestcontextdata
[basic.processContextData]: #2-2-4-basic-processcontextdata
[basic.requestData]: #2-2-5-basic-requestdata
[basic.processData]: #2-2-6-basic-processdata
[MethodsOrderPicking]: #2-3-order-picking
[orderPicking.getPickingList]: #2-3-1-orderpicking-getpickinglist
[orderPicking.submitPickingPosition]: #2-3-2-orderpicking-submitpickingposition
[Types]: #3-types
[TypeSystem]: #3-1-system
[Credentials]: #3-1-1-credentials
[Configuration]: #3-1-2-configuration
[Client]: #3-1-3-client
[TypeBasic]: #3-2-basic
[Job]: #3-2-1-job
[JobStep]: #3-2-2-jobstep
[Destination]: #3-2-3-destination
[Info, Warning, Error]: #4-info-warning-error
