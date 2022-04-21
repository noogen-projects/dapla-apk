# Laplace mobile application

## Install the Android NDK and SDK

1. Install Java Runtime Environment, for example `openjdk-jre`.
2. Download [Android SDK Command line tools](https://developer.android.com/studio#command-tools) for your platform.
3. Unpack `cmdline-tools` and put it to the `android-sdk` directory. WARNING! Make location to the content of the tools
archive as `android-sdk/cmdline-tools/latest/`.
4. Setup `ANDROID_SDK_ROOT` env variable to the `android-sdk` location and add the `android-sdk/cmdline-tools/latest/bin/`
to the `PATH` env variable.
5. Install SDK components:
```
$ sdkmanager "ndk;23.1.7779620" "platforms;android-23" "platform-tools" "build-tools;32.0.0"
```
6. Setup `ANDROID_NDK_ROOT` env variable to the `$ANDROID_SDK_ROOT/ndk/23.1.7779620/` location.
7. Add necessary build targets:
```
$ rustup target add --toolchain stable aarch64-linux-android
$ rustup target add --toolchain stable i686-linux-android
```
8. Install `cargo-apk` tool and build project:
```
$ cargo install cargo-apk
$ cargo +stable apk build --lib --release
```
WARNING! If you get an error like this:
```
  = note: ld: error: unable to find library -lgcc
          clang-12: error: linker command failed with exit code 1 (use -v to see invocation)
```
Then [patch](https://github.com/rust-windowing/android-ndk-rs/issues/265) your toolchain libraries with:
```shell
$ echo 'INPUT(-lunwind)' > ~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/aarch64-linux-android/lib/libgcc.a
```