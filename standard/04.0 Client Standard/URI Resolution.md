URI Resolution
==============

URI Resolution is the process the Polywrap client goes through to resolve a URI to a wrapper.

## Prior Knowledge
Readers are expected to have a prior understanding of the [WRAP URI standard](), and the individual components of a `wrap://` URI string.

## Resolution process
The resolution process inside of the client happens in the `resolveUri` method. The method takes in a URI argument, also known as an origin URI.
The result of the resolution contains the following optional properties: 
- URI
- wrapper
- error

Additionally, it returns the URI resolution history.

Starting with an origin URI the client goes through the list of URI resolvers defined in the client configuration. 
For each of the resolvers, a `resolveUri` method is called to delegate resolution of the URI to that resolver. The result can be a URI, a wrapper or an error. 
If the result of the resolver resolution is a wrapper, then the process ends and the client returns the wrapper as a result of the resolution process.
If the result of the resolver resolution is a the same as the origin URI then the process continues with another resolver from the list.
If the result of the resolver resolution is a URI that is different from the origin URI (redirect) then the process restarts with a new origin URI.
During the resolution process, the client keeps track of all URI redirects and if at any point a URI is revisited, the process stops and the client throws an `InfiniteLoop` error.

```typescript=

```


## Notes

It accomplishes this through the use of [URI resolvers](TODO).

- description
- resolveUri Function
    - arguments*
    - result
    - algorithm overview (ref: URI Resolver)
- URI Resolver
  - URI Resolver interface
  - Future Proof URI Resolver (wrapper interface)
  - Example URI resolvers

