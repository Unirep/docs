# Nullifiers

* Nullifiers are used to prevent things from happening more than once
* UniRep uses two different nullifiers

## Epoch key nullifiers

* epoch key nullifiers are used to prevent users from using the same epoch key twice and prevent users from double user state transition
* nullifier of an epoch key is computed by

```
hash5(EPOCH_KEY_NULLIFIER_DOMAIN, identityNullifier, epoch, nonce, 0)
```

## Reputation nullifiers

* reputation nuliifiers are used to prevent users from double spending the reputation if an action requires users to spend reputation
* nullifier of a reputation spent is computed by

```
hash5(REPUTATION_NULLIFIER_DOMAIN, identityNullifier, epoch, nonce, attesterId)
```

{% hint style="info" %}
* NOTE: `EPOCH_KEY_NULLIFIER_DOMAIN` and `REPUTATION_NULLIFIER_DOMAIN` are used to prevent mixed-up of epoch key nullifiers and reputation nullifiers
{% endhint %}
