[![CI](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml/badge.svg)](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml)

# telephony-app

## Compile and test 

Requirements: Android distribution and the [gradle](https://gradle.org/) build tool. 

A possible way to install the Android distribution and gradle is via [nix](https://nixos.org/). A "old-style" `shell.nix` (no nix flakes yet!) is provided as part of the project, so assuming nix is available it's enough to open an ephemeral shell from the root of the project: 
```bash
nix-shell 
```
Alternatively, see [official docs](https://developer.android.com/studio)


Unit tests can by run from command-line using 
```bash
gradle testDebug
```
and to assembly the debug or release apk artifacts use 
```bash 
gradle assemblyDebug 
gradle assemblyRelease 
```

Commands can also be executed via nix-shell directly. For example, the CI runs 
```bash
 nix-shell --pure --run "gradle --no-watch-fs --console=plain --no-daemon --info testDebug assembleDebug
```

Notice: project dependencies are managed by `gradle` outside nix, so gradle will download the artifacts and store them in `~/.gradle/` and those artifacts will stay after leaving the ephemeral `nix-shell`. 


## CI/CD 

This project uses github actions to run unit tests run and generate an apk artifact at every commit. The artifactu is downloadable from github releases.

The setup intentionally does not rely on ad-hoc Github Actions to run gradle. Instead, it uses the same `nix-shell` provided for local development. 
Nix store is cached, and a hash of`shell.nix` is used as part of the cache key to ensure changes to `shell.nix` result in cache invalidation. 
Gradle dependencies in `~/.gradle/caches` are also cached, notice however that in this the `toml` file is used to generate the cache key: if dependencies are updated outside of this file (eg, imported directly in some `build.gradle.kts` file) the cache key (and thus the cache) will not be updated
