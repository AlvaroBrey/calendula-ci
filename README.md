# calendula-ci

This is a simple Docker image for testing Android apps.

## Usage

The image has its entrypoint in the home directory of the only user, `build`.
In this directory you will find the following:

* `android-sdk-linux/`: Folder with the SDK, tools and components according to the image tag.
* `android-wait-for-emulator`: Script for waiting until a emulator has finished booting up.

There is also a preconfigured emulator named `test`. Details of its configuration can be looked up in the Dockerfile.

Relevant environment variables are already configured.

## Example usage

```bash
# Clone your project
build~$ git clone my-project && cd my-project/
# Build sources before starting the emulator for better performance
build~$ ./gradlew assembleDebug
# Start the emulator from its folder to prevend dependency path issues
build~$ ( cd ~/android-sdk-linux/emulator && ./emulator -avd test -no-window -no-audio & )
# Wait for the emulator to start
build~$ ./android-wait-for-emulator
# Unlock the screen
build~$ adb shell input keyevent 82
# Run connected tests
build~$ ./gradlew connectedDebugAndroidTest
```

## Tags
Build tags follow the form `calendula-ci:compile_COMPILESDK-test_TESTSDK-tools_TOOLSVERSION`, where:

* `COMPILESDK` is the Android API level used for building, e.g. `26`.
* `TESTSDK` is the Android API level of the included emulator, e.g. `25`.
* `TOOLSVERSION` is the version of the installed `build-tools`, e.g. `25.0.3`.