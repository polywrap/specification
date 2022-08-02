# Invocation Standard
`version 1.0`

## Overview
The term "invoke" (or "invocation") refers to the act of executing a method exposed by a wrapper module. This can be done by using a client library that supports the [WRAP Standard](../01.0%20General%20Standard/WRAP%20Standard.md?plain=1) like so:

```typescript
client.invoke({
  uri: "...",
  method: "getData",
  args: {
    arg1: "...",
    arg2: [1, 2, 3]
  }
});
```

The full list of available methods each wrapper implements can be found by viewing its ABI, located in the `wrap.info` manifest.

Wrapper modules can be implemented using WebAssembly, or as Plugins in the language of the client.

**Related Standards**
* [ABI](TODO)
* [WRAP Manifest](TODO)
* [WebAssembly Wrappers](TODO)
* [Plugin Wrappers](TODO)

## Terminology
### Invoker & Invocable
From the perspective of the user interfacing with the WRAP client, the call-graph of an invocation is as follows:  
`client->wrapper`

Where a `wrapper` contains a `module`:  
`wrapper{module}`

Since the client & wrapper constructs are used for more than just invocations, it is important for us to name entities in a more specific way in the context of invocation. `client` will now be referred to as `invoker`, and `wrapper` will now be refered to as `invocable`:  
`invoker->invocable{module}`

### Subinvoke

Similar to other code execution runtimes, the WRAP invocation standard supports nested invocations. Nested invocations are referred to as "subinvocations", or "subinvoke" in its singular form.

As you will read below, when an invocable is called, it is given a reference to the invoker instance that has called it. This enables the invocable to use the invoker's `invoke(...)` method to call other invocables, starting  the whole process over again.

Through the use of subinvocation, it makes it easier to compose wrappers, and create complex interactions.

```
invoker->invocable
             ->invoker->invocable
                            ...
```

## Invoke Options

Invocations require the following options:
```typescript
interface InvokeOptions {
  uri: URI;              // wrapper's URI
  method: String;        // module's schema method
  args?: Object | Bytes; // method's arguments
}
```

**Related Standards**
* [URI](TODO)
* [URI Resolution](TODO)
* [Modules](TODO)

## Invoke Result

Invocations return an object with two attributes, `data` and `error`, one of them must be `undefined` while the other must be set by the client appropriately.

```typescript
interface InvokeResult<TData> {
  data?: TData;
  error?: Error;
}
```

If the invocation is succesful, `data` attribute will be the method's returned value, and `error` will be `undefined`.

If the invocation fails, `error` will be provided, and `data` will be `undefined`.

## Invoker Interface

Invokers (such as the client) must implement the following interface:  
```typescript
interface InvokerOptions extends InvokeOptions {
  encodeResult?: boolean;
}

interface Invoker {
  invoke<TData>(
    options: InvokerOptions
  ): Promise<InvokeResult<TData>>;
}
```

Invokers have an additional option `encodeResult`, allowing the caller to specify how they'd like the return data to be formatted. If `encodeResult == true`, then the return data will be an encoded buffer. Otherwise it will be a decoded object.

The `encodeResult` flag is an important optimization technique used when performing sub-invocations between two wrappers, helping eliminate needless decoding/encoding.

**Related Standards**
* [Data Translation](TODO)

## Invocable Interface

Invocables (such as wasm & plugin wrappers) must implement the following interface:  
```typescript
export interface InvocableResult<TData> extends InvokeResult<TData> {
  encoded?: boolean;
}

export interface Invocable {
  invoke<TData>(
    options: InvokeOptions,
    invoker: Invoker
  ): Promise<InvocableResult<TData>>;
}
```

Invocables have an additional return value `encoded`, which specifies whether the `data` property is an encoded buffer or not. It is common for wasm wrappers to return `encoded == true`.

Invokers will determine how to encode/decode the result `data` based on the value of its `encodeResult` option.

## Client Invoker Implementation
The client's invoke method is responsible for resolving the URI into a wrapper, and dispatching the invocation to the wrapper (invocable) instance.

```typescript
class Client implements Invoker, UriResolver {
  invoke<TData>(options: InvokeOptions): InvokeResult<TData> {
    let resolveResult = this.resolveUri(options.uri);

    if (resolveResult.error) {
      return {
        error: resolveResult.error
      };
    }

    let wrapper = resolveResult.wrapper;
    let result = wrapper.invoke<TData>(options, this);

    if (result.data == undefined) {
      return {
        error: result.error
      };
    }

    // ensure the result is in the format the caller expects
    if (options.encodeResult && !result.encoded) {
      return {
        data: encode(result.data)
      };
    } else if (result.encoded && !options.encodeResult) {
      return {
        data: decode(result.data)
      };
    }

    return {
      data: result.data
    };
  }
}
```

## Wrapper Creation
The creation of the wrapper is done via the [URI Resolution](TODO) process using the URI given to the `invoke` function, which should return to the client an instance of a wrapper. If it does not find a wrapper, it will return the URI Resolution process' error.

### Optimizations
Client implementations can decide how to best avoid excessive re-fetching of wrappers by implementing caching solutions that work best with their unique runtime environments.

## Wrapper Invocation
As previously stated, all wrapper types (wasm, plugin) must implement the same invocable interface:
```typescript
interface Wrapper implements Invocable {
  // invoke(...)
}
```

## Environment Variables

Wrappers may require environment variables, read more about this in the [Wrapper Environment Variables Standard]().

## WebAssembly Wrappers

The process of invoking a wasm wrapper is as follows:
1. Instantiate the `wrap.wasm` module with all necessary imports
2. Ensure the arguments are encoded
3. Get encoded environment
4. Invoke the wasm module's method

```typescript
class WasmWrapper implements Wrapper {
  invoke<TData>(
    options: InvokeOptions,
    invoker: Invoker
  ): InvocableResult<TData> {
    // instantiates a wrap.wasm module instance
    // with all necessary imports
    // NOTE: see WebAssembly Runtime Standard
    let instance = this._getWasmModuleInstance();

    // encode args
    let args = options.args || {};
    let encodedArgs = isBuffer(args) ? args : encode(args);

    // get encoded environment
    let encodedEnv = this._getEncodedEnv();

    // invoke the module's method
    // NOTE: see WebAssembly Runtime Standard
    let result = instance.invoke(
      options.method,
      encodedArgs,
      encodedEnv,
      invoker
    );

    return {
      data: result.data,
      error: result.error,
      encoded: true
    };
  }
}
```

**Related Standards**
* [Wasm Runtime](TODO)

## Plugin Wrappers

The process of invoking a plugin wrapper is as follows:
1. Instantiate the plugin module
2. Ensure the arguments are decoded
3. Get decoded environment
4. Invoke the plugin module's method

```typescript
class PluginWrapper implements Wrapper {
  invoke<TData>(
    options: InvokeOptions,
    invoker: Invoker
  ): InvocableResult<TData> {  
    // instantiates a plugin module instance
    // NOTE: see Plugin Standard
    let instance = this._getPluginModuleInstance();

    // decode args
    let args = options.args || {};
    let decodedArgs = isBuffer(args) ? decode(args) : args;

    // get the environemnt
    let decodedEnv = this._getEnv();

    // invoke the module's method
    // NOTE: see Plugin Standard
    let result = instance.invoke(
      options.method,
      decodedArgs,
      decodedEnv,
      invoker
    );

    return {
      data: result.data,
      error: result.error,
      encoded: false
    };
  }
}
```

**Related Standards**
* [Plugin](TODO)
