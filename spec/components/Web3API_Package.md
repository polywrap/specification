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

### Package structure

The structure of a package source should be:
```
.
+-- mutation
|   +-- index.ts
|   +-- schema.graphql
+-- query
|   +-- index.ts
    +-- schema.graphql
+-- subgraph
|   +-- index.ts
|   +-- schema.graphql
+-- web3api.yaml
```

When built, the structure of the package (when smart contract and subgraph are used) should be:
```
.
+-- subgraph
|   /* Graph CLI's WASM and ABI output */
+-- mutation.wasm
+-- query.wasm
+-- schema.graphql
+-- web3api.yaml
```

## Web3API manifest schema

Manifest defines the structure of the Web3API package so that the [client]() can load its content for a given user query.
Manifest YAML file is created by a protocol developer at package's build time.

### Manifest schema versioning

Versioning is supported using [JSON schema](https://json-schema.org/) for the formats.
Version in each manifest file is marked as `format` i.e.:

`format: 0.0.1-alpha.1`

The latest format can be found in [Web3API repository](https://github.com/Web3-API/prototype/tree/master/packages/manifest-schema/formats).

Version migrations are supported through defined migrators, meaning that previous manifest versions are accepted and upgraded.

### Manifest schema validation

Manifest is validated at run-time when a package is downloaded to ensure the structure is according to the required format. This includes handling a few cases:
* when a non-accepted field is added to the manifest,
* when the type of the field is unknown,
* when a required field is not sent,
* when version string is not correct,
* when file string is not an existing file.

If the manifest format is not at the latest format version, manifest is upgraded to the latest format.

## Web3API schema
Web3API schema is basically a GraphQL schema composed with package's functions.
It is extended with custom imports and custom scalar types for WASM serialization (uint types).
During the package build time schemas are generated to GraphQL files using predefined templates and resolving imports.  

Schema lifecycle happens through several Web3API core parts:
* `schema-compose`
  * adds Web3API header containing defined scalars and directives
  * resolves local and external dependency imports
  * creates new `mutation.graphql` and `query.graphql` files based on user's input mutation.graphql` and `query.graphql` files
* `schema-parse`
  * responsible for helping client examine the Web3API schema
  * uses the output schema file to create Web3API own AST `TypeInfo` to help clients map native types to [msgpack](), later in serialization
* `schema-bind`
  * responsible for generating code in WASM language
  * creates wrapped methods with arguments serialization and deserialization
  * creates AssemblyScript files for all types with serialization and write functions
 
<!-- Question: add drawing here of the lifecycle? -->
<!-- Question: should TypeInfo be defined/explained or only linked to repo?  -->

### Importing local and external schemas

Queries and objects in Web3API schema can be imported from other schemas.
This can be done by either using an import from a local file or a hosted package.

#### Local import
Web3API handles a local import in the following way:
1. Fetch the local file
2. Check if the local file has the Web3API GraphQL Header types and add if it doesn't
3. Forward the schema to the `schema-parser`
4. Extract the required types from the `TypeInfo` returned by the `schema-parser`
5. Add the extracted types to the schema in which import is called

#### External import
Web3API handles an external import in the following way:
1. Check that import is into a unique namespace
2. Fetch the `schema.graphql` at the specified URI
3. Give the `schema.graphql` to the `schema-parser`
4. Extract the required types from the `TypeInfo` returned by the `schema-parser`
5. Add the extracted types to the schema in which import is called 

## Web3API (WASM) modules 

Modules are written in [AssemblyScript](https://www.assemblyscript.org).
WASM modules are compiled by protocol developer based on package's written AssemblyScript source.
Web3API WASM modules can be:
* `mutation` - can perform write operations (but also read operations if needed) on package
* `query`  - can only perform read operations on package

