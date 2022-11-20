# URI Standard
`version 1.0` [`APPROVED`](https://forum.polywrap.io/t/technical-council-standard-acceptance-uri-standard/287)


The WRAP URI standard allows developers to reference a wrapper through simple strings, much like referencing a website using an HTTP URI string.

## Existing URI Standards
The W3C has published [existing URI standards](https://www.rfc-editor.org/rfc/rfc3986), which we'll be leveraging to create the WRAP URI Standard.

Based on the available ["URI syntax components"](https://www.rfc-editor.org/rfc/rfc3986#section-3), all WRAP URIs must have the following **required** components:
1. [Scheme](https://www.rfc-editor.org/rfc/rfc3986#section-3.1)
2. [Authority](https://www.rfc-editor.org/rfc/rfc3986#section-3.2)
3. [Path](https://www.rfc-editor.org/rfc/rfc3986#section-4.2)

Note: If you're using a WRAP client, the scheme may be prepended by default.

## Scheme
The `wrap://` scheme is used to indicate a WRAP URI. Similar to `http://`, this let's developers know that they must use a compatible WRAP client to resolve the provided URI. The resolution process is detailed in the [WRAP URI Resolution Standard](TODO).

## Authority
WRAP URI authorities can be any UTF-8 string, with some constraints applied to termination.

The authority component is preceded by a double slash ("//") and is terminated by the next slash ("/").

## Path
WRAP URI path is simply defined as everything that comes after the authority's terminating slash.

The path URI is only useful given the context provided by the authority. 
The format and encoding of the path URI is dependent on the authority to which it belongs.

# Example WRAP URIs
## ENS URI
```
  wrap://ens/my.wrapper.eth
  \__/   \_/ \____________/
    |     |        |
scheme authority  path
```

## IPFS URI
```
  wrap://ipfs/QmXUQXwJ8eb8TjXvXJGYNGEjipKpDPUG933FTiakeD2UTE
  \__/   \__/ \____________________________________________/
    |      |                        |
scheme authority                  path
```

## HTTP URI
```
  wrap://http/wrappers.server.io/my-wrapper?version=2
  \__/   \__/ \_____________________________________/
    |      |                     |
scheme authority               path
```

## FileSystem URI
```
  wrap://file/~/path/to/wrapper/directory/
  \__/  \_/\___________________________/
    |    |            |
scheme authority    path
```
