# User State Transition

* User performs user state transition by calling [`updateUserStateRoot`](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L502)
  * User will attach a [User State Transition Proof](../circuits/user-state-transition-proof.md) when calling `updateUserStateRoot`. Others can make sure if the user state transition is correct by verifying the User State Transition Proof.
  * Once the user performed user state transition, his user state will be inserted into the [global state tree](trees.md#global-state-tree) of the latest epoch
  * So if a user does not perform user state transition during an epoch, his user state will not be in the global state tree of that epoch
* User **should** perform user state transition before he can prove the latest attestations he received.
  * Also, user **should** perform user state transition before he can receive any attestations further. Attester can still attest to a user's epoch key in the past epoch but the user will not be able to process the attestation.

***

* See also
  * [Trees](trees.md)
  * [Epoch Transition](epoch.md)
  * [User State Transition Proof](../circuits/user-state-transition-proof.md)
