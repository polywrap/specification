# Web3API Client

Web3API client is an execution environment intended for application developers that can use the client as a single library in their (d)apps to interact with various decentralized services using Web3API's packages.

Currently, Web3API client is available in the following languages:
 - JavaScript (TypeScript)

## Client interface
The following snippet contains the interface that client should implement.

```
interface Web3APIClient {
  redirects(): UriRedirect[];
  query(options: QueryApiOptions): QueryApiResult;
}

type UriRedirect {
 // Redirect from this URI or set of URIs specified by the regex
 from: Uri | RegExp;
 // Destination URI or plugin that will handle invocations
 to: Uri | (() => Plugin)
}

type QueryApiOptions {
  // Target URI that is being queried
  uri: Uri;
  // GraphQL query
  query: string | QueryDocument;
  // Optional variables that are part of the GraphQL query
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
Redirects are used for resolving Web3API URIs to decentralized protocols and their API services.
Upon initialization it is filled with [default redirects](#default-uri-redirects) to enable most common integration (Ethereum, IPFS, ENS) out of the box.

Additionally, user can define own redirects i.e.
```js
{
  from: new Uri("w3://ens/ethereum.web3api.eth"),
  to: new EthereumPlugin(window.ethereum) // JS plugin OR another URI
}
```

#### Default URI redirects

The included and recommended default API redirects are:
* IPFS - required for downloading Web3API packages
  * `"w3://ens/ipfs.web3api.eth"` to IPFS core plugin and default provider
* Ethereum - required for smart contract interactions
  * `w3://ens/ethereum.web3api.eth` to Ethereum core plugin and default provider
* ENS - required for resolving domain to IPFS hashes
  * `w3://ens/ens.web3api.eth` to ENS core plugin

### Functions

#### `query` function
Query function receives `options` required for an API query containing:
 - `query` - GraphQL query (AST representation)
 - `variables` - (optional) key value records included in query
 - `uri` - server uri that should resolve the query

It is responsible for:
 - converting the query string into a query document
 - parsing the query by calling parse-query algorithm
 - processing API invocations on loaded Web3APIs
 - returning from API result `data` and `errors`

#### `redirects` function

Returns URI redirects array set upon initialization.
Contains default redirects plus additional user provided redirects.

## URI resolution algorithm

A URI redirect is a translation from a given URI pattern (string or regex) to a destination URI (string or plugin).
Therefore, it can redirect from one URI, or a set of URIs, to a new URI or a plugin. 
Redirects enable users to provide additional domain resolvers, override existing or have a code in native language that needs to be used as a part of Web3API as a plugin.  

The following URI resolution logic could look like this:

```
// A Web3API URI
/**
  * Breaking down the various parts of the URI, as it applies
  * to [the URI standard](https://tools.ietf.org/html/rfc3986#section-3):
  * **w3://** - URI Scheme: differentiates Web3API URIs.
  * **ipfs/** - URI Authority: allows the Web3API URI resolution algorithm to determine an authoritative URI resolver.
  * **sub.domain.eth** - URI Path: tells the Authority where the API resides.
*/
interface Uri {
    authority(): string;
    path(): string;
    uri(): stirng;
    isUri(value: object): boolean;
    isValidUri(uri: string, parsed?: UriConfig): boolean;
    parseUri(uri: string): UriConfig;
}

// URI configuration
type UriConfig {
  authority: string;
  path: string;
  uri: string;
}

function resolveUri(
  uri: Uri,
  client: Client,
  createPluginApi: (uri: Uri, plugin: () => Plugin) => Api
  createApi: (uri: Uri, manifest: Manifest, apiResolver: Uri) => Api
): Api {
  // 1. Resolve final URI first
  let resolvedUri;
  for (const redirect of redirects) {
     // query interfaces to see if they accept the URI
     const resolvedTo = resolveUriOrPlugin(redirect.from);
     if (isUri(redirect.from)) {
       // recursively track redirects until final URI is reached
       resolvedUri = trackUriRedirect(redirect);
    } else {
        // plugin has been resolved
        return createPluginApi(uri, resolvedUri);
    }
  }

  // 2. Resolve the Web3API package
  for (uriResolver in UriResolverImplementations) {
    if (uriResolver.isSupported(resolvedUri)) {
        let result = implementation.resolve(resolvedUri);
        if (isUri(result)) {
            // result is another URI that needs resolving
            resolvedUri = trackUriRedirect(result);
        }
        const data = uriResolver.resolveUriPath(resolvedUri);
        return createApi(resolvedUri, data.manifest, uriResolver);
    }
  }

  throw Error("No Web3API found at URI");
}
```

### URI resolution example

In the case of a common scenario (ENS + IPFS) i.e. `w3://ens/api.uniswap.eth`, the following high-level steps will be taken:
1. Find an uri-resolver that supports the `ens` authority.
2. Try to resolve the URI path to get IPFS content identifier (CID)
3. Resolve new URI until manifest is found at the URI resolver.
4. Create new API instance that can be used now for invocations.
