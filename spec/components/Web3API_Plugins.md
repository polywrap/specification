# Web3API plugins

Web3API plugin is an implementation of a Web3API in the host language of the Web3API client i.e. JavaScript.
Plugins allow importing JavaScript based modules into Web3API packages.

When creating a new Web3API client instance, user can specify [URI redirects](Web3API_Client.md#uri-resolution-algorithm) to a plugin for any given URI.

These are the currently existing plugins:

- Ethereum
  * contains Ethereum client for interaction with the Ethereum network
  * implements default Web3API URI for Ethereum
  * implements `setProvider` method for updating Ethereum provider API
  * contains additional Ethereum-related methods: `setSigner`, `getSigner`, `getContract`, `deployContract`, `callView` and `sendTransaction`.
- IPFS
  * contains IPFS client for interaction with IPFS storage
  * implements default Web3API URIs for IPFS
  * implements `setProvider` method for updating IPFS provider API
  * contains additional IPFS-related methods: `add`, `cat`, `catToString`, `catToBuffer`, `ls` and `isCID`.
- ENS
  * implements default Web3API URIs for ENS
  * implements `setAddress` method for updating the address of root register
  * contains additional ENS-related methods: `ensToCID` and `isENSDomain`.
- Graph node
  * contains GraphQL client for interaction with the Graph API service
  * contains additional Graph-related methods: `query`.

## Plugin interface
The following snippet contains the interface that a plugin should implement.

```js
interface Plugin {
    // Checks if provided API is implemented by the plugin
    isImplemented(uri: Uri): boolean;
    // Returns all APIs implemented by the plugin
    implemented(): Uri[];
    // Returns all API dependencies imported by the plugin
    imported(): Uri[];
    // Returns plugin's modules
    // Client instance requesting the module will be used for any sub-querries
    getModules(client: Client): PluginModules;
}

/**
 * A plugin "module" is a named map of [[PluginMethod | invocable methods]].
 * The names of these methods map 1:1 with the schema's query methods.
 */
type PluginModule = Record<string, PluginMethod>
// Invocable plugin method 
type PluginMethod {
    // Input arguments for the method, structured as a map, removing the chance of incorrectly ordering arguments.
    input: Record<string, any>;
    // The client instance requesting the invocation.
    client: Client;
}
```

