# Unirep Smart Contract

## User Signs Up

User signs up by providing an identity commitment. It also inserts a state leaf into the state tree.

```
function userSignUp(uint256 _identityCommitment) external
```

source: [Unirep.sol/userSignUp](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L217)

If user signs up through an attester (`msg.sender` is an registered attester) and the attester sets the airdrop amount `airdropAmount` non-zero, the user will have a one non-zero leaf in his user state. The non-zero leaf is computed by [`hashAirdroppedLeaf`](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/DomainObjs.sol#L18)

```
hasSignedUp = 1
airdroppedLeaf = hash(airdropPosRep, 0, 0, hasSignedUp)
```

## Attester Signs up

Attester can sign up through `attesterSignUp` and `attesterSignUpViaRelayer`.

```
function attesterSignUp() external
```

source: [Unirep.sol/attesterSignUp](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L270)

If an attester signs up through his wallet or another smart contract, UniRep smart contract will record the `msg.sender` and assign an attester ID to the address.

```
function attesterSignUpViaRelayer(address attester, bytes calldata signature) external
```

source: [Unirep.sol/attesterSignUpViaRelayer](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L283)

The attester can also sign up through another relayer, but he has to provide the address of the attester who wants to sign up and the signature. The attester signs over it's own address concatenated with this contract address, then the UniRep smart contract will verify the signature by [verifySignature](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L251)

## Set airdrop amount

After signing up, attesters can set the airdrop amount that whoever signs up through the attester, the user can get airdropped positive reputation.

```
function setAirdropAmount(uint256 _airdropAmount) external
```

source: [Unirep.sol/setAirdropAmount](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L297)

## Submit Epoch Key Proof

The epoch key proof should be submitted before to get attestation. Then others can verify if the attestation is given to a valid epoch key.

```
function submitEpochKeyProof(EpochKeyProofRelated memory epochKeyProofData) external
```

source: [Unirep.sol/submitEpochKeyProof](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L351)

The structure of `EpochKeyProofRelated` is defined by

```
    struct EpochKeyProofRelated{
        uint256 globalStateTree;
        uint256 epoch;
        uint256 epochKey;
        uint256[8] proof;
    }
```

## Submit Attestation

An attester can submit the attestation with a proof index. A valid proof is either a [epoch key proof](../circuits/epoch-key-proof.md), a [user sign up proof](../circuits/user-sign-up-proof.md) or a [reputation proof](../circuits/reputation-proof.md) with epoch key one of the public signals. An attester can also submit attestations through a relayer or not.

```
function submitAttestation(
        Attestation calldata attestation, 
        uint256 epochKey, 
        uint256 _proofIndex
    ) external payable
```

source: [Unirep.sol/submitAttestation](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L307)

```
function submitAttestationViaRelayer(
        address attester,
        bytes calldata signature,
        Attestation calldata attestation,
        uint256 epochKey,
        uint256 _proofIndex
    ) external payable
```

source: [Unirep.sol/submitAttestationViaRelayer](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L327)

## Airdrop Epoch key

An attester can submit the airdrop attestation to an epoch key with a sign up proof. The `msg.sender` should match the `attesterId` in the `signUpProofData`.

```
function airdropEpochKey(SignUpProofRelated memory signUpProofData) external payable
```

source: [Unirep.sol/airdropEpochKey](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L368)

The structure of `SignUpProofRelated`is defined by

```
    struct SignUpProofRelated{
        uint256 epoch;
        uint256 epochKey;
        uint256 globalStateTree;
        uint256 attesterId;
        uint256[8] proof;
    }
```

## Spend Reputation

A user spend reputation via an attester, the non-zero nullifiers will be processed as a negative attestation, and the spent reputation cannot be re-used.

```
function spendReputation(ReputationProofRelated memory reputationProofData) external payable
```

source: [Unirep.sol/spendReputation](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L404)

The structure of `ReputationProofRelated`is defined by

```
    struct ReputationProofRelated{
        uint256[] repNullifiers;
        uint256 epoch;
        uint256 epochKey;
        uint256 globalStateTree;
        uint256 attesterId;
        uint256 proveReputationAmount;
        uint256 minRep;
        uint256 proveGraffiti;
        uint256 graffitiPreImage;
        uint256[8] proof;
    }
```

## Epoch Transition

While the `block.timestamp - latestEpochTransitionTime >= epochLength`, users can perform epoch transition to receive reputation and start using another epoch keys.

```
function beginEpochTransition() external
```

source: [Unirep.sol/beginEpochTransition](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L456)

## User State Transition

There are three steps in user state transition (see [user state transition proof](../circuits/user-state-transition-proof.md)), and they should be performed in order.

```
function startUserStateTransition(
    uint256 _blindedUserState,
    uint256 _blindedHashChain,
    uint256 _globalStateTree,
    uint256[8] calldata _proof
) external
```

source: [Unirep.sol/startUserStateTransition](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L472)

```
function processAttestations(
    uint256 _outputBlindedUserState,
    uint256 _outputBlindedHashChain,
    uint256 _inputBlindedUserState,
    uint256[8] calldata _proof
) external
```

source: [Unirep.sol/processAttestations](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L487)

```
function updateUserStateRoot(
    UserTransitionedRelated memory userTransitionedData, 
    uint256[] memory proofIndexRecords
) external
```

source: [Unirep.sol/updateUserStateRoot](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L502)

The structure of `UserTransitionedRelated`is defined by

```
    struct UserTransitionedRelated{
        uint256 newGlobalStateTreeLeaf;
        uint256[] epkNullifiers;
        uint256 transitionFromEpoch;
        uint256[] blindedUserStates;
        uint256 fromGlobalStateTree;
        uint256[] blindedHashChains;
        uint256 fromEpochTree;
        uint256[8] proof;
    }
```

The `proofIndexRecords` is the proof indexes of all `startTransitionProof` and `processAttestationsProof` that are submitted sequentially.

## Verify Proofs

There is an api in UniRep to call all of the verifiers.

### Epoch Key Verifier

```
function verifyEpochKeyValidity(
    uint256 _globalStateTree,
    uint256 _epoch,
    uint256 _epochKey,
    uint256[8] calldata _proof
) external view returns (bool)
```

source: [Unirep.sol/verifyEpochKeyValidity](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L520)

### Reputation Verifier

```
function verifyReputation(
    uint256[] calldata _repNullifiers,
    uint256 _epoch,
    uint256 _epochKey,
    uint256 _globalStateTree,
    uint256 _attesterId,
    uint256 _proveReputationAmount,
    uint256 _minRep,
    uint256 _proveGraffiti,
    uint256 _graffitiPreImage,
    uint256[8] calldata _proof
) external view returns (bool)
```

source: [Unirep.sol/verifyReputation](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L678)

### User Sign Up Verifier

```
function verifyUserSignUp(
    uint256 _epoch,
    uint256 _epochKey,
    uint256 _globalStateTree,
    uint256 _attesterId,
    uint256[8] calldata _proof
) external view returns (bool)
```

source: [Unirep.sol/verifyUserSignUp](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L731)

### User State Transition Verifier

```
function verifyStartTransitionProof(
    uint256 _blindedUserState,
    uint256 _blindedHashChain,
    uint256 _GSTRoot,
    uint256[8] calldata _proof
) external view returns (bool)
```

source: [Unirep.sol/verifyStartTransitionProof](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L559)

```
function verifyProcessAttestationProof(
    uint256 _outputBlindedUserState,
    uint256 _outputBlindedHashChain,
    uint256 _inputBlindedUserState,
    uint256[8] calldata _proof
) external view returns (bool)
```

source: [Unirep.sol/verifyProcessAttestationProof](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L593)

```
function verifyUserStateTransition(
    uint256 _newGlobalStateTreeLeaf,
    uint256[] calldata _epkNullifiers,
    uint256 _transitionFromEpoch,
    uint256[] calldata _blindedUserStates,
    uint256 _fromGlobalStateTree,
    uint256[] calldata _blindedHashChains,
    uint256 _fromEpochTree,
    uint256[8] calldata _proof
) external view returns (bool)
```

source: [Unirep.sol/verifyUserStateTransition](https://github.com/appliedzkp/UniRep/blob/7e5cf425242134f73b6131778549b6039ea20a9b/contracts/Unirep.sol#L627)
