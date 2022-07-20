---
description: Users should call user state transition to receive reputation
---

# User state transition

## `userStateTransition`

```
npx ts-node cli/index.ts userStateTransition
                  [-h] 
                  [-e ETH_PROVIDER] 
                  -id IDENTITY 
                  [-b START_BLOCK] 
                  -x CONTRACT 
                  (-dp | -d ETH_PRIVKEY)
```

* User provide the semaphore identity to generate a user state transition proof.
* User state transition proof includes:
  1. Whether the user has already signed up.
  2. Which epoch keys the user has.
  3. Whether the receive reputation all matches the hash-chain retrieves from the Unirep contract.
  4. Whether user state tree root is correctly transitioned.
  5. It computes a new GST leaf in the current epoch.
  6. It computes the epoch key nullifiers.

### Options

```
  -e ETH_PROVIDER, --eth-provider ETH_PROVIDER
                        A connection string to an Ethereum provider. Default: http://localhost:8545
  -id IDENTITY, --identity IDENTITY
                        The (serialized) user's identity
  -b START_BLOCK, --start-block START_BLOCK
                        The block the Unirep contract is deployed. Default: 0
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
