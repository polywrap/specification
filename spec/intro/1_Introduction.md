# Introduction

[Web3API](https://web3api.dev) brings the ease of [Web2 to Web3](https://ethereum.org/nl/developers/docs/web2-vs-web3/).  It aims to make integrating Web3 protocols into applications seamless, without sacrificing decentralization.

## How It Works

> For the visual learners, here is a [video](http://video.web3api.eth.link/).  

Web3API accomplishes this through a WebAssembly (WASM) standard and a developer toolchain that streamlines Web3 protocol integrations.  
All logic that was once bundled into JavaScript SDKs (among other languages) is now within lightweight, secure, and portable WASM modules called Web3APIs.

Querying these Web3APIs is done through a familiar [GraphQL](https://graphql.org/) interface resulting in a developer experience almost identical to that of a Web2 web service.  
Instead of sending GraphQL queries to a centralized endpoint, such as `api.service.com`, apps query a decentralized endpoint like `api.protocol.eth`.

The Web3API Client resolves the Web3API package hosted at its decentralized endpoint, downloads it at runtime (if not present), and executes the application's queries on the WASM modules.

See [the Architecture section](./2_Architecture.md) for a deep dive.

## Impact  

As a result, Web3API has this impact on Web3:  
* **Streamlined Integration** - Query any Web3API on-the-fly, by simply providing a URI.
* **Simple Interface** - With GraphQL, Web3APIs are as easy to use as Web2 web services.
* **Multi-Platform** - Use Web3 protocols in any programming language by using a Web3API Client.
* **Automatic Updates** - Resolving packages at runtime allows for automatic (opt-in) updates.
* **Limitless Composability** - Since Web3APIs are not bundled, there are no limitations on composability.
* **Extendable Protocols** - Web3APIs can query each other and define standard interfaces, enabling fully extendable protocols.
