# Architecture Overview
Web3API's high-level architecture (as it pertains to Web3API-enabled applications)

![Architecture](../assets/Architecture.png)

## Components

- [Web3API Client](../components/Web3API_Client.md) - Execution Environment  
- [Web3API URI](../components/Web3API_URI.md) - Uniform Resource Identifier (URI)  
- [Web3API Package](../components/Web3API_Package.md) - Descriptive & Executable Resources
- [Web3API Plugins](../components/Web3API_Plugins.md) - Non-WASM Based Web3APIs
- [Web3API WASM Protocol](../components/Web3API_WASM_Protocol.md) - { Client <> WASM Module } Communication Protocol

## Step-By-Step Walkthrough

### **0: Initialize**  
Applications must first include a [Web3API Client](../components/Web3API_Client.md) library. 
When creating a Web3API Client instance, [additional URI redirects and plugins](TODO) may be provided. 
Web3API Clients provide the recommended [default redirects and plugins](TODO), enabling out of the box usability. 

### **1: Query an API**  
Applications query APIs by specifying the API's [URI](../components/Web3API_URI.md) and providing a GraphQL query.

URIs can be of any type so long as there is a compatible `uri-resolver`. The most common URI types are [Ethereum Name Service (ENS)](https://ens.domains/) domains (example: `w3://ens/domain.eth`) and [InterPlanetary File System (IPFS)](https://ipfs.io/) hashes (example: `w3://ipfs/QmHASH`).  

GraphQL is the default query language of Web3API clients. Other formats can be supported, such as a simple web request format (example: `w3://ens/api.eth/mutation/method&arg=5`).

Details on the Web3API client's query interface can be found [here](TODO).

### **2: Fetch API at URI**  

[Web3API URIs](../components/Web3API_URI.md) are resolved based on the [Web3API Client](../components/Web3API_Client.md)'s [URI resolution algorithm](../components/Web3API_Client.md#algorithms-resolve-uri).  

In cases where the URI is redirected to a [plugin](../components/Web3API_Plugins.md), a new plugin instance is instantiated.  

In all other cases, a [URI resolver](TODO) must be found for the provided [URI authority](TODO). This resolver is used to fetch another URI or the [Web3API Package's manifest](TODO). By providing the manifest, the current URI resolver demonstrates that it implements the [API resolver]() standard interface. Using this interface, the API's schema and WASM modules can be fetched. This provides enough to instantiate the WASM Web3API instance.  

For example, an ENS domain may resolve to an IPFS hash, which then resolves to a Web3API Package's manifest. For a detailed step-by-step breakdown of how this works, see the [URI resolution algorithm]((../components/Web3API_Client.md#algorithms-resolve-uri)).  

### **3: Parse Query & Build Invocations**  

The query provided by the user is parsed using the [parse query algorithm](../components/Web3API_Client.md#algorithms-parse-query). This algorithm outputs one or more [invocation configurations](TODO) that detail the module, method, input arguments, and requested results that will be called by the client.  

### **4: Perform Invocations on API Modules**  

Each invocation is performed on the Web3API in question. Within an invocation, one or more sub-queries may be performed on other Web3APIs.  

### **5: Get Query Result**  

The aggregate result of all invocations will then be returned back to the application, including any errors encountered.  
