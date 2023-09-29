[![CI](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml/badge.svg)](https://github.com/alessandrocandolini/telephony-app/actions/workflows/ci.yml)

# telephony-app

## Compile and test 

Assuming a standard Android distribution and `gradle` available, unit tests can by run from command-line using 
```bash
gradle test
```
and to assembly the debug or release artifacts use 
```bash 
gradle assemblyDebug 
gradle assemblyRelease 
```

One way to install the Android distribution is via [nix](https://nixos.org/), for example using an ephemeral `nix-shell` as follows:
```bash
nix-shell -p android.nix
```

## CI/CD 

This project uses github actions to ensure at every commit we run unit tests and generate a downloadable artifact from github releases
