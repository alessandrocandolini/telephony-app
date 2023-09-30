[![CI](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml/badge.svg)](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml)

# telephony-app

## Compile and test 

Assuming a standard Android distribution and the `gradle` build tool are available, unit tests can by run from command-line using 
```bash
gradle testDebug
```
and to assembly the debug or release artifacts use 
```bash 
gradle assemblyDebug 
gradle assemblyRelease 
```

One way to install the Android distribution is via [nix](https://nixos.org/), for example using an ephemeral `nix-shell` as follows:
```bash
nix-shell 
```
A "old-style" `shell.nix` has been provided in the repository (no nix flakes yet!) 

You can also use nix to execute the commands directly. For example, the CI runs 
```bash
nix-shell --pure --run "gradle --console=plain --no-daemon --info testDebug assembleDebug"
```

Notice: project dependencies are managed by `gradle` outside nix (ie, even if using nix, gradle will download the artifacts and store them in `~/.gradle/`) 



## CI/CD 

This project uses github actions to ensure at every commit we run unit tests and generate an apk artifact downloadable from github releases.

The CI intentionally does not rely on ad-hoc Github Actions to run gradle. Instead, it uses the nix shell provided for local development. 
Nix store is cached. We also attempt at caching gradle dependencies, but for that `**/build.gradle.kts` files are used to generate the cache key: if you update dependencies stored in other files those will not automatically be cached). 
