# quickbit-native-nodejs-mobile

[NodeJS Mobile](https://github.com/nodejs-mobile/nodejs-mobile) prebuilds for [`quickbit-native`](https://github.com/holepunchto/quickbit-native)

## Working locally

### Requirements

- Node 18
- Android NDK (CI uses version 27.2.12479018)
  - (optional) exported `ANDROID_NDK_HOME` environment variable

### General steps

Should be clear enough to follow the [reusable workflow steps](https://github.com/digidem/nodejs-mobile-bare-prebuilds/blob/main/.github/workflows/prebuild.yml) but in summary:

1. Download the npm tarball package and unzip e.g.
    ```
    npm pack quickbit-native@latest | xargs tar -zxvf
    ```
2. Navigate to unzipped directory:
   ```
   cd package
   ```
3. Install dependencies:
   ```
   npm install
   ```
4. Install [patched `cmake-napi`](https://github.com/digidem/cmake-napi-nodejs-mobile):
   ```
   npm install cmake-napi@github:digidem/cmake-napi-nodejs-mobile
   ```
5. Install [bare-make](https://github.com/holepunchto/bare-make) globally:
   ```
   npm install -g bare-make@latest
   ```
6. Generate, build and install:
   ```
   bare-make generate --platform android --arch arm64
   bare-make build
   bare-make install
   ```

## Patches

`quickbit-native` needs a small Android-specific patch to `CMakeLists.txt`.
Patches live under `patches/` using the `patch-package` filename convention:
`<module>+<version>.patch`. When bumping to a `quickbit-native` version not
already covered, copy the existing patch to the new filename (and adjust if
upstream's `CMakeLists.txt` has changed). The build fails loudly if the
requested version has no matching patch while others exist, so a forgotten
rename is caught before the build runs.

## Creating a release

1. Navigate to the [Generate Prebuilds workflow](https://github.com/digidem/quickbit-native-nodejs-mobile/actions/workflows/prebuilds.yml)
2. Manually dispatch the worflow with the version you want to build, ensuring that "Publish Release" is checked.
3. If the target version isn't already in `patches/`, add `patches/quickbit-native+<version>.patch` first.

## Contributing

We welcome contributions to this repository. If you have an idea for a new feature or have found a bug, please open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
