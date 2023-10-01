[![CI](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml/badge.svg)](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml)

# telephony-app

## Compile and test 

**Requirements**: Android distribution and the [gradle](https://gradle.org/) build tool. 

**Installation:** the Android distribution and gradle from command-line is via [nix](https://nixos.org/). A "old-style" `shell.nix` (no nix flakes yet!) is provided as part of the project, so assuming nix is available it's enough to open an ephemeral shell from the root of the project: 
```bash
nix-shell 
```
Alternatively, see [official docs](https://developer.android.com/studio) for ways to install it using Android Studio from the UI, or using [homebrew](https://brew.sh/) on MACOS. 


**Unit tests** can by run from command-line using 
```bash
gradle testDebug
```
and to **assembly the debug or release apk** artifacts use 
```bash 
gradle assemblyDebug 
gradle assemblyRelease 
```
To check dependency updates: 
```bash 
gradle dependencyUpdates  
```

Commands can also be executed via nix-shell directly. For example, the CI runs 
```bash
 nix-shell --pure --run "gradle --no-watch-fs --console=plain --no-daemon --info testDebug assembleDebug
```

**Note on gradle and nix**: in this project the gradle dependencies are managed by `gradle` itself, not by nix. This means that gradle will download the artifacts and store them in `~/.gradle/` and those artifacts will *not* be cleaned up automatically when leaving the ephemeral `nix-shell`. 


## CI/CD 

This project uses github actions to run unit tests run and generate an apk artifact at every commit. The generated artifact is downloadable from github releases.

The setup intentionally does not rely on ad-hoc Github Actions to run gradle. Instead, it's completely agnostic about gradle and android, and it uses the same `nix-shell` provided for local development. 
This might be not the most efficient setup, but it ensures that the provided `shell.nix` is always up-to-date. 

To improve build times in the CI, the nix store is cached. A hash of`shell.nix` is used as part of the cache key to ensure changes to `shell.nix` trigger a cache invalidation. Gradle dependencies in `~/.gradle/caches` are also cached, notice however that in this case only the `gradle/libs.versions.toml` file is used to generate the cache key: if dependencies are updated outside of this file (eg, imported directly in some `build.gradle.kts`) the cache key does not change and the cache content is not updated. 


## Template

The original skelethon of this project has been generated usign this template: https://github.com/alessandrocandolini/android-app.g8 
