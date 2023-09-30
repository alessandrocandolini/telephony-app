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
nix-shell 
```
(a "old-style" `shell.nix` has been provided; no nix flakes yet!) 

You can also use nix to execute the commands directly:
```bash
nix-shell --run "gradle test"
```

Notice: the nix shell provides `gradle` and android installation, it does not handle jvm dependencies managed via gradle. Those will be managed directly by gradle. 



## CI/CD 

This project uses github actions to ensure at every commit we run unit tests and generate a downloadable artifact from github releases

The CI uses the nix shell provided. An alternative possibility was to use specific gradle and android github actions, but using nix provides a more general-purpose, technology-agnostic approach, and it allows to indirectly verify that `shell.nix` is up-to-date
