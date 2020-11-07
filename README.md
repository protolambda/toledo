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

TODO: which git branch to run?

Requires a directory structured as:
```shell script
./boot_enr.yaml   # containing all bootnodes as YAML list
./config.yaml  # the chain config file
./deploy_block.txt  # containing just a bare number, of the eth1 block that the contract was deployed at
./deposit_contract.txt  # containing the deposit contract address, with quotes like a json string
```

#### Beacon node

```
  --testnet-dir="some/config_dir/path_here"
```

#### Validator
```
  --testnet-dir="some/config_dir/path_here"
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
 

