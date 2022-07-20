---
description: The zero-knowledge circuit of user sign up proof in UniRep
---

# User Sign Up Proof

The user sign up proof is used to indicate if the user has a valid membership from an attester. Attesters can send a [reputation](../protocol/glossary/reputation.md) with a `signUp` flag to authenticate the user. Once the attester has signed the user up, the sign up flag will not be changed (in the current version).

In the current version, the user sign up proof is used to give users the reputation airdrop. Once the users are authenticated by an attester, the user can get the airdrop from the attester. Then the users also need epoch keys to receive reputation from the attester.

The idea of the user sign up proof (or called the airdrop proof) is to prevent the same user from obtaining airdrop twice in the same epoch. As a result, the airdrop proof output an epoch key an fix the nonce to `0`.

Therefore, the proof checks that

1. If the user has a sign-up flag from a given attester.
2. The user has [registered](https://github.com/vivianjeng/UniRep/blob/git-book/introduction/README.md#1.-registration) in UniRep and has performed the [user state transition](../protocol/glossary/user-state-transition.md) in the latest epoch. In other words, the user has a leaf in the global state tree.
3. If the sign up proof epoch matches the current epoch
4. If the output epoch key is computed with the `nonce = 0`

## Public inputs

* `epoch`
* `epoch_key`
* `GST_root`
* `attester_id`

## Private inputs

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

## Contraints

### 1. Check if user exists in the Global State Tree and verify epoch key

### 2. Check if the reputation given by the attester is in the user state tree

### 3. Check if user has signed up in the attester's app

See the whole circuit in [circuits/proveUserSignUp.circom](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/circuits/proveUserSignUp.circom)
