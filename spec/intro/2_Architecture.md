# Architecture overview
The following schema represents the overall architecture of the Web3API and how it runs inside the user application.

![Architecture](../assets/Architecture.png)

First, an application (dapp) should have Web3API client installed and initialized to be able to use a Web3API package.
Upon initialization, there are default plugins and URI redirects provided, however users can override that or provide additional redirect rules.
More on this can be found in [Web3API Client](../components/Web3API_Client.md).

1. Application is querying packages using a URI. This can be any type of URI but most for the project most common is ENS URI i.e. `w3://ens/uniswap.eth` or IPFS hash.
All calls (both mutations and queries) to the package are done through a created GraphQL API found in a Web3API package.

2. URI is resolved based on Web3API's [resolve-uri algorithm](../components/Web3API_Client.md#). In case it has recognized that URI is external, then package files are downloaded from decentralized storage such as IPFS.
A package is the one that defines it's schema (mutations and queries) and creates a subgraph for data indexing.

3. When API is retrieved, the query is being parsed using `parse-query algorithm` to understand what's being invoked.
This includes validating query, parsing and recognizing the module (query or mutation), method and its input arguments, variable values and requested results.

4. Depending on the module parsed, WASM (package) or plugin API is invoked. Plugins are part of the Web3API library while WASM modules run on separate threads that don't load app's main execution and can be paused when needed.
WASM may invoke another (sub) query which takes the execution recursively to step 2. 

5. Executed functions return the result back to the invoke function that returns query result which includes retrieved data or errors.  


## Components

Main components of the system can be divided into:
- [Web3API Client](../components/Web3API_Client.md) - contains exposed interfaces and methods that (d)apps can use 
- [Web3API Packages](../components/Web3API_Package.md) - contain API for interaction with decentralized protocols and apps
- [Web3API Plugins](../components/Web3API_Plugins.md) - core plugins that contain host methods that packages can use i.e. for Ethereum interactions
- [W3 WASM protocol](../components/WASM_protocol.md) - contains all host imports required for querying and invoking functions

 
