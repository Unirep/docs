---
description: User spends it own reputation to perform actions
---

# Spend Reputation

## `spendReputation`

```
npx ts-node cli/index.ts spendReputation 
                  [-h] 
                  [-e ETH_PROVIDER] 
                  [-ep EPOCH] 
                  -p PUBLIC_SIGNALS 
                  -pf PROOF 
                  [-b START_BLOCK] 
                  -x CONTRACT 
                  (-dp | -d ETH_PRIVKEY)
```

* After user [generate a reputation proof with nullifiers](https://github.com/vivianjeng/UniRep/blob/git-book/cli/reputation-proof/README.md#genreputationproof), the attester can **spend** the reputation. In other words, the attester will send the negative reputation to the epoch key of the reputation proof, then the (`positive_reputation` - `negative_reputation`) reputation from the attester will decrease.

### Options

```
  -e ETH_PROVIDER, --eth-provider ETH_PROVIDER
                        A connection string to an Ethereum provider. Default: http://localhost:8545
  -ep EPOCH, --epoch EPOCH
                        The latest epoch user transitioned to. Default: current epoch
  -p PUBLIC_SIGNALS, --public-signals PUBLIC_SIGNALS
                        The snark public signals of the user's epoch key
  -pf PROOF, --proof PROOF
                        The snark proof of the user's epoch key
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
