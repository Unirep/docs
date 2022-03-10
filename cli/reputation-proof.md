---
description: User can generate reputation proof to claim how much reputation he has
---

# Reputation Proof

## `genReputationProof`

```
npx ts-node cli/index.ts genReputationProof
                  [-h] 
                  [-e ETH_PROVIDER] 
                  -id IDENTITY 
                  -n EPOCH_KEY_NONCE 
                  -a ATTESTER_ID 
                  [-r REPUTATION_NULLIFIER] 
                  [-mr MIN_REP]
                  [-gp GRAFFITI_PREIMAGE] 
                  [-b START_BLOCK] 
                  -x CONTRACT
```

* After user state transition, the new GST leaf contains the information of user state tree root.
* User can prove that how much reputation he receives from a certain attester.
* The proof includes:
  1. Check if user exists in the Global State Tree
  2. Check if the reputation given by the attester is in the user state tree
  3. Check conditions on reputations and the pre-image of graffiti
* Reputation and base64url encoded reputation proof will be printed
* A string with `Unirep.reputation.proof.` prefix is the proof of this reputation
* A string with `Unirep.reputation.publicSignals.` prefix it the public signals of this proof The public signals includes
  * reputation nullifiers (default nullifier: 0)
  * minimum reputation score the user wants to prove (default: 0 if the user don't want to prove the score)
  * epoch key
  * current epoch
  * global state tree root
  * whether the user wants to prove the pre-image of the graffiti (boolean: 0 or 1)
  * the pre-image of the graffiti

### Options

```
  -e ETH_PROVIDER, --eth-provider ETH_PROVIDER
                        A connection string to an Ethereum provider. Default: http://localhost:8545
  -id IDENTITY, --identity IDENTITY
                        The (serialized) user's identity
  -n EPOCH_KEY_NONCE, --epoch-key-nonce EPOCH_KEY_NONCE
                        The epoch key nonce
  -a ATTESTER_ID, --attester-id ATTESTER_ID
                        The attester id (in hex representation)
  -r REPUTATION_NULLIFIER, --reputation-nullifier REPUTATION_NULLIFIER
                        The number of reputation nullifiers to prove
  -mr MIN_REP, --min-rep MIN_REP
                        The minimum positive score minus negative score the attester given to the user
  -gp GRAFFITI_PREIMAGE, --graffiti-preimage GRAFFITI_PREIMAGE
                        The pre-image of the graffiti for the reputation the attester given to the user (in hex representation)
  -b START_BLOCK, --start-block START_BLOCK
                        The block the Unirep contract is deployed. Default: 0
  -x CONTRACT, --contract CONTRACT
                        The Unirep contract address
```

#### Options inherited from parent commands <a href="#options-inherited-from-parent-commands" id="options-inherited-from-parent-commands"></a>

```
  -h, --help            Show this help message and exit.
```

## `verifyReputationProof`

```
npx ts-node cli/index.ts verifyReputationProof
                  [-h] 
                  [-e ETH_PROVIDER] 
                  [-ep EPOCH] 
                  -p PUBLIC_SIGNALS 
                  -pf PROOF 
                  [-b START_BLOCK] 
                  -x CONTRACT
```

* This command will help other users with reputation proof with `Unirep.reputation.proof` prefix and it public signals with `Unirep.reputation.publicSignals` prefix to call the Unirep smart contract to verify the proof.

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
```

#### Options inherited from parent commands <a href="#options-inherited-from-parent-commands" id="options-inherited-from-parent-commands"></a>

```
  -h, --help            Show this help message and exit.
```
