# Nim Toolchain SDK for Workshop

This repository provides the `nim-toolchain` SDK for [Workshop](https://ubuntu.com/workshop).  

The SDK installs the [Nim](https://github.com/nim-lang/Nim) programming language toolchain for use inside Workshop environments.  
It includes Nim and related command-line tools such as `nimble`, so projects can build, test, and run Nim code without manually setting up the toolchain inside each workshop.

## SDK information

```text
name:       nim-toolchain
publisher:  Seiji Matsuoka (shizuku198411)
license:    MIT
```

Published channels:

```text
CHANNEL           VERSION  BASE          REV
latest/stable     2.2.10   ubuntu@24.04    1
latest/edge       2.2.10   ubuntu@24.04    1
```

## Supported platforms

This SDK currently supports:

```yaml
platforms:
  ubuntu@24.04:amd64:
  ubuntu@24.04:arm64:
```

## Usage

Add the SDK to your `workshop.yaml`:

```yaml
name: nim-dev
base: ubuntu@24.04

sdks:
  - name: nim-toolchain
    channel: latest/stable
```

Launch the workshop:

```bash
workshop launch
```

Then verify Nim inside the workshop:

```bash
workshop exec -- nim --version
workshop exec -- nimble --version
```

You can also compile and run a simple Nim program:

```bash
workshop exec -- bash -lc 'echo "echo \"hello\"" > /tmp/hello.nim && nim c -r /tmp/hello.nim'
```

## What this SDK installs

The SDK installs:

* Nim
* Nimble
* Nimsuggest, when available in the Nim distribution
* Basic build dependencies needed for compiling Nim projects

The SDK downloads the upstream Nim binary archive based on the workshop architecture:

* `linux_x64` for `amd64`
* `linux_arm64` for `arm64`

The installed commands are exposed through `$HOME/.local/bin`, which is already included in the default Workshop user `PATH`.

## Local development

To build and test the SDK locally, use `sdkcraft`.

Pack the SDK:

```bash
sdkcraft clean
sdkcraft pack
```

Try the SDK locally:

```bash
sdkcraft clean
sdkcraft try
```

Then use the local try SDK in a workshop definition:

```yaml
name: nim-local-test
base: ubuntu@24.04

sdks:
  - name: try-nim-toolchain
```

Launch and verify:

```bash
workshop launch --verbose --wait-on-error
workshop exec -- nim --version
workshop exec -- nimble --version
```

## Repository layout

```text
.
├── hooks
│   ├── setup-base
│   └── setup-project
├── sdkcraft.yaml
├── tests
│   ├── main
│   │   └── launch
│   │       └── task.yaml
│   └── spread.yaml
└── example
    └── workshop.yaml
```

## License

This SDK is licensed under the MIT License.
Nim itself is also distributed under the MIT License.
