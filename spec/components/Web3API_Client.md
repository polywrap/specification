# Web3API Client

Web3API client is intended for application developers that can use this as a single library in their (d)apps to interact with various decentralized services using Web3API's packages.

Currently, Web3API client is available in the following languages:
 - JavaScript

## Client interface

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
#### `redirects` function

## Query handler

## URI resolution algorithm  
