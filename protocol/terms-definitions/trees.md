# Trees

## **User state tree**

*   A user state tree is a sparse merkle tree with it's leaves storing reputation received from each attester, e.g.,

    * a user state tree leaf = hash of the reputation

    ```
                                  user state tree root
                                  /                  \
      hash(DEFAULT_REP_HASH, 0xabc...)              hash(0xbcd..., 0xcde...)
             /        \                                      /         \
    [No rep for leaf 0] [leaf 1: 0xabc...]              [leaf 2: 0xbcd...] [leaf 3: 0xcde...]
    ```
* NOTE: `[leaf 1: 0xabc...]` represents the reputation received from attester with `attesterId = 1` and `0xabc...` is the hash of the reputation
*   Hash of the reputation:

    ```
    hashReputation = hash5(posRep, negRep, graffiti, signUp, 0)
    ```

    where, `posRep` is the positive reputation given by the attester `negRep` is the negative reputation given by the attester `graffifi` is the message given by the attester `signUp` indicates if the attester authenticates the user

## **Global state tree**

* A global state tree stores the updated user state after a user signs up and a user performs the [user state transition](user-state-transition.md).
* It is an incremental sparse merkle tree with it's leaves storing users' `identityCommitment`s and `userStateRoot`s, e.g.,
  * a global state tree leaf = `hash(identityCommitment, userStateRoot)`

```
                            global state tree root
                            /                \
        hash(0xabc..., 0xcde...)         hash(0xdef..., DEFAULT_EMPTY_HASH)
               /        \                          /         \
[leaf_0: 0xabc...] [leaf_1: 0xcde...] [leaf_2: 0xdef...] [DEFAULT_EMPTY_HASH]
```

* NOTE: this is an incremental merkle tree so leaves are inserted from left (leaf index 0) to right, one by one, instead of inserted directly into the specified leaf index.
* NOTE: since global state tree leaf is the hash of `identityCommitment` and `userStateRoot`, others will be not be able to tell which user (his `identityCommitment`) inserted his user state into global state tree.

## **Epoch tree**

* An epoch tree is used to prevent users from omitting any attestation attesting to the user. If the user skip one attestation, the hash chain and the output epoch tree root will be different from others.
*   An epoch tree is a sparse merkle tree with it's leaves storing hashchain results of each epoch key, e.g.,

    ```
                                  epoch tree root
                                 /               \
        hash(DEFAULT_EMPTY_HASH, 0x123...)   hash(DEFAULT_EMPTY_HASH, 0x456...)
              /             \                      /                  \
    [DEFAULT_EMPTY_HASH]  [epk_1: 0x123...]    [DEFAULT_EMPTY_HASH]  [epk_3: 0x456...]
    ```
* The hash chain is computed by

```
hashChainResult = hash(attestation_3, hash(attestation_2, hash(attestation_1, 0)))
```

*   an attestation includes the following data:

    ```
    struct Attestation {
      // The attester’s ID
      uint256 attesterId;
      // Positive reputation
      uint256 posRep;
      // Negative reputation
      uint256 negRep;
      // A hash of an arbitary string
      uint256 graffiti;
      // A flag to indicate if user has signed up in this leaf
      uint256 signUp;
    }
    ```
* a reputation includes the following data: `posRep, negRep, graffiti, signUp`
  * it does not include `attesterId` like an attestation does because reputation is already stored in user state tree with `attesterId` as leaf index
