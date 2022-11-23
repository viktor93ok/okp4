[![okp4 github banner](https://raw.githubusercontent.com/okp4/okp4d/main/etc/okp4-banner.webp)](https://okp4.network)



# 𝒐𝒌𝒑4𝒅

[![lint](https://img.shields.io/github/workflow/status/okp4/okp4d/Lint?label=lint&style=for-the-badge&logo=github)](https://github.com/okp4/okp4d/actions/workflows/lint.yml)
[![build](https://img.shields.io/github/workflow/status/okp4/okp4d/Build?label=build&style=for-the-badge&logo=github)](https://github.com/okp4/okp4d/actions/workflows/build.yml)
[![test](https://img.shields.io/github/workflow/status/okp4/okp4d/Test?label=test&style=for-the-badge&logo=github)](https://github.com/okp4/okp4d/actions/workflows/test.yml)
[![discord](https://img.shields.io/discord/946759919678406696.svg?label=discord&logo=discord&logoColor=white&style=for-the-badge)](https://discord.gg/okp4)
[![conventional commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg?style=for-the-badge&logo=conventionalcommits)](https://conventionalcommits.org)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg?style=for-the-badge)](https://github.com/okp4/.github/blob/main/CODE_OF_CONDUCT.md)
[![license](https://img.shields.io/github/license/okp4/okp4d.svg?label=License&style=for-the-badge)](https://opensource.org/licenses/Apache-2.0)

> `OKP4` is a public [dPoS](https://en.bitcoinwiki.org/wiki/DPoS) layer 1 specifically designed to enable communities to trustlessly share data, algorithms and resources to build the Dataverse - An open world where everybody can create or participate in custom ecosystems (with common governance mechanisms, sharing rules, business models...) to build a new generation of dApps way beyond Decentralized Finance.

## The protocol

`okp4d` is the node of the [OKP4](https://okp4.network) network built on the [Cosmos SDK] 💫 & [Tendermint] consensus, and designed to become a hub of incentivized data providers, developers, data scientists & users collaborating to generate value from data and algorithms.

Read more in the [introduction blog post](https://blog.okp4.network/what-is-okp4-b6bb058ae221). For a high-level overview of the OKP4 protocol and network economics, check out the [white paper](https://docs.okp4.network/docs/whitepaper/abstract).

## Want to become a validator?

Validators are responsible for securing the okp4 network. Validator responsibilities include maintaining a functional [node](https://docs.okp4.network/docs/nodes/run-node) with constant uptime and providing a sufficient amount of $KNOW as stake. In exchange for this service, validators receive block rewards and transaction fees.

Want to become a validator? 👉 [Checkout the documentation!](https://docs.okp4.network/docs/nodes/introduction)

Looking for a network to join ? 👉 [Checkout the networks!](https://github.com/okp4/networks)

## Supported platforms

The `okp4d` blockchain currently supports the following builds:

| **Platform** | **Arch** |       **Status**       |
|--------------|----------|:----------------------:|
| Darwin       | amd64    |           ✅            |
| Darwin       | arm64    |           ✅            |
| Linux        | amd64    |           ✅            |
| Linux        | arm64    |           ✅            |
| Windows      | amd64    | ️🚫<br/> Not supported |

> Note: as the blockchain depends on [CosmWasm/wasmvm](https://github.com/CosmWasm/wasmvm), we only support the targets
> supported by this project.

## Releases

All releases can be found [here](https://github.com/okp4/okp4d/releases).

`okp4d` follows the [Semantic Versioning 2.0.0](https://semver.org/) to determine when and how the version changes, and
we also apply the philosophical principles of [release early - release often](https://en.wikipedia.org/wiki/Release_early,_release_often).

## Docker image

For a quick start, a docker image is officially available on [Docker hub](https://hub.docker.com/r/okp4/okp4d).

```bash
docker pull okp4/okp4d:latest
```

### Get the documentation

```bash
docker run okp4/okp4d:latest --help
```

### Query a running network

Example:

```bash
API_URL=https://api.devnet.okp4.network:443/rpc
WALLET=okp41pmkq300lrngpkeprygfrtag0xpgp9z92c7eskm

docker run okp4/okp4d:latest query bank balances $WALLET --chain-id okp4-devnet-1 --node $API_URL
 ```

### Create a wallet

```bash
docker run -v $(pwd)/home:/home okp4/okp4d:latest keys add my-wallet --keyring-backend test --home /home 
```

### Start a node

Everything you need to start a node and more is explained here: <https://docs.okp4.network/docs/nodes/run-node>

```bash
MONIKER=node-in-my-name
CHAIN_ID=localnet-okp4-1

docker run -v $(pwd)/home:/home okp4/okp4d:latest init $MONIKER --chain-id $CHAIN_ID --home /home 
```

This will create a home folder, you can then update the `config/genesis.json` with one of this ones : <https://github.com/okp4/okp4d/tree/main/chains/>

#### Join a running network

Set `persistent_peers` in `config/config.toml` file.

#### Then start the node

```bash
docker run -v $(pwd)/home:/home okp4/okp4d:latest start --home /home
```

## Developing & contributing

`okp4d` is written in [Go] and built using [Cosmos SDK]. A number of smart contracts are also deployed on the
OKP4 blockchain and hosted in the [okp4/contracts](https://github.com/okp4/contracts) project.

### Prerequisites

- install [Go] `1.19+` following instructions from the [official Go documentation](https://golang.org/doc/install);
- use [gofumpt](https://github.com/mvdan/gofumpt) as formatter. You can integrate it in your favorite IDE following these [instructions](https://github.com/mvdan/gofumpt#installation) or invoke the makefile `make format-go`;
- verify that [Docker] is properly installed and if not, follow the [instructions](https://docs.docker.com) for your environment;
- verify that [`make`](https://fr.wikipedia.org/wiki/Make) is properly installed if you intend to use the provided `Makefile`.

### Makefile

The project comes with a convenient `Makefile` that helps you to build, install, lint and test the project.

```text
$ make help

Usage:
  make <target>

Targets:
  Lint:
    lint                Lint all available linters
    lint-go             Lint go source code
    lint-proto          Lint proto files
  Format:
    format              Run all available formatters
    format-go           Format go files
  Build:
    build               Build all available artefacts (executable, docker image, etc.)
    build-go            Build node executable for the current environment (default build)
    build-go-all        Build node executables for all available environments
  Install:
    install             Install node executable
  Test:
    test                Pass all the tests
    test-go             Pass the test for the go source code
  Clean:
    clean               Remove all the files from the target folder
  Proto:
    proto-format        Format Protobuf files
    proto-build         Build all Protobuf files
    proto-gen           Generate all the code from the Protobuf files
  Release:
    release-assets      Generate release assets
  Help:
    help                Show this help.

This Makefile depends on docker. To install it, please follow the instructions:
- for macOS: https://docs.docker.com/docker-for-mac/install/
- for Windows: https://docs.docker.com/docker-for-windows/install/
- for Linux: https://docs.docker.com/engine/install/
```

### Build

To build the `okp4d` node, invoke the goal `build` of the `Makefile`:

```sh
make build
```

The binary will be generated under the folder `target/dist`.

## Bug reports & feature requests

If you notice anything not behaving how you expected, if you would like to make a suggestion or would like
to request a new feature, please open a [**new issue**](https://github.com/okp4/okp4d/issues/new/choose). We appreciate any help
you're willing to give!

> Don't hesitate to ask if you are having trouble setting up your project repository, creating your first branch or
> configuring your development environment. Mentors and maintainers are here to help!

## Community

The [**OKP4 Discord Server**](https://discord.gg/okp4) is our primary chat channel for the open-source community,
software developers and node operators.

Please reach out to us and say hi 👋, we're happy to help there.

[Cosmos SDK]: https://v1.cosmos.network/sdk
[Docker]: https://www.docker.com/
[Go]: https://go.dev
[Tendermint]: https://tendermint.com/
