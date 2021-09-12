# <h1 align="center"> dapptools.rs </h1>

*Rust port of DappTools*

![Github Actions](https://github.com/gakonst/dapptools-rs/workflows/Tests/badge.svg)


## `dapp` example Usage

### Run Solidity tests

```bash
$ cargo r --bin dapp test --contracts ./*.sol
   Compiling dapptools v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 2.17s
     Running `target/debug/dapp test --contracts ./GreetTest.sol`
Running 1 tests for GmTest
[PASS] testGm (gas: 30723)

Running 3 tests for GreeterTest
[PASS] testFailGreeting (gas: 31293)
[PASS] testGreeting (gas: 31222)
[PASS] testIsolation (gas: 5444)
```

### Test output as JSON

In order to compose with other commands, you may print the results as JSON via the `--json` flag

```
./target/release/dapp test --contracts ./*.sol --json
{"GmTest":{"testGm":{"success":true,"gas_used":26123}},"GreeterTest":{"testGreeting":{"success":true,"gas_used":26622},"testFailGreeting":{"success":true,"gas_used":26693},"testIsolation":{"success":true,"gas_used":4144}}}
```

### Build the contracts

You can build the contracts by running, which will by default output the compilation artifacts
of all contracts under `src/` at `out/dapp.sol.json`:

```
./target/release/dapp build
```

You can specify an alternative path for your contracts and libraries with `--remappings`, `--lib-path`
and `--contracts`.

### CLI Help

The CLI options can be seen below. You can fully customize the initial blockchain
context. As an example, if you pass the flag `--block-number`, then the EVM's `NUMBER`
opcode will always return the supplied value. This can be useful for testing.


#### Build

```
cargo r --bin dapp build --help
   Compiling dapptools v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 3.45s
     Running `target/debug/dapp build --help`
dapp-build 0.1.0
build your smart contracts

USAGE:
    dapp build [FLAGS] [OPTIONS] [--] [remappings-env]

FLAGS:
    -h, --help          Prints help information
    -n, --no-compile    skip re-compilation
    -V, --version       Prints version information

OPTIONS:
    -c, --contracts <contracts>         glob path to your smart contracts [default: ./src/**/*.sol]
        --evm-version <evm-version>     choose the evm version [default: berlin]
        --lib-path <lib-path>           the path where your libraries are installed
    -o, --out <out-path>                path to where the contract artifacts are stored [default: ./out/dapp.sol.json]
    -r, --remappings <remappings>...    the remappings

ARGS:
    <remappings-env>     [env: DAPP_REMAPPINGS=]
```

#### Test

```bash
$ cargo r --bin dapp test --help
    Finished dev [unoptimized + debuginfo] target(s) in 0.28s
     Running `target/debug/dapp test --help`
dapp-test 0.1.0

USAGE:
    dapp test [FLAGS] [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -j, --json       print the test results in json format
    -V, --version    Prints version information

OPTIONS:
        --block-coinbase <block-coinbase>
            the block.coinbase value during EVM execution [default: 0x0000000000000000000000000000000000000000]

        --block-difficulty <block-difficulty>    the block.difficulty value during EVM execution [default: 0]
        --block-gas-limit <block-gas-limit>      the block.gaslimit value during EVM execution
        --block-number <block-number>            the block.number value during EVM execution [default: 0]
        --block-timestamp <block-timestamp>      the block.timestamp value during EVM execution [default: 0]
        --chain-id <chain-id>                    the chainid opcode value [default: 1]
    -c, --contracts <contracts>                  glob path to your smart contracts [default: ./src/**/*.sol]
        --gas-limit <gas-limit>                  the block gas limit [default: 25000000]
        --gas-price <gas-price>                  the tx.gasprice value during EVM execution [default: 0]
    -o, --out <out-path>
            path to where the contract artifacts are stored [default: ./out/dapp.sol.json]

        --tx-origin <tx-origin>
            the tx.origin value during EVM execution [default: 0x0000000000000000000000000000000000000000]
```

## Features

* seth
    * [x] `--from-ascii`
    * [x] `--to-checksum-address`
    * [x] `--to-bytes32`
    * [x] `block`
    * [x] `call` (partial)
    * [x] `send` (partial)
* dapp
    * [ ] test
        * [x] simple unit tests
            * [x] Gas costs
            * [x] DappTools style test output
            * [x] JSON test output
            * [x] matching on regex
        * [ ] fuzzing
        * [ ] symbolic execution
        * [ ] coverage
        * [ ] HEVM-style Solidity cheatcodes
        * [ ] structured tracing with abi decoding
        * [ ] per-line gas profiling
        * [ ] forking mode
        * [ ] automatic solc selection
    * [x] build
        * [x] can read DappTools-style .sol.json artifacts
        * [x] remappings
    * [ ] debug

## Development

### Rust Toolchain

We use the stable Rust toolchain. Install by running: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

### Building & testing

```
cargo check
cargo test
cargo doc --open
cargo build [--release]
```

