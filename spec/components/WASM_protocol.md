# W3 WASM Protocol
This module contains all host imports required for querying and invoking functions.

It produces a generated code which is used to handle a number of things for the Web3API developers such as:
 - serialization and deserialization of datatypes
 - implementing the `_w3_call` standard for allowing the host to call query methods
 - wrapping the query methods so that arguments are deserialized before being passed into developer's function
 - importing data types & queries from external modules.
 
## Data serialization
[msgpack](https://msgpack.org) is used for data serialization and deserialization between WASM and host language.
This common data format enables representing data types correctly within both areas.

All **user (package) methods** are wrapped (new functions are generated) with arguments deserialization and serialization of the return data.
Those wrapped functions are added to the w3's `invokes` map that keeps track of all invokable functions and their implementations.
This map of function pointers is initialized in the `_w3_init` function. 

**Objects** can be passed as arguments to functions. For those objects new map is created that contains all object's properties with
additional methods for object serialization and deserialization.
There are two cases that are handled:
* generating *imported* query and object types
* generating *user* query and object types

<!-- Provide example or even better, drawing -->

## Naming conventions

There are several naming conventions used in Web3API development. Those are:
* #### Host import
All hosts imports are prefixed with two underscores (`__`) i.e. `__w3_invoke_args`.

* #### Module export
All exported methods from WASM modules are prefixed with one underscore (`_`) i.e. `_w3_invoke`.

## Module initialization
<!-- https://github.com/Web3-API/prototype/blob/prealpha-dev/packages/schema/bind/src/__tests__/cases/sanity/output/wasm-as/index.ts -->


## Methods imported from host
<!-- invoke a module's method from host (https://github.com/Web3-API/prototype/blob/issue-28/packages/wasm/as/assembly/w3/invoke.ts) & (https://github.com/Web3-API/prototype/blob/issue-28/packages/schema/bind/src/__tests__/cases/sanity/output/wasm-as/index.ts) -->

## Querying Web3APIs from module
<!-- query Web3APIs from module (https://github.com/Web3-API/prototype/blob/issue-28/packages/wasm/as/assembly/w3/query.ts) -->
