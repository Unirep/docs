---
description: Epoch transition to the next epoch
---

# Epoch transition

## `epochTransition`

```
npx ts-node cli/index.ts epochTransition
                  [-h] 
                  [-e ETH_PROVIDER] 
                  [-t] 
                  -x CONTRACT 
                  (-dp | -d ETH_PRIVKEY)
```

* In a Unirep system, each epoch transition is called only once.
* After calling an epoch transition, all epoch keys in this epoch are sealed and they cannot be attested anymore.
* After an epoch transition, the current epoch is increased by 1.

### Options

```
  -e ETH_PROVIDER, --eth-provider ETH_PROVIDER
                        A connection string to an Ethereum provider. Default: http://localhost:8545
  -t, --is-test         Indicate if the provider is a testing environment
  -x CONTRACT, --contract CONTRACT
                        The Unirep contract address
  -dp, --prompt-for-eth-privkey
                        Whether to prompt for the user's Ethereum private key and ignore -d / --eth-privkey
  -d ETH_PRIVKEY, --eth-privkey ETH_PRIVKEY
                        The deployer's Ethereum private key
```

#### Options inherited from parent commands <a href="#options-inherited-from-parent-commands" id="options-inherited-from-parent-commands"></a>

```
  -h, --help            Show this help message and exit.
```
