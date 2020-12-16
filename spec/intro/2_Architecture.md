# Architecture overview
The following schema represents the overall architecture of the Web3API and how Web3API packages are used.

![Architecture](../assets/Architecture.png)

For example, if a (d)app is trying to use a Web3API package then Web3API client should be installed and initialized. (D)app is querying packages using their ENS URI i.e. `w3://ens/uniswap.eth`.
Upon Web3API client initialization, packages are also loaded. This is done in the following order:
1. ENS URI is resolved to IPFS hash using existing Web3API's ENS plugin
2. Package files are downloaded from IPFS
3. Package is the one that defines it's schema (mutations and queries) and creates a subgraph. Packages interact with decentralized protocols such as Ethereum using W3 helper methods.
4. As packages (WASM code) are sandboxed environements, W3 WASM runtime module contains all host imports (helper methods for queries).
Using Web3API package schema, W3 helper methods and Web3API's schema binding the AssemblyScript (WASM) code is generated.

All calls (both mutations and queries) to the package are done through GraphQL API.

## Components

Main components of the system can be divided into:
- [Web3API Client](../components/Web3API_Client.md) - contains exposed interfaces and methods that (d)apps can use 
- [Web3API Packages](../components/Web3API_Package.md) - contain API for interaction with decentralized protocols and apps
- [Web3API Plugins](../components/Web3API_Plugins.md) - core plugins that contain host methods that packages can use i.e. for Ethereum interactions
- [Web3API WASM runtime](../components/WASM_runtime.md) - 

 
