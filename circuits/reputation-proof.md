---
description: The zero-knowledge circuit of reputation proof in UniRep
---

# Reputation Proof

Users can use a reputation proof to claim that how the reputation is from a given attester. There are following things that user can choose to prove:

1.  The `pos_rep - neg_rep` given by the attester is more than the claimed `min_rep` i.e.

    ```
    (pos_rep - neg_rep) > min_rep
    ```
2.  The `graffiti_preimage` of a `graffiti` i.e.

    ```
    hash(graffiti_preimage) == graffiti
    ```
3.  The [reputation nullifiers](../terms/nullifiers.md#reputation-nullifiers) are computed correctly i.e.

    ```
    // for all nonces
    nonce >= 0
    nonce < pos_rep - neg_rep
    reputation_nullifiers = hash5(REPUTATION_NULLIFIER_DOMAIN, identity_nullifier, epoch, nonce, 0)
    ```

The circuit also checks if the user has [registered](https://github.com/vivianjeng/UniRep/blob/git-book/introduction/README.md#1.-registration) and performed [user state transition](../terms/user-state-transition.md) in the claimed epoch.

## Public inputs

* `epoch`
* `epoch_key`
* `GST_root`
* `attester_id`
* `rep_nullifiers_amount`
* `min_rep`
* `prove_graffiti`
* `graffiti_pre_image`

## Public outputs

* `rep_nullifiers`

## Private inputs

* `epoch_key_nonce`
* `identity_pk`
* `identity_nullifier`
* `identity_trapdoor`
* `user_tree_root`
* `GST_path_index`
* `GST_path_elements`
* `pos_rep`
* `neg_rep`
* `graffiti`
* `sign_up`
* `UST_path_elements`
* `selectors`
* `rep_nonce`

## Contraints

### 1. Check if user exists in the Global State Tree and verify epoch key

### 2. Check if the reputation given by the attester is in the user state tree

### 3. Check if reputation nullifiers are valid

### 4. Check if user has reputation greater than `min_rep`

### 5. Check pre-image of graffiti

See the whole circuit in [circuits/proveReputation.circom](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/circuits/proveReputation.circom)
