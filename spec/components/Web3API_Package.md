# Web3API Package

Web3API packages contain everything needed to execute API invocations on-the-fly. This includes the API's:

- Manifest: `web3api.yaml`
- Schema: `schema.graphql`
- WASM Modules:
  * `mutation.wasm` - Write Operations  
  * `query.wasm` - Read Operations  

## Web3API Manifest
The manifest YAML file defines the structure of the package. It acts as a central directory for all other contents within the package, allowing readers, such as the [Web3API Client](TODO), to easily fetch different pieces of the API such as its schema or WASM modules.

### Manifest Schema

The latest manifest schema can be found here: [TODO](link-to-latest-schema)

### Manifest Formats

New formats of the manifest schema can be introduced as new features are added to the Web3API standard. Manifest files self-define what format of the schema they're using using through the `format` property, i.e.:
`format: 0.0.1-alpha.1`

### Manifest Migrations

Older manifest formats can still remain compatible with newer Web3API clients through [manifest migrations](todo). Manifest migrations are performed at run-time by the client.

### Manifest Validation

The manifest is validated at run-time when a package is downloaded to ensure the structure is according to the required format. This includes handling a few cases:
* unknown property,
* malformed property value,
* missing required properties

Individual property types can have their own sanitization logic, such as:
* format strings
* file paths

## Web3API Schema
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

