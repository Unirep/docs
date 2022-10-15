# Contract

View the source [here](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/contracts/contracts/Unirep.sol).

## User Signup

Users signup by generating a [signup proof](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/circuits/circuits/signup.circom). This proof will output an identity commitment and a state tree leaf with a 0 reputation balance. This proof can be used with the `signup` function in the UniRep contract.

```solidity
function function userSignUp(
  uint256[] memory publicSignals,
  uint256[8] memory proof
) public
```

{% hint style="info" %}
source: [Unirep.sol/submitAttestation](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/contracts/contracts/Unirep.sol#L68)
{% endhint %}

## Attester Signup

Attesters signup by calling the `attesterSignUp` function and specifying the epoch length, in seconds, they would like to use. The attester id is the address of the contract making the call.

Attesters can signup directly using this function:

```solidity
function attesterSignUp(
    uint256 epochLength /* in seconds */
) public
```

Or they can generate an ECDSA signature and allow a relay to signup on their behalf using this function:


```solidity
function attesterSignUpViaRelayer(
    address attester,
    uint256 epochLength,
    bytes calldata signature
) public
```

{% hint style="info" %}
source: [Unirep.sol/attesterSignUp](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/contracts/contracts/Unirep.sol#L150)
{% endhint %}

## Attestations

An attester can create an attestation to an epoch key by calling the `submitAttestation` function in the UniRep contract. Attesters can give positive reputation, negative reputation, or graffiti.

```solidity
function submitAttestation(
    uint256 targetEpoch,
    uint256 epochKey,
    uint256 posRep,
    uint256 negRep,
    uint256 graffiti
) public
```

{% hint style="info" %}
source: [Unirep.sol/submitAttestation](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/contracts/contracts/Unirep.sol#L174)
{% endhint %}


## Epoch Transition

Each attester has their own epoch length. Epoch transitions happen automatically anytime state changes are made to the system. The current epoch is defined as `(block.timestamp - attester.startTimestamp) / attester.epochLength`.

## User State Transition

User state transitions are executed when a user wants to move from an old epoch to a new epoch. The user must submit a [user state transition proof](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/circuits/circuits/userStateTransition.circom).

```solidity
function userStateTransition(
    uint256[] memory publicSignals,
    uint256[8] memory proof
) public
```

{% hint style="info" %}
source: [Unirep.sol/userStateTransition](https://github.com/Unirep/Unirep/blob/45611e98b5859f24a13acc03a757695277d990b4/packages/contracts/contracts/Unirep.sol#L214)
{% endhint %}
