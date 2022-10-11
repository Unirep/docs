---
description: Definition of reputation in UniRep
---

# Reputation

## Reputation

* The reputation in UniRep protocol includes
  * `posRep`: is the positive reputation given by the attester
  * `negRep`: is the negative reputation given by the attester
  * `graffiti`: is the message given by the attester
  * `timestamp`: is the timestamp the last graffiti was received
* The hash of the reputation is computed by the [Poseidon hash](https://www.poseidon-hash.info/) function.

```typescript
const hashReputation = hash(posRep, negRep, graffiti, timestamp)
```

* The overall **reputation status** of a user is stored in the users latest state tree leaf.

## Attestation

* Attestations are stored as leaves in the epoch tree

{% hint style="info" %}
See also

* [Trees](trees.md)
* [Epoch Transition](epoch-transition.md)
* [User State Transition](user-state-transition.md)
* [User State Transition Proof](../circuits/README.md)
{% endhint %}
