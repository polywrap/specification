# W3 WASM Protocol
This module contains all host imports required for querying and invoking functions.

It produces a generated code which is used to handle a number of things for the Web3API developers such as:
 - serialization and deserialization of datatypes
 - implementing the `_w3_call` standard for allowing the host to call query methods
 - wrapping the query methods so that arguments are deserialized before being passed into developer's function
 - importing data types & queries from external modules.
 
## Data serialization
Explain `msgpack`

## Host methods?
## Schema binding?


/*
TODO: document...
* host import naming convention (__NAME)
* module export naming convention (_NAME)
* the various protocol routines including...
  * initializing a module (https://github.com/Web3-API/prototype/blob/issue-28/packages/schema/bind/src/__tests__/cases/sanity/output/wasm-as/index.ts)
  * query Web3APIs from module (https://github.com/Web3-API/prototype/blob/issue-28/packages/wasm/as/assembly/w3/query.ts)
  * invoke a module's method from host (https://github.com/Web3-API/prototype/blob/issue-28/packages/wasm/as/assembly/w3/invoke.ts) & (https://github.com/Web3-API/prototype/blob/issue-28/packages/schema/bind/src/__tests__/cases/sanity/output/wasm-as/index.ts)
  * data de/serialization (all objects passed in are named maps, see here for example: https://github.com/Web3-API/prototype/tree/issue-28/packages/schema/bind/src/__tests__/cases/sanity/output/wasm-as/CustomType)
*/
