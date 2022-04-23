# letheanVPN/wails-build-action@v1
GitHub action to build Wails.io, the action will install GoLang, NodeJS and run a build.
this is to be used on a [Wails.io](https://wails.io) v2 project.

```yaml
- uses: letheanVPN/wails-build-action@v1
  with:
    build-platform: linux/amd64
```

| Name                     | Default            | Description                                        |
|--------------------------|--------------------|----------------------------------------------------|
| `build-platform`         | `darwin/universal` | Platform to build for                              |
| `wails-version`          | `latest`           | Wails version to use                               |
| `wails-build-webview2`   | `download`         | Webview2 installing [download,embed,browser,error] |
| `go-version`             | `1.17`             | Version of Go to use                               |
| `node-version`           | `16.x`             | Node js version                                    |
| `deno-build`             | ``                 | This gets run into bash, use the full command      |
| `deno-working-directory` | `.`                | This gets run into bash, use the full command      |
| `deno-version`           | `v1.20.x`          | Deno version to use                                |


# Example Build

```yaml
name: Wails build

on: [push, pull_request]
 
jobs:
  linux:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: recursive
      - uses: letheanVPN/wails-build-action@v1
        with:
          build-platform: linux/amd64
      - uses: actions/upload-artifact@v2
        with:
          name: Linux Desktop
          path: build/bin/*
      - name: Release on tagged build
        uses: softprops/action-gh-release@v1
        if: startsWith(github.ref, 'refs/tags/')
        with:
          files: |
            build/bin/*
  windows:
    runs-on: windows-2022
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: recursive
      - uses: letheanVPN/wails-build-action@v1
        with:
          build-platform: windows/amd64
      - uses: actions/upload-artifact@v2
        with:
          name: Windows Desktop
          path: build\bin\*
      - name: Release on tagged build
        uses: softprops/action-gh-release@v1
        if: startsWith(github.ref, 'refs/tags/')
        with:
          files: |
            build\bin\*
  macos:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: recursive
      - uses: letheanVPN/wails-build-action@v1
        with:
          build-platform: darwin/universal
      - uses: actions/upload-artifact@v2
        with:
          name: macOS Desktop
          path: build/bin/*
      - name: Release on tagged build
        uses: softprops/action-gh-release@v1
        if: startsWith(github.ref, 'refs/tags/')
        with:
          files: |
            build/bin/*
```
