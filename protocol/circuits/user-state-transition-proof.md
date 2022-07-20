---
description: The zero-knowledge circuit of user state transition proof in UniRep
---

# User State Transition Proof

The [user state transition](../terms-definitions/user-state-transition.md) proof is used to process attestations from the latest epoch the user transitioned and then compute the latest [global state tree](../terms-definitions/trees.md#global-state-tree) leaf.

The user state transition circuit checks that

1. The user has [registered](https://github.com/vivianjeng/UniRep/blob/git-book/introduction/README.md#1.-registration) and performed [user state transition](../terms-definitions/user-state-transition.md) in the previous epoch.
2.  The [user state tree](../terms-definitions/trees.md#user-state-tree) is updated correctly by the attestations, and the same attestations are chained by hash functions.

    * For example, the original user state tree root is `r_1`, and the original user state tree leaf has 5 positive repuation

    ```
    user_state_tree_leaf = hash(5, 0, 0, 0, 0)
    ```

    * An incoming attestation has
      * `attester_id = 1`
      * `pos_rep = 3`
    * compute the hash of [reputation](../terms-definitions/reputation.md#reputation)

    ```
    hash_reputation = hash(5 + 3, 0, 0, 0, 0)
    ```

    * compute the updated user state tree root `r_2` with user state tree leaf `hash(8, 0, 0, 0, 0)` in the leaf index `1`
    * compute the hash of [attestation](../terms-definitions/reputation.md#attestation)

    ```
    hash_attestation = hash(1, 3, 0, 0, 0)
    ```

    * compute the hash chain

    ```
    hash_chain = hash(hash_attestation, hash_chain)
    ```
3. After all attestations of all epoch keys are processed, the circuit seals all hash chains and computes the [epoch tree](../terms-definitions/trees.md#epoch-tree). (If the output epoch tree root mismatches others' epoch tree roots, then the user state transition proof is invalid because the user process attestations in a wrong way.)
4. Compute the updated global state tree leaf by

```
hash(identity_commitment, updated_user_state_root)
```

Ideally, the user state transition proof should be in [one circuit](https://github.com/appliedzkp/UniRep/blob/5701dd3c060c332e8e83895c49e3a90b3fca3c49/circuits/userStateTransition.circom). But the circuit size is too big to generate a proof in a browser, we seperate the proof into three different parts: **start transition proof**, **process attestations proof**, and **user state transition proof**.

The idea of chaining these three proofs but without revealing user's sensitive information is to use `blinded_user_state` and `blinded_hash_chain`

```
blinded_user_state = hash(identity_nullifier, user_tree_root, epoch, from_epoch_key_nonce, 0)
blinded_hash_chain = hash(identity_nullifier, current_hash_chain, epoch, from_epoch_key_nonce, 0)
```

In the start transition proof, the circuit will compute the initial `blinded_user_state` and `blinded_hash_chain`, the `user_tree_root` is the latest transitioned user state tree and the `hash(identity_commitment, updated_user_state_root)` should be in a global state tree. The `current_hash_chain` is start with `0` as the definition of hash chain.

After `blinded_user_state` and `blinded_hash_chain` are submitted, the user can take them as public inputs and start to process attestations according to the `user_tree_root` and `current_hash_chain`. When the attestations limit reaches (e.g. a `processAttestations` circuit can process only 10 attestations per proof) or all attestations to the epoch key are processed, the circuit will output another `blinded_user_state` and `blinded_hash_chain` to continue processing attestations.

The `epoch_key_nonce` is used in the blinded user state and blinded hash chain to indicate the attestations of which epoch key is processed. In the final `userStateTransition` proof, it checks all epoch key with different `epoch_key_nonce` are processed and the hash chain result matches the epoch tree.

There are only one user state tree result after all attestations are processed, so in the final proof it only takes the initial `blinded_user_state` and the final one and computes the new global state tree leaf. On the other hand, there are `numEpochKeyNoncePerEpoch` hash chains after processing attestations, so the final circuit will take `numEpochKeyNoncePerEpoch` `blinded_hash_chain` to check the epoch tree root.

The user state tree root is continuously updated: the output should be the input of another proof, so the `processAttestation` proof takes `blinded_user_state` as public input and output another `blinded_user_state`. The hash chain results might not be continuously. When all attestations of one epoch key is processed, the hash chain of the next epoch key should be `0` but not the previous hash chain. Therefore, `processAttestation` proof does not take `blinde_hash_chain` as input.

While verifying all of the proofs, there are the following things to check to make sure it follows the rules:

1. `startTransitionProof` takes a valid global state tree root as a public input.
2. `processAttestationsProof` takes `blinded_user_state` as public input and `blinded_hash_chain` as private input from either `startTransitionProof` or another `processAttestationsProof`.
3. `userStateTransitionProof` takes two `blinded_user_state`, one is from `startTransitionProof` and the other is from the latest `processAttestationsProof`.
4. `userStateTransitionProof` takes `numEpochKeyNoncePerEpoch` `blinded_hash_chain` from `processAttestationsProof`.

## 1. Start Transition proof

### Public inputs

* `GST_root`

### Public outputs

* `blinded_user_state`
* `blinded_hash_chain_result`

### Private inputs

* `epoch`
* `epoch_key_nonce`
* `user_tree_root`
* `identity_pk`
* `identity_nullifier`
* `identity_trapdoor`
* `GST_path_elements`
* `GST_path_index`

### Contraints

#### 1. Check if user exists in the Global State Tree

#### 2. Compute blinded public output

See the whole circuit in [circuits/startTransition.circom](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/circuits/startTransition.circom)

## 2. Process attestations proof

### Public inputs

* `input_blinded_user_state`

### Public outputs

* `blinded_user_state`
* `blinded_hash_chain_result`

### Private inputs

* `epoch`
* `from_nonce`
* `to_nonce`
* `identity_nullifier`
* `intermediate_user_state_tree_roots`
* `old_pos_reps`
* `old_neg_reps`
* `old_graffities`
* `old_sign_ups`
* `path_elements`
* `attester_ids`
* `pos_reps`
* `neg_reps`
* `graffities`
* `overwrite_graffities`
* `sign_ups`
* `selectors`
* `hash_chain_starter`

### Contraints

#### 1. Verify blinded input user state

#### 2. Verify attestation hash chain

#### 3. Process attestations and update user state tree

#### 4. Compute blinded public output

See the whole circuit in [circuits/processAttestations.circom](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/circuits/processAttestations.circom)

## 3. User State Transition proof

### Public inputs

* `epoch`
* `GST_root`
* `blinded_hash_chain_results`
* `epoch_tree_root`

### Public outputs

* `new_GST_leaf`
* `epoch_key_nullifier`

### Private inputs

* `blinded_user_state`
* `intermediate_user_state_tree_roots`
* `start_epoch_key_nonce`
* `end_epoch_key_nonce`
* `identity_pk`
* `identity_nullifier`
* `identity_trapdoor`
* `GST_path_elements`
* `GST_path_index`
* `epk_path_elements`
* `hash_chain_results`

### Contraints

#### 1. Check if user exists in the Global State Tree

#### 2. Process the hashchain of the epoch key specified by nonce `n`

#### 3. Check if blinded user state matches

#### 4. Compute and output nullifiers and new GST leaf

See the whole circuit in [circuits/userStateTransition.circom](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/circuits/userStateTransition.circom)
