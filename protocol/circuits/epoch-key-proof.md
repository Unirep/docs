---
description: The zero-knowledge circuit of epoch key proof in UniRep
---

# Epoch Key Proof

[Epoch key](../terms-definitions/epoch-key.md) is computed by

```
hash5(identityNullifier, epoch, nonce, 0, 0)
```

The epoch key proof in UniRep is used to prove that

1. The epoch key is in the epoch that user claims
2. The epoch key nonce is between `0` and `numEpochKeyNoncePerEpoch - 1`
3. The owner of the epoch key has [registered](https://github.com/vivianjeng/UniRep/blob/git-book/introduction/README.md#1.-registration) in UniRep and has performed the [user state transition](../terms-definitions/user-state-transition.md) in the latest epoch. In other words, the user has a leaf in the global state tree.

## Public inputs

* `epoch`: the claimed epoch that the epoch key is in
* `epoch_key`: the claimed epoch key
* `GST_root`: the global state tree root that the user has a leaf in

## Private inputs

* `nonce`: the nonce of epoch key. It should be in range `[0, numEpochKeyNoncePerEpoch)`
* `identity_pk`: the identity private key that the semaphore protocol uses
* `identity_nullifier`: the identity that the semaphore protocol uses, and it is also used to generate an epoch key
*   `identity_trapdoor`: the identity trapdoor key that the semaphore protocol uses The hash output of `identity_pk`, `identity_nullifier`, and `identity_trapdoor` is the `identity_commitment` and it is used to generate a global state tree leaf by

    ```
    GST_leaf = hash(identity_commitment, UST_root)
    ```
* `user_tree_root`: the user state tree root. It is used to compute the global state tree leaf
* `GST_path_index`: the path index routes from leaf to root in the global state tree. It should be either `0` or `1` to indicate if the element is in the right sibling or the left sibling.
* `GST_path_elements`: The sibling node that should be hashed with current path element to get the root.

## Contraints

### 1. Check if user exists in the Global State Tree

```
    // COmpute the identity commitment
    component identity_commitment = IdentityCommitment();
    identity_commitment.identity_pk[0] <== identity_pk[0];
    identity_commitment.identity_pk[1] <== identity_pk[1];
    identity_commitment.identity_nullifier <== identity_nullifier;
    identity_commitment.identity_trapdoor <== identity_trapdoor;
    out <== identity_commitment.out;

    // Compute user state tree root
    component leaf_hasher = HashLeftRight();
    leaf_hasher.left <== identity_commitment.out;
    leaf_hasher.right <== user_tree_root;

    // Check if user state hash is in GST
    component GST_leaf_exists = LeafExists(GST_tree_depth);
    GST_leaf_exists.leaf <== leaf_hasher.hash;
    for (var i = 0; i < GST_tree_depth; i++) {
        GST_leaf_exists.path_index[i] <== GST_path_index[i];
        GST_leaf_exists.path_elements[i][0] <== GST_path_elements[i][0];
    }
    GST_leaf_exists.root <== GST_root;
```

### 2. Check nonce validity

```
    var bitsPerNonce = 8;

    component nonce_lt = LessThan(bitsPerNonce);
    nonce_lt.in[0] <== nonce;
    nonce_lt.in[1] <== EPOCH_KEY_NONCE_PER_EPOCH;
    nonce_lt.out === 1;
```

### 3. Check epoch key is computed correctly

```
    // Compute the epoch key
    component epochKeyHasher = Hasher5();
    epochKeyHasher.in[0] <== identity_nullifier;
    epochKeyHasher.in[1] <== epoch;
    epochKeyHasher.in[2] <== nonce;
    epochKeyHasher.in[3] <== 0;
    epochKeyHasher.in[4] <== 0;

    // Compute the quotion of the hash
    quotient <-- epochKeyHasher.hash \ (2 ** epoch_tree_depth);

    // Check equality
    epochKeyHasher.hash === quotient * (2 ** epoch_tree_depth) + epoch_key;
```

See the whole circuit in [circuits/verifyEpochKey.circom](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/circuits/verifyEpochKey.circom)
