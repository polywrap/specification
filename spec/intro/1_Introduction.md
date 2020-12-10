# Introduction

Web3API is a developer toolchain that brings the ease of Web2 to Web3.
It aims to make integrating Web3 protocols into apps seamlessly, without sacrificing decentralization.

In addition, Web3API is built with multi-platform support in mind. The Web3API standard allows Web3 protocols to run on all types of devices in multiple programming languages (Rust, Javascript, Python, Go, C, C#, etc).

For the visual learners, here is the [video presentation](http://video.web3api.eth.link/).

## How it works

At core of Web3API is a WASM runtime that enables interactions with popular P2P networks such as
IPFS, Ethereum, Subgraph and other similar Web3 apps and protocols.
These WASM modules are paired with a [subgraph](https://thegraph.com/) and together they create a single GraphQL schema called Web3API that defines the entirety of a WEB3API package.
On the client (dapp) side Web3API client, together with some GraphQL client, is used to interact with the Web3API package. 

## Hosting WEB3API packages

// IPFS, ENS
