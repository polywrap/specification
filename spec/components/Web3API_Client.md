# Web3API Client

Web3API client is intended for application developers that can use this as a single library in their (d)apps to interact with various decentralized services using Web3API's packages.

Currently, Web3API client is available in the following languages:
 - JavaScript

## Client interface

```
class Web3APIClient {
  redirects(): UriRedirect[];
  query(options: QueryApiOptions): QueryApiResult;
}

type UriRedirect {
 // Redirect from this URI or set of URIs specified by the regex.
 from: Uri | RegExp;
 // Destination URI or plugin that will handle invocations
 to: Uri | (() => Plugin)
}

type QueryApiOptions {
  uri: Uri;
  query: string | QueryDocument;
  variables?: Record<string, any>;
}

type QueryApiResult {
 /**
   * Query result data. The type of this value is a named map,
   * where the key is the method's name, and value is the [[InvokeApiResult]]'s data.
   * This is done to allow for parallel invocations within a
   * single query document. In case of method name collisions,
   * a postfix of `_0` will be applied, where 0 will be incremented for
   * each occurrence. If undefined, it means something went wrong.
 */
  data?: [[InvokeApiResult]];
  // errors encountered during the query
  errors?: Error[];
}
```

### Initialization
Client is initialized using `Web3APIClient` class that should support receiving:

#### `redirects` array
This in an unbounded array containing `UriRedirect` objects.
Redirects are used for resolving Web3API URIs to decentralized protocols and their nodes.
 
Upon initizalition it is filled by default redirects:
 - IPFS - required for downloading Web3API packages
 - Ethereum - required for smart contract interactions
 - ENS - required for resolving domain to IPFS hashes

Additionally, user can define own redirects i.e.
```js
{
  from: new Uri("w3://ens/ethereum.web3api.eth"),
  to: new EthereumPlugin(window.ethereum) // JS plugin OR another URI
}
```

### Functions

#### `query` function
Query function receives `options` required for an API query containing:
 - `query` - GraphQL query (AST representation)
 - `variables`
 - `uri` - server uri that should resolve the query.

It is responsible for:
 - converting the query string into a query document
 - parsing the query by calling parse-query algorithm
 - processing API invocations on loaded Web3APIs
 - returning from API result `data` and `errors`.

#### `redirects` function

Returns URI redirects array set upon initialization. 

## URI resolution algorithm

URI redirect can redirect from one URI, or a set of URIs, to a new URI or a plugin  
