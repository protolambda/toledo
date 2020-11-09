# Toledo testnet

![](Toledo_metro_station.jpg)

*Toledo metro station, Naples, italy. [Photo: BaileyParsonsNL, CC SA 4.0](https://commons.wikimedia.org/wiki/File:Toledo_metro_station_(J).jpg)*

A temporary dev-net to test the first v1.0.0 client candidates.

The genesis validator set is split in 8 tranches of 2048 validators.
Contact @protolambda if you are a developer that likes to run validators.
Client dev teams will be provided 1 tranche each.

## Config

Chain config file in **[`config.yaml`](./config.yaml)**

Bootnodes in [`bootnodes.txt`](./bootnodes.txt)

Differences with regular mainnet config:
```yaml
deposit_contract_address: "0x47709dC7a8c18688a1f051761fc34ac253970bC0"
deposit_contract_deploy_block_nr: 3702432
deposit_contract_deploy_tx: "0x2b093a40f4e1e8ede6fc049abf4ae87cb424ffacca2e759aa05cfd1d3881ec99"
eth2_fork_version: "0x00701ED0"
MIN_GENESIS_TIME: 1605009600  # Tuesday, November 10, 2020 12:00:00 PM
GENESIS_DELAY: 86400  # 1 day
```

[`0x47709dC7a8c18688a1f051761fc34ac253970bC0` on etherscan](https://goerli.etherscan.io/address/0x47709dc7a8c18688a1f051761fc34ac253970bc0)

Misc. data:
```yaml
# Fork digest, mixed with zeroed genesis validators root: 
bootnode_fork_digest: 0xb812f7ec
```

## Eth2stats

Eth2stats dashboard: https://toledo.eth2.wtf

Example to participate:
```
docker run -d --restart always --network="host" \
--name eth2stats-client \
-v ~/.eth2stats/data:/data \
alethio/eth2stats-client:latest run \
--eth2stats.node-name="example node" \
--data.folder="/data" \
--eth2stats.addr="grpc.toledo.eth2.wtf:8080" --eth2stats.tls=false \
--beacon.type="v1" \
--beacon.addr="http://localhost:5052" \
--beacon.metrics-addr="http://localhost:5054/metrics"
```

More info about eth2stats in the [eth2stats-client repo](https://github.com/Alethio/eth2stats-client/blob/master/README.md)


## Using your testnet mnemonic

For client devs:
1. get a pre-loaded mnemonic from @protolambda to participate.
2. Load the first 2048 accounts, output into keystores or other format
3. Use keystores to participate!

*__Not__ recommended for mainnet*, but easy to use here: https://github.com/protolambda/eth2-val-tools

Example to build keystores (and random keystore secrets + config files) for a range of accounts,
 along with an json file that tracks assignments:

```shell script
mkdir -p example_output/hosts

# assign 100 random accounts to "foobar" host
eth2-val-tools assign \
  --assignments="example_output/assignments.json" \
  --hostname="foobar" \
  --out-loc="example_output/hosts/foobar" \
  --source-mnemonic="$SOURCE_WALLET_VALIDATORS_MNEMONIC" \
  --source-min=0 \
  --source-max=2048 \
  --count=100 \
  --config-base-path="/data" \
  --key-man-loc="/data/wallets" \
  --wallet-name="foobar"

# and assign another 100 to "pineapple" host (no overlaps, previous assignments are loaded from json file)
eth2-val-tools assign \
  --assignments="example_output/assignments.json" \
  --hostname="pineapple" \
  --out-loc="example_output/hosts/pineapple" \
  --source-mnemonic="$SOURCE_WALLET_VALIDATORS_MNEMONIC" \
  --source-min=0 \
  --source-max=2048 \
  --count=100 \
  --config-base-path="/data" \
  --key-man-loc="/data/wallets" \
  --wallet-name="pineapple"
```

Or alternatively, use your own tooling.
The accounts loaded with deposits are `[f"m/12381/3600/{i}/0/0" for i in range(2048)]`
The latest Ethdo libraries and Herumi BLS were used to derive private keys from the mnemonic (not sure if updated to new mnemonic derivation yet).


## Client configuration

Work in progress. Client configurations are unconfirmed.
Previously there were subtle differences in config formatting.
These are being resolved.


### Prysm

Run the `v1.0.0-rc.0` branch

#### Beacon node

```
  --chain-config-file="config.yaml"
  --deposit-contract="0x47709dC7a8c18688a1f051761fc34ac253970bC0"
  --contract-deployment-block="3702432"
```

#### Validator

```
  --chain-config-file="config.yaml"
```


### Lighthouse

Use the [`toledo`](https://github.com/sigp/lighthouse/pull/1874) branch (until that PR is merged into master).

#### Beacon node

```
  --testnet toldeo
```

#### Validator
```
  --testnet toldeo
```

### Teku

TODO: which git branch to run?

#### Beacon node & validator

```
  --network="config.yaml"
  --eth1-deposit-contract-address="0x47709dC7a8c18688a1f051761fc34ac253970bC0"
```

### Nimbus

TODO: which git branch to run?

#### Beacon node

Prepare a binary with insecure/experimental features enabled:
```shell script
make -j8 LOG_LEVEL="TRACE" NIMFLAGS="-d:insecure" beacon_node
```

Requires a special json config file.
```
  --network="nimbus_config.json"
```

Config file looks like [`nimbus_config.json`](./nimbus_config.json)

#### Validator

No configuration necessary, fork version etc. are loaded from the beacon node.

Prepare a binary with insecure/experimental features enabled:
```shell script
make -j8 LOG_LEVEL="TRACE" NIMFLAGS="-d:insecure" validator_client
```

## Q & A

### How do I participate?

This is a dev-net. A more public testnet will launch about a week later.
If you reach out we can give you instructions to get started as non-dev.
You can join the network and run a beacon-node.
The deposit contract is permissioned, deposits won't work as regular user.

### Why?

Time to get clients ready for v1.0.0! Starting relatively small, and then bigger!

### What is that image?

Yes, Toledo is also a city and municipality in Spain.
This is another Toledo: [Toledo Via in Naples, Italy](https://en.wikipedia.org/wiki/Via_Toledo) and [its metro station](https://en.wikipedia.org/wiki/Toledo_(Naples_Metro)) filled with art.
 

