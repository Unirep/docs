# Circuits

Below are descriptions of the proofs used in the UniRep protocol. If an input signal is not marked `(public)` it is private.

## Signup Proof

The signup proof outputs a state tree leaf and an identity commitment for the user. The state tree leaf will have zero values for positive and negative reputation.

Inputs:
- `attesterId` (public)
- `epoch` (public)
- `identityNullifier`
- `identityTrapdoor`

Outputs:
- `identityCommitment`
- `stateLeaf`

## Epoch Key Proof

The epoch key proof allows a user to prove control of an epoch key in a certain epoch. This proof calculates two things: merkle inclusion of a state leaf against the current state root, and an epoch key.

Inputs:
- `epoch` (public)
- `attesterId` (public)
- `posRep`
- `negRep`
- `graffiti`
- `timestamp`
- `stateTreeIndexes[STATE_TREE_DEPTH]`
- `stateTreeElements[STATE_TREE_DEPTH]`
- `identityNullifier`

Outputs:
- `epochKey`
- `stateTreeRoot`

## User State Transition Proof

The user state transition proof allows a user to prove how much reputation they have at the end of an epoch and output a new state tree leaf. The proof calculates an inclusion proof for the state tree, and for each epoch key nonce an inclusion proof for the epoch tree.

Once it has proved inclusion it sums the reputation values stored in the leaves. Then it takes the graffiti value with the highest timestamp value and outputs a new state tree leaf for the next epoch.

Inputs:
- `fromEpoch` (public)
- `toEpoch` (public)
- `attesterId` (public)
- `epochTreeRoot` (public)
- `identityNullifier`
- `stateTreeIndexes[STATE_TREE_DEPTH]`
- `stateTreeElements[STATE_TREE_DEPTH]`
- `startPosRep`
- `startNegRep`
- `startGraffiti`
- `startTimestamp`
- `newPosRep[EPOCH_KEY_NONCE_PER_EPOCH]`
- `newNegRep[EPOCH_KEY_NONCE_PER_EPOCH]`
- `newGraffiti[EPOCH_KEY_NONCE_PER_EPOCH]`
- `newTimestamp[EPOCH_KEY_NONCE_PER_EPOCH]`
- `epochTreeElements[EPOCH_KEY_NONCE_PER_EPOCH][EPOCH_TREE_DEPTH]`

Outputs:
- `stateTreeRoot`
- `stateTreeLeaf`
- `transitionNullifier`

## Aggregate Epoch Keys

Only the root of the epoch tree is stored on chain. This proof allows multiple leaves to be added or updated in the tree using a single proof. This proof is used to update the epoch tree root onchain root.

Constants:
- `KEY_COUNT`: The max number of keys to update per proof.

Inputs:
- `fromEpochRoot` (public)
- `epoch`
- `attesterId`
- `hashchainIndex`
- `epochTreeElements[KEY_COUNT][EPOCH_TREE_DEPTH]`
- `epochKeys[KEY_COUNT`
- `epochKeyBalances[KEY_COUNT][4]`
- `oldEpochKeyHashes[KEY_COUNT]`
- `epochKeyCount`

Outputs:
- `toRoot`
- `hashchain`
