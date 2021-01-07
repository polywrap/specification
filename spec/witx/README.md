## Generating Markdown

Generating Markdown is done via tool from WASI [repo](https://github.com/WebAssembly/WASI/tree/master/tools/witx):

```bash
cargo run --example witx docs -o target.md target.witx
```

### Docker

Also available is a docker image with prepackaged cli:

```bash
docker run -v /path:/usr/src/witx  nodefactory/witx docs -o target.md target.witx
```
