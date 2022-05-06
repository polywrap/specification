# URI Standard

The WRAP URI standard allows developers to reference wrapper through simple strings, much like referencing a website using an HTTP URI string.

## Existing URI Standards
The W3C has published [existing URI standards](https://www.rfc-editor.org/rfc/rfc3986), which we'll be leveraging to create the WRAP URI Standard.

Based on the available ["URI syntax components"](https://www.rfc-editor.org/rfc/rfc3986#section-3), all WRAP URIs must have the following **required** components:
1. [Scheme](https://www.rfc-editor.org/rfc/rfc3986#section-3.1)
2. [Authority](https://www.rfc-editor.org/rfc/rfc3986#section-3.2)
3. [Target](https://www.rfc-editor.org/rfc/rfc3986#section-4.2)

Note: If you're using a WRAP client, the scheme may be prepended by default.

## Scheme
The `wrap://` scheme is used to indicate a WRAP URI. Similar to `http://`, this let's developers know that they must use a compatible WRAP client to resolve the provided URI. The resolution process is detailed in the [WRAP URI Resolution Standard](TODO).

## Authority
WRAP URI authorities can be any [valid authority string](https://www.rfc-editor.org/rfc/rfc3986#section-3.2), with some constraints applied to termination.

The authority component is preceded by a double slash ("//") and is terminated by the next slash ("/").

## Target
WRAP URI target is simply defined as everything that comes after the authority's terminating slash.

The target URI is only useful given the context provided by the authority. For some URI authorities, there may be a special syntax that's supported within the target URI, such as query arguments.

# Example WRAP URIs
## ENS URI
```
  wrap://ens/my.wrapper.eth
  \__/   \_/ \____________/
    |     |        |
scheme authority  target
```

## IPFS URI
```
  wrap://ipfs/QmXUQXwJ8eb8TjXvXJGYNGEjipKpDPUG933FTiakeD2UTE
  \__/   \__/ \____________________________________________/
    |      |                        |
scheme authority                 target
```

## HTTP URI
```
  wrap://http/wrappers.server.io/my-wrapper?version=2
  \__/   \__/ \_____________________________________/
    |      |                     |
scheme authority               target
```

## FileSystem URI
```
  wrap://fs/~/path/to/wrapper/directory/
  \__/  \_/\___________________________/
    |    |            |
scheme authority   target
```
