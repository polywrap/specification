# The WRAP Standard

At the core of Polywrap is The WRAP Standard. This standard enables SDK developers to create highly-composable modules, and application developers the ability to integrate them with a single line of code.  

WRAP stands for **W**ebAssembly **R**andom **A**ccess **P**rotocol.  

**WebAssembly** - A secure-by-default, portable, light-weight, bytecode standard.
**Random Access** - Any WRAP module is accessible on-demand via its URI.
**Protocol** - A set of rules and procedures for loading & executing WRAP modules.

You can think of WRAP as an interoperability/abstaction standard ontop of WebAssembly.

## Why Another Standard?
With "vanilla" WebAssembly, there are a few short-comings that limit its adoption.

**Composability** - There is not an easy way to create inter-module dependencies, drastically limiting the wasm community's ability to re-use code.

**Interfaces** - There is no standard for defining typed interfaces for your wasm modules. This means that vanilla wasm modules are opaque blobs of bytecode, making it hard to integrate them into applications.

**Complex Data Passing** - By default vanilla wasm only supports the passing of integers between the host <> wasm boundary, making it difficult to pass complex data structures (ex: struct, array, etc)  into / out of / between wasm modules.

**Naming & Addressing** - Just as [any other resource on the web](https://www.w3.org/Addressing/), there needs to be a way to address a wasm module using a tailored URI format.


## How Does WRAP Work?
### Building Wrappers
Building WRAP compatible WebAssembly modules can be broken down into the following steps:

1. **Define an Interface**
    All WRAP modules have semantic interfaces that describe the module's: methods, custom types, and dependencies.

    Related Standards: [Interfaces](TODO)

2. **Generate Bindings**
    WRAP interfaces are "bound" automatically through codegen to the implementation language of the wrappers. These bindings are used to support the easy development of wrappers by providing:
    - Type Safety
    - Data Marshalling
    - Client Interactions

    Related Standards: [Serialization](TODO), [Code Generation](TODO)

3. **Compile Wasm**
    Once all methods have been implemented, wasm-based wrappers must expose all necessary exports (invoke, asyncify, etc), and require imports supported by the client (subinvoke, get implementations, etc).

    Related Standards: [WebAssembly Runtime](TODO)

4. **Deploy Wrapper**
    When deploying a wrapper, it is up to developers to choose the best solution for their needs. There is no finite set of deployment options, and is left open for developers to choose, thanks to the generic URI & URI resolution standards.

    In an effort to support developers in deploying & discovering new wrappers, the Polywrap DAO is actively building new solutions. One such solution is the Polywrap Registry, which enhances reliability through built-in version verification, and composability through interface implementation registries.

    Related Standards: [URI](TODO), [URI Resolution]()

### Using Wrappers

Using wrappers within applications can be done by using a WRAP client. WRAP clients are expected to support the following functionality:

- Custom Configuration (TODO)
- Plugins (TODO)
- URI Resolution (TODO)
- Loading Wrappers (TODO)
- Invoking Wrappers (TODO)


TODO: some examples of what's possible with this standard?
