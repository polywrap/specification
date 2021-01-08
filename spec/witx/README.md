## Generating Markdown

Generating Markdown is done via tool from WASI [repo](https://github.com/WebAssembly/WASI/tree/master/tools/witx):

```bash
cargo run witx docs "path/**/*.witx"
```

### Docker

Also available is a docker image with prepackaged cli:

```bash
docker run -v /path:/usr/src/witx  nodefactory/witx docs "path/**/*.witx"
```
