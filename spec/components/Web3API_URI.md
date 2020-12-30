# Web3API URI

Format of the Web3API URI is based on the [RFC 3986 formal definition](https://tools.ietf.org/html/rfc3986#section-3).

A Web3API URI can be broken down into few components:
* **w3://** - URI Scheme: differentiates Web3API URIs.
* **ipfs/** - URI Authority: allows the Web3API URI resolution algorithm to determine an authoritative URI resolver.
* **sub.domain.eth** - URI Path: tells the Authority where the API resides.

```
      w3://ens/api.domain.eth
      \_/ \___/\_____________/
       |    |          |     
    scheme authority   path
````

Some examples of valid URIs are:
 * w3://ipfs/QmHASH
 * w3://ens/api.domain.eth
 * w3://fs/directory/file.txt
 * w3://uns/domain.crypto
