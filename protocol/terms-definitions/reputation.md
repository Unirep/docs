---
description: Definition of reputation in UniRep
---

# Reputation

## Reputation

* The reputation in UniRep protocol includes
  * `posRep`: is the positive reputation given by the attester
  * `negRep`: is the negative reputation given by the attester
  * `graffiti`: is the message given by the attester
  * `signUp`: indicates if the attester authenticates the user
* The hash of the reputation is computed by the Poseidon hash function

```
hashReputation = hash5(posRep, negRep, graffiti, signUp, 0)
```

* The overall reputation status of a user is stored in the user's [User State Tree](trees.md#user-state-tree)
  * where the index of the leaf is the attester ID
  * the leaf value is the accumulated `hashReputation` that the attester gives

## Attestation

* The attestation is almost the same as reputation, but it includes an `attesterId`
* The hash of attestation is computed by

```
hashAttestation = hash5(attesterId, posRep, negRep, graffiti, signUp)
```

*   The attestations to an epoch key would be chained together. A hashchain would be formed by the hashes of the attestations.

    * For example, **Hash chain** is computed by

    ```
    hashChainResult = hash(hashAttestation_3, hash(hashAttestation_2, hash(hashAttestation_1, 0)))
    ```

    * So user can not omit any attestation because the [circuit](../circuits/user-state-transition-proof.md) requires each attestation in the hashchain to be processed.
    * if user omits an attestation, then the computed hashchain would not match the one in the Unirep contract

***

* See also
  * [Trees](trees.md)
  * [Epoch Transition](epoch-transition.md)
  * [User State Transition](user-state-transition.md)
  * [User State Transition Proof](../circuits/user-state-transition-proof.md)
