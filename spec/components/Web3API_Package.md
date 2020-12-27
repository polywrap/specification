# Web3API package

A package enables connection and interaction with a decentralized service or app (i.e. Uniswap).
Packages should be created by protocol developers. 

A Web3API package is consisted of: 

- Web3API Manifest file (`web3api.yaml`) 
- Web3API API schema (`schema.graphql`)
- Web3API (WASM) modules
  * write operations (`mutation.wasm`)
  * read operations (`query.wasm`)
- Historical data (Graph's subgraph) 

## Web3API manifest schema

Manifest defines the structure of the Web3API package so that the [client]() can load its content for a given user query.
Manifest YAML file is created by a protocol developer at package's build time.

### Manifest schema versioning

Versioning is supported using [JSON schema](https://json-schema.org/) for the formats.
Version in each manifest file is marked as `format` i.e.:

`format: 0.0.1-alpha.1`

Latest format can be found in [Web3API repository](https://github.com/Web3-API/prototype/tree/master/packages/manifest-schema/formats).

Version migrations are supported through defined migrators, meaning that previous manifest versions are accepted and upgraded.

### Manifest schema validation

Manifest is validated at run-time when package is downloaded to ensure the structue is according to the required format. This includes handling few cases:
* when a non-accepted field is added to the manifest,
* when the type of the field is unknown,
* when a required field is not sent,
* when version string is not correct,
* when file string is not an existing file.

If manifest format is not at the latest format version, manifest is upgraded to the latest format.

## Web3API schema

### Importing local and external schemas

## Web3API (WASM) modules 
