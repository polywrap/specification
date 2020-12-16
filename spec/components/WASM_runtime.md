# W3 WASM runtime
// Better naming?

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


