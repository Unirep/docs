---
description: Attester gives reputation to an epoch key
---

# Attestation

## `attest`

```
npx ts-node cli/index.ts attest
                  [-h] 
                  [-e ETH_PROVIDER] 
                  -i PROOF_INDEX 
                  -epk EPOCH_KEY 
                  [-pr POS_REP] 
                  [-nr NEG_REP] 
                  [-gf GRAFFITI] 
                  [-s SIGN_UP] 
                  -x CONTRACT
                  (-dp | -d ETH_PRIVKEY)
```

* `-d` is the attester's private key here,
* `-epk` is the epoch key of the receiver,
* `-i` is the proof index of the epoch key,
* `-pr` (optional) is the positive reputation given to the user,
* `-nr` (optional) is the negative reputation given to the user,
* `-gf` (optional) is the graffiti for the reputation given to the user,
* `-s` (optional) is the sign up flag to give to the user to indicate the attester authenticates the user's membership.

### Options

```
  -e ETH_PROVIDER, --eth-provider ETH_PROVIDER
                        A connection string to an Ethereum provider. Default: http://localhost:8545
  -i PROOF_INDEX, --proof-index PROOF_INDEX
                        The proof index of the user's epoch key
  -epk EPOCH_KEY, --epoch-key EPOCH_KEY
                        The user's epoch key to attest to (in hex representation)
  -pr POS_REP, --pos-rep POS_REP
                        Score of positive reputation to give to the user
  -nr NEG_REP, --neg-rep NEG_REP
                        Score of negative reputation to give to the user
  -gf GRAFFITI, --graffiti GRAFFITI
                        Graffiti for the reputation given to the user (in hex representation)
  -s SIGN_UP, --sign-up SIGN_UP
                        Whether to set sign up flag to the user
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
