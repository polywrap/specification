# Web3API Package

Web3API packages contain everything needed to perform API invocations. This includes the API's:

- Manifest: `web3api.yaml`
- Schema: `schema.graphql`
- WASM Modules:
  * `mutation.wasm` - Write Operations  
  * `query.wasm` - Read Operations  

## Web3API Manifest

The manifest YAML file defines the structure of the package. It acts as a central directory for all other contents within the package, aalpha.1llowing readers, such as the [Web3API Client](TODO), to easily fetch different pieces of the API such as its schema or WASM modules.

### **Schema:**

The latest manifest schema can be found here: [TODO](link-to-latest-schema)

### **Formats:**

New formats of the manifest schema can be introduced as new features are added to the Web3API standard. Manifest files self-define what format of the schema they're using using through the `format` property, i.e.:
`format: 0.0.1-alpha.1`

### **Migrations:**

Older manifest formats can still remain compatible with newer Web3API clients through [manifest migrations](todo). Manifest migrations are performed at run-time by the client.

### **Validation:**

The manifest is validated at run-time when a package is downloaded to ensure the structure is according to the required format. This includes handling a few cases:
* unknown property,
* malformed property value,
* missing required properties

Individual property types can have their own sanitization logic, such as:
* format version strings
* file paths

## Web3API Schema

The Web3API Schema, written in GraphQL, declares all invocation methods that are exported from the API's modules. Custom object types can be defined, and used as both input argument and return types.

Types can be imported from other schemas as well. Local and externally hosted imports are both supported.

### **Invocation Modules**
TODO: show how to define query & mutation module methods

### **Base Types**

Supported base types include:  
TODO - update with full list
| Type | Description |
|---------------------|-------------|
| UInt | 32-bit unsigned integer. |
| UInt8 | 8-bit unsigned integer. |
| UInt16 | 16-bit unsigned integer. |
| UInt32 | 32-bit unsigned integer. |
| UInt64 | 64-bit unsigned integer. |
| Int | 32-bit signed integer. |
| Int8 | 8-bit signed integer. |
| Int16 | 16-bit signed integer. |
| Int32 | 32-bit signed integer. |
| Int64 | 64-bit signed integer. |
| String | UTF-8 string. |

### **Object Types**

TODO: describe how object types are defined

### **Local Imports**

TODO: describe how to import local files

Web3API handles a local import in the following way:
1. Fetch the local file
2. Check if the local file has the Web3API GraphQL Header types and add if it doesn't
3. Forward the schema to the `schema-parser`
4. Extract the required types from the `TypeInfo` returned by the `schema-parser`
5. Add the extracted types to the schema in which import is called

### **External Imports**

TODO: describe how to import external imports

Web3API handles an external import in the following way:
1. Check that import is into a unique namespace
2. Fetch the `schema.graphql` at the specified URI
3. Give the `schema.graphql` to the `schema-parser`
4. Extract the required types from the `TypeInfo` returned by the `schema-parser`
5. Add the extracted types to the schema in which import is called 

### **Schema Tooling**

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

## Web3API (WASM) modules 

TODO: supported languages

Modules are written in [AssemblyScript](https://www.assemblyscript.org).
WASM modules are compiled by protocol developer based on package's written AssemblyScript source.
Web3API WASM modules can be:
* `mutation` - can perform write operations (but also read operations if needed) on package
* `query`  - can only perform read operations on package
