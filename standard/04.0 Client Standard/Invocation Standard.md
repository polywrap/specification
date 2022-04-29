# Invocation Standard

## Overview
The term "invoke" (or "invocation") refers to the act of executing a method exposed by a wrapper module. This can be done through the client like so:  

```typescript=
client.invoke({
  uri: "...",
  module: "query",
  method: "getData",
  args: {
    arg1: "...",
    arg2: [1, 2, 3]
  }
});
```

The wrapper being invoked could be implemented as a WebAssembly or plugin wrapper.

## Invoke Options

```typescript=
interface InvokeOptions {
  uri: String;           // wrapper's URI
  module: String;        // wrapper's module
  method: String;        // module's method
  args?: Object | Bytes; // method's arguments
  noDecode?: Boolean;    // toggle result decoding
}
```

**Related Standards**
* [URI](TODO)
* [Wrapper Modules](TODO)

## Invoke Result
```typescript=
interface InvokeResult<TData> {
  /**
  * Invoke result data. The type of this value is the return type
  * of the method. If undefined, it means something went wrong.
  * Errors should be populated with information as to what happened.
  * Null is used to represent an intentionally null result.
  */
  data?: TData;

  /** Errors encountered during the invocation. */
  error?: Error;
}
```

## Client Invoke
```typescript
class Client {
  invoke(options: InvokeOptions): InvokeResult {
    wrapper = loadWrapper(options.uri)
    return wrapper.invoke(options)
  }
}
```

## Load Wrapper
TODO: link to another standard here?

## Wasm Wrapper Invoke
```typescript
class WasmWrapper {
  invoke(options: InvokeOptions, env: EnvArgs) {
    // setup
    wasm = loadWasm(options.module)
    instance = createWasmInstance(wasm)
    sanitizeAndLoadEnv(wasm, env)

    // invocation
    result = instance.invoke(options.method, options.args)

    if (options.noDecode)
      return result
    else
      return decode(result)
  }
}
```

TODO: note about subinvoke

**Related Standards**
* [Encoding](TODO)
* [Wasm Runtime](TODO)
* [Module Environments](TODO)

## Plugin Wrapper Invoke
TODO

**Related Standards**
* [Plugin](TODO)
* [Module Environments](TODO)













---
# Examples
* ERC Standards: https://eips.ethereum.org/erc
* Chain Agnostic Standards: https://github.com/ChainAgnostic/CAIPs
* Python Standards: https://peps.python.org/pep-0000/
* Ethereum Paper: https://ethereum.github.io/yellowpaper/paper.pdf
* GraphQL Spec: https://spec.graphql.org/October2021/

# Question
* Should we invoke interface implemenations just like wasm & plugins?

# Feedback
- I think maybe there should be more elaboration in words, so that the standard is super clear. I say that in part because the example standards (e.g. EIPs) are really detailed in their descriptions.
    - You could write out in words what happens when an invocation occurs. First the URI is resolved, then the API is possibly downloaded and cached, then it is loaded...
    - It could be useful to explain why the invocation standard is as it is, and not some other way.
    - Maybe discuss why the InvokeApiResult<T> type is as it is.
- Need to find a healthy balance between having enough information & being too verbose.
    - EIPs are a bit verbose
    - CAIPs are pretty bare bones
