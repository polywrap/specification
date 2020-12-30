# Introduction

[Web3API](https://web3api.dev) brings the ease of Web2 to Web3. It aims to make integrating Web3 protocols into apps seamless, without sacrificing decentralization.

## How It Works

> For the visual learners, here is a [video](http://video.web3api.eth.link/).  

Web3API accomplishes this through a WASM standard & developer toolchain that streamlines Web3 protocol integrations into applications. All of the logic that used to be bundled into JavaScript SDKs (among other languages), is now within light-weight, secure, and portable WASM modules.

Querying these Web3APIs is done through a familiar GraphQL interface, making the app developer experience almost identical to a Web2 service. Instead of sending GraphQL queries to a centralized endpoint like `api.service.com`, app's instead query a "decentralized endpoint" like `api.protocol.eth`.

The Web3API Client resolves the Web3API package hosted at `api.protocol.eth`, downloads it at run-time if not present, and executes the app's queries on the WASM modules.

See [the Architecture section](./2_Architecture.md) for a more thorough deep-dive.

## The Impact  

As a result, Web3API has the following impacts on the Web3 space:  
* **Streamlined Integration** - Query any Web3API on-the-fly, simply provide a URI.
* **Simple Interface** - With GraphQL, Web3APIs are as easy to use as Web2 APIs.
* **Multi-Platform** - Use Web3 protocols in any programming language using a Web3API Client.
* **Automatic Upgrades** - Resolving packages at run-time allows for automatic upgrades for app developers (opt-in).
* **Limitless Composability** - Since Web3APIs aren't bundled, composability has no limitations.
* **Extendable Protocols** - Web3APIs can query eachother and define standard interfaces, enabling fully extendable protocols.
