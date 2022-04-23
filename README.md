# letheanVPN/wails-build-action@v1
GitHub action to build Wails.io

```yaml
name: Wails build

on: [push, pull_request]
 
jobs:
  linux:
    runs-on: ubuntu-latest
    steps:
      # Checkout code
      - uses: actions/checkout@v2
        with:
          submodules: recursive
      - uses: letheanVPN/wails-build-action@main
        with:
          build-platform: linux/amd64
      - uses: actions/upload-artifact@v2
        with:
          name: Linux Desktop
          path: build/bin/*
```

| Name                     | Default            | Description                                   |
|--------------------------|--------------------|-----------------------------------------------|
| `build-platform`         | `darwin/universal` | Platform to build for                         |
| `wails-version`          | `latest`           | Wails version to use                          |
| `go-version`             | `1.17`             | Version of Go to use                          |
| `node-version`           | `16.x`             | Node js version                               |
| `deno-build`             | ``                 | This gets run into bash, use the full command |
| `deno-working-directory` | `.`                | This gets run into bash, use the full command |
| `deno-version`           | `v1.20.x`          | Deno version to use                           |