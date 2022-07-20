# User State Transition

* After epoch transition, user **should** perform user state transition before he can prove the latest attestations he received.
* Also, user **should** perform user state transition before he can receive any attestations further. Attester can still attest to a user's epoch key in the past epoch but the user will not be able to process the attestation.
* User performs user state transition by calling [`updateUserStateRoot()`](https://github.com/Unirep/Unirep/blob/f3502e1a551f63ab44b73444b60ead8731d45167/packages/contracts/contracts/Unirep.sol#L559)``
  * User will attach a [User State Transition Proof](../../circuits/user-state-transition-proof.md) when calling `updateUserStateRoot`. Others can make sure if the user state transition is correct by verifying the User State Transition Proof.
  * Once the user performed user state transition, his user state will be inserted into the [global state tree](trees.md#global-state-tree) of the latest epoch
  * So if a user does not perform user state transition during an epoch, his user state will not be in the global state tree of that epoch.

***

* See also
  * [Trees](trees.md)
  * [Epoch Transition](epoch.md)
  * [User State Transition Proof](../../circuits/user-state-transition-proof.md)
