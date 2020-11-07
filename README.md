# Toledo testnet

![](Toledo_metro_station.jpg)

*Toledo metro station, Naples, italy. [Photo: BaileyParsonsNL, CC SA 4.0](https://commons.wikimedia.org/wiki/File:Toledo_metro_station_(J).jpg)*

A temporary dev-net to test the first v1.0.0 client candidates.

Chain config file in **[`config.yaml`](./config.yaml)**

Bootnodes in [`bootnodes.txt`](./bootnodes.txt)

Differences with regular mainnet config:
```yaml
deposit_contract_address: "0x47709dC7a8c18688a1f051761fc34ac253970bC0"
deposit_contract_deploy_block_nr: 3702432
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

The genesis validator set is split in 8 tranches of 2048 validators.
Contact @protolambda if you are a developer that likes to run validators.
Client dev teams will be provided 1 tranche each.

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
 

