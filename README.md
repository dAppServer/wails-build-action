# letheanVPN/wails-build-action@v1
GitHub action to build Wails.io, the action will install GoLang, NodeJS and run a build.
this is to be used on a [Wails.io](https://wails.io) v2 project.

```yaml
- uses: letheanVPN/wails-build-action@v1
  with:
    build-name: wailsApp
    build-platform: linux/amd64
```

## GitHub Action Options

| Name                                 | Default                | Description                                        |
|--------------------------------------|------------------------|----------------------------------------------------|
| `build-name`                         | none, required input   | The name of the binary                             |
| `build-platform`                     | `darwin/universal`     | Platform to build for                              |
| `wails-version`                      | `latest`               | Wails version to use                               |
| `wails-build-webview2`               | `download`             | Webview2 installing [download,embed,browser,error] |
| `go-version`                         | `1.17`                 | Version of Go to use                               |
| `node-version`                       | `16.x`                 | Node js version                                    |
| `deno-build`                         | ``                     | This gets run into bash, use the full command      |
| `deno-working-directory`             | `.`                    | This gets run into bash, use the full command      |
| `deno-version`                       | `v1.20.x`              | Deno version to use                                |
| `sign-macos-app-id`                  | ''                     | ID of the app signing cert                         |
| `sign-macos-apple-password`          | ''                     | MacOS Apple password                               |
| `sign-macos-app-cert`                | ''                     | MacOS Application Certificate                      |
| `sign-macos-app-cert-password`       | ''                     | MacOS Application Certificate Password             |
| `sign-macos-installer-id`            | ''                     | MacOS Installer Certificate id                     |
| `sign-macos-installer-cert`          | ''                     | MacOS Installer Certificate                        |
| `sign-macos-installer-cert-password` | ''                     | MacOS Installer Certificate Password               |

## Example with Code signing

```yaml
    - uses: letheanVPN/wails-build-action@v1
    with:
      build-name: wailsApp
      build-platform: darwin/universal
      sign-macos-apple-password: ${{ secrets.APPLE_PASSWORD }}
      sign-macos-app-id: ${{ secrets.MACOS_DEVELOPER_CERT_ID }}
      sign-macos-app-cert: ${{ secrets.MACOS_DEVELOPER_CERT }}
      sign-macos-app-cert-password: ${{ secrets.MACOS_DEVELOPER_CERT_PASSWORD }}
      sign-macos-installer-id: ${{ secrets.MACOS_INSTALLER_CERT_ID }}
      sign-macos-installer-cert: ${{ secrets.MACOS_INSTALLER_CERT }}
      sign-macos-installer-cert-password: ${{ secrets.MACOS_INSTALLER_CERT_PASSWORD }}
```

## Example Build

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
          build-name: wailsApp
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
          build-name: wailsApp
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
          build-name: wailsApp
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
