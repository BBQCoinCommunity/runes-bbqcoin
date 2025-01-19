# Runes

A minter and protocol for runes on BBQCoin.

You can find general information about Runes [here](./RUNES.md).

## ⚠️⚠️⚠️ Important ⚠️⚠️⚠️

Use this wallet for runes only! Always mint from this wallet to a different address. This wallet is not meant for storing funds or runes.

## Prerequisites

To use this, you'll need to use your console/terminal and install Node.js on your computer. So please ensure, that you have your

### Install NodeJS

Please head over to [https://nodejs.org/en/download](https://nodejs.org/en/download) and follow the installation instructions.

### Launch your own RPC

In order to inscribe, you will need to have access to a BBQCoin RPC. For example: [https://getblock.io/](https://getblock.io/) provides a service to get access to an RPC.
You will need that for the configuration.

## Setup

### git clone and install

Install by git clone (requires git and node on your computer)

#### git clone

```
git clone https://github.com/BBQCoinCommunity/runes-bbqcoin.git
```

```
cd <path to your download / installation>
npm install
```

After all dependencies are solved, you can configure the environment:

### Configure environment

Copy a `.env.example` to `.env` and add your node information. Here are also some recommended settings:

```
PROTOCOL_IDENTIFIER=B
NODE_RPC_URL=http://<ip>:<port>
NODE_RPC_USER=<username>
NODE_RPC_PASS=<password>
TESTNET=false
FEE_PER_KB=100000000
ORD=https://bbqnals.bbqcoin.link/
```

## Funding

Generate a new `.wallet.json` file:

```
node runes.js wallet new
```

Then send BQC to the address displayed. Once sent, sync your wallet:

```
node runes.js wallet sync
```

If you are minting a lot, you can split up your UTXOs:

```
node runes.js wallet split <splits>
```

When you are done minting, send the funds back:

```
node runes.js wallet send <address> <amount>
```

## Runes

Deploy a rune:

```
node runes.js deployOpenRune 'RANDOM•RUNE•NAME' <symbol> <limit-per-mint> <decimals> <max-nr-of-mints> <mint-absolute-start-block-height> <mint-absolute-stop-block-height> <mint-relative-start-block-height> <mint-relative-end-block-height> <amount-premined-to-deployer> <opt-in-for-future-protocol-changes> <minting-allowed>
```

Parameters Explained:
- **Rune Name:** The name of the rune. Use `•` to add spacers for uniqueness.
- **Symbol:** Single-character symbol or emoji representing the rune.
- **Limit:** Maximum amount that can be minted per transaction.
- **Divisibility:** Number of decimals for the rune (e.g., `8` for BTC-like divisibility).
- **Height Start / End:** Absolute block height defining when minting starts and ends.
- **Offset Start / End:** Relative block height defining when minting starts and ends (used for temporary minting periods).
- **Premine:** Amount of the rune allocated to the deployer at the time of deployment.
- **Turbo:** Boolean flag to opt into future protocol changes (`true` or `false`).
- **Open Mint:** Boolean flag to allow minting (`true` or `false`).

Notes:
- Use `null` for parameters that are not applicable.
- If both absolute and relative block heights are defined, the highest `start` and lowest `end` values are used.

Examples

Example 1
Deploy a rune that can be minted for 100 blocks, with a limit of `100000000`, 8 decimals, no mint limit, symbol `R` (emojis also work), and premining `1R` for the deployer during deployment. The `false` means the rune will not opt-in for future protocol changes. The `true` means mints are open.

```
node runes.js deployOpenRune 'RANDOM•RUNE•NAME' 'R' 100000000 8 null null null null 100 100000000 false true
```

Example 2
Deploy a rune named `BQC•GO•TO•THE•MOON` with the emoji `🔥` as the symbol, divisibility of `0` (non-fractional amounts), a per-mint limit of `1000`, and an overall cap of `21000` mints. Minting is open (`true`), and there is no premine (`0`).

```
node runes.js deployOpenRune 'BQC•GO•TO•THE•MOON' '🔥' 1000 0 21000 null null null null 0 false true
```

Mint a rune:

```
node runes.js mintRune <id> <amount> <to>
```

Example:

```
node runes.js mintRune '537304:1' 1 bc3L5KvP1Y5M9ubp68GMRSFLoyvi1z22Fm
```

Mass mint a rune:

```
node runes.js batchMintRune <id> <amount> <number-of-mints> <to>
```

Example (this will do 100x mints):

```
node runes.js batchMintRune '537304:1' 1000 100 bc3L5KvP1Y5M9ubp68GMRSFLoyvi1z22Fm
```

Get the ID from: https://bbqnals.bbqcoin.link/runes

Print the balance of an address:

```
node runes.js printRuneBalance <rune-name> <address>
```

Example:

```
node runes.js printRuneBalance BQC•GO•TO•THE•MOON bc3L5KvP1Y5M9ubp68GMRSFLoyvi1z22Fm
```

Split runes from one output to many:

```
node runes.js sendRuneMulti <txhash> <vout> <rune> <decimals> <amounts> <addresses>
```

Example:

```
node runes.js sendRuneMulti 15a0d646c03e52c3bf66d67c910caa9aa30e40ecf27f495f1b9c307a4ac09c2e 1 BQC•GO•TO•THE•MOON 0 2,3 bc3L5KvP1Y5M9ubp68GMRSFLoyvi1z22Fm,bLkzX2EvKi6xCKg4mD9vWX1qDNGh6v49zQ
```

Combine runes from multiple outputs to one:

```
node runes.js sendRunesNoProtocol <address> <utxo-amount> <rune>
```

Example:

```
node runes.js sendRunesNoProtocol bc3L5KvP1Y5M9ubp68GMRSFLoyvi1z22Fm 10 BQC•GO•TO•THE•MOON
```

## FAQ

### I'm getting ECONNREFUSED errors when minting

There's a problem with the node connection. Your `bbqcoin.conf` file should look something like:

```
rpcuser=bbq
rpcpassword=coin
rpcport=59332
server=1
```

Make sure `port` is not set to the same number as `rpcport`. Also make sure `rpcauth` is not set.
