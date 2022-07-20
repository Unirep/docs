---
description: This tutorial will help you run UniRep examples
---

# 🎮 Getting Started

## Install and Build 🛠

### 1. Download UniRep repository from GitHub

```bash
git clone https://github.com/Unirep/Unirep.git && \
cd Unirep
```

### 2. Install packages and build

```bash
yarn && yarn build
```

### 3. (Optional) Run test scripts

```bash
yarn test
```

## Example flow using cli commands 🔌

### 1. Spin up the testing chain

Go to`./packages/core` and run a hardhat testing node:

```bash
cd packages/core && \
npx hardhat node
```

{% hint style="info" %}
**NOTE:** a list of default accounts will be printed, choose one of them to be user's account and one to be attester's. User's and attester's private key will be referred to as `userPrivateKey` and `attesterPrivateKey` respectively.
{% endhint %}

* For example, choose `0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80` as the user's private key and `0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d` as the attester's private key
* Export both keys to the environment:

```bash
export USER_PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
export ATTESTER_PRIVATE_KEY=0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d
```

{% hint style="info" %}
**NOTE:** Then open another terminal to run the following cli-commands.
{% endhint %}

### 2. Deploy Unirep contract

Deploy the UniRep smart contract with a private key.

```bash
npx ts-node cli/index.ts deploy -d $USER_PRIVATE_KEY
```

{% hint style="info" %}
**NOTE:** `-d` is the deployer's private key.

See: [cli/deploy](cli/deploy-unirep-contract.md) for more `deploy` options.
{% endhint %}

Then the Unirep contract address will be printed:

```bash
Unirep: 0x0165878A594ca255338adfa4d48449f69242Eb8F
```

* Then we use the Unirep contract's address to interact with.
* Export contract address to the environment:

```bash
export UNIREP_CONTRACT_ADDRESS=0x0165878A594ca255338adfa4d48449f69242Eb8F
```

### 3. User generates semaphore identity

```
npx ts-node cli/index.ts genUnirepIdentity
```

* base64url encoded identity and identity commitment will be printed, For example,

```
Unirep.identity.eyJpZGVudGl0eU51bGxpZmllciI6IjdjNDY5MGQyNTVhMjFiZjU3MmI0N2E5MjA3ZTg5ZGMwZjJhY2QwNGZlOTU3M2UzY2U0MzQ4ZjVkN2FiZjRlIiwiaWRlbnRpdHlUcmFwZG9vciI6IjJiNGE3MzY4OTgyMjMxYjY4YjU3YTM1ZjdjMzBiZmY4YWQ5ZGIwMWMzY2JlZWMwNDVhZDNkY2JjMTExMDM1YyIsInNlY3JldCI6WyI3YzQ2OTBkMjU1YTIxYmY1NzJiNDdhOTIwN2U4OWRjMGYyYWNkMDRmZTk1NzNlM2NlNDM0OGY1ZDdhYmY0ZSIsIjJiNGE3MzY4OTgyMjMxYjY4YjU3YTM1ZjdjMzBiZmY4YWQ5ZGIwMWMzY2JlZWMwNDVhZDNkY2JjMTExMDM1YyJdfQ
Unirep.identityCommitment.MTEwNzczMDY1MDUwODg2ODk0NjY1MDY5Mjk0MjMyNjMzMzQ1OTM4ODIwNTA0OTk0MzQ4NTkyNjc0OTIxNDQ4NDEwNTc1MjA4NTExMTU
```

* Export both identity text to the environment:

```bash
export USER_IDENTITY=Unirep.identity.eyJpZGVudGl0eU51bGxpZmllciI6IjdjNDY5MGQyNTVhMjFiZjU3MmI0N2E5MjA3ZTg5ZGMwZjJhY2QwNGZlOTU3M2UzY2U0MzQ4ZjVkN2FiZjRlIiwiaWRlbnRpdHlUcmFwZG9vciI6IjJiNGE3MzY4OTgyMjMxYjY4YjU3YTM1ZjdjMzBiZmY4YWQ5ZGIwMWMzY2JlZWMwNDVhZDNkY2JjMTExMDM1YyIsInNlY3JldCI6WyI3YzQ2OTBkMjU1YTIxYmY1NzJiNDdhOTIwN2U4OWRjMGYyYWNkMDRmZTk1NzNlM2NlNDM0OGY1ZDdhYmY0ZSIsIjJiNGE3MzY4OTgyMjMxYjY4YjU3YTM1ZjdjMzBiZmY4YWQ5ZGIwMWMzY2JlZWMwNDVhZDNkY2JjMTExMDM1YyJdfQ && \
export IDENTITY_COMMITMENT=Unirep.identityCommitment.MTEwNzczMDY1MDUwODg2ODk0NjY1MDY5Mjk0MjMyNjMzMzQ1OTM4ODIwNTA0OTk0MzQ4NTkyNjc0OTIxNDQ4NDEwNTc1MjA4NTExMTU
```

### 4. User signs up

* Sign up user's semaphore identity with identity commitment with the prefix `Unirep.identityCommitment`.

```bash
npx ts-node cli/index.ts userSignUp \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -d $USER_PRIVATE_KEY \
    -c $IDENTITY_COMMITMENT
```

{% hint style="info" %}
**NOTE:**&#x20;

`-x` is the contract address of Unirep contract&#x20;

`-c` is the identity commitment

See: [cli/userSignUp ](cli/user-sign-up.md#usersignup)for more `userSignUp` options.
{% endhint %}

### 5. Attester signs up

```bash
npx ts-node cli/index.ts attesterSignUp \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -d $ATTESTER_PRIVATE_KEY
```

{% hint style="info" %}
**NOTE:** `-d` is attester's private key, this private key is to be used only by this attester hereafter

See: [cli/attesterSignUp](cli/user-sign-up.md#attestersignup) for more `attesterSignUp` options.
{% endhint %}

* The attester ID will be printed, for example

```bash
Attester sign up with attester id: 1
```

Export Attester Id to the environment:

```bash
export ATTESTER_ID=1
```

### 6. User generates epoch key and epoch key proof

```bash
npx ts-node cli/index.ts genEpochKeyAndProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -id $USER_IDENTITY  \
    -n 0
```

{% hint style="info" %}
**NOTE:** `-id` is user's identity and `-n` is epoch key nonce which should be less than the system parameter `maxEpochKeyNonce`

See: [cli/genEpochKeyAndProof](cli/epoch-key-and-proof.md#genepochkeyandproof) for more `genEpochKeyAndProof` options.

**NOTE:** epoch key and base64url encoded epoch key proof and public signals will be printed and they should be handed to attester to be verified, for example:
{% endhint %}

```bash
Epoch key of epoch 1 and nonce 0: 1421637519
Unirep.epk.proof.WyIyMTA5NTc4NDAyNzkwNTE1NTM4MTQ3NzE5ODAwMjMzNTU0ODgxMTkzOTk4NjcxNzE1NzY0MDU1MTQ2MTY4MTQ3Nzg4Mjc5MjAxMDM2NiIsIjEyMzYzNTc3NjM0NDc1MzM2NDQ2MjkyNjg3NTk5NTYyODU1MjY5ODA1MDUzNTE0OTExNjMwMjY1MzIwOTYyNDAxNTkxNjY0OTI0NzA1IiwiMTEzOTkxNzYwMTgzOTY0ODM1NjM4OTY3NzIzMzg1MTg1NzA2MzYxOTI2MTk1OTkyMDA3MjcwNDU0MTAzODExMDE1MzI5MDMwNzMxNjEiLCIxMzk5NTQ0NzUzMjMxMTE0OTM4MDU4NTQ1MjI1Mzg3MDk0MzQ1MzM1NDM3MjU5MjYyNjk4NjI1NDAzNTM4ODU1NDY5MjgwMzIyNjE4MSIsIjE0MDc0NDk5OTIyMDE1NTgzNzMwNjMyMjEzNjc5NDYyNTQ0NTk0NzE5MDI0NzIyMTQyNjg1Mzg3MjIzOTA3NDkzMTE0MTc0ODA1OTQiLCIxMDQ4NzU1NjIxMzIxNDM4MTMzMjgyODUwOTA3ODgzMzYzOTIwMjI1NjMwMTgwNDU5MjA0NDk1ODE3NTk0NjE3MDYyMjIyMDU4NzAyNSIsIjU4NDQxMzU5NjgwOTI1MjU5ODA4MzgxNTU1MTg2OTgwMzc1NzkyMzc1NzI5NzgwMzYxMTY0NTI5MDM1MjMwOTM0MDQyMjY3MzA1MDAiLCIxNDQxMTk1NDU0NzM0NzcwNDM5NjU3MDkxMjE0MTkzOTMwMDYyNjc3Mzg3MTE2OTUzOTE4OTc0MjU4Njk5OTcxOTQ5MzQ5NjM5ODI1MCJd
Unirep.epk.publicSignals.WyIxNDI3MDkxNTA5MjcyNTk3MDQ1MTI2ODIwNDUzODc3OTUwMDUwMjI3ODgwNzE2NzY5MDY2MTU5NTczNDE0OTExNjgzMzA2NTg3Mjk1MiIsIjEiLCIxNDIxNjM3NTE5Il0
```

Export those values to the environment:

```bash
export EPOCH_KEY=1421637519 && \
export EPOCH_KEY_PROOF=Unirep.epk.proof.WyIyMTA5NTc4NDAyNzkwNTE1NTM4MTQ3NzE5ODAwMjMzNTU0ODgxMTkzOTk4NjcxNzE1NzY0MDU1MTQ2MTY4MTQ3Nzg4Mjc5MjAxMDM2NiIsIjEyMzYzNTc3NjM0NDc1MzM2NDQ2MjkyNjg3NTk5NTYyODU1MjY5ODA1MDUzNTE0OTExNjMwMjY1MzIwOTYyNDAxNTkxNjY0OTI0NzA1IiwiMTEzOTkxNzYwMTgzOTY0ODM1NjM4OTY3NzIzMzg1MTg1NzA2MzYxOTI2MTk1OTkyMDA3MjcwNDU0MTAzODExMDE1MzI5MDMwNzMxNjEiLCIxMzk5NTQ0NzUzMjMxMTE0OTM4MDU4NTQ1MjI1Mzg3MDk0MzQ1MzM1NDM3MjU5MjYyNjk4NjI1NDAzNTM4ODU1NDY5MjgwMzIyNjE4MSIsIjE0MDc0NDk5OTIyMDE1NTgzNzMwNjMyMjEzNjc5NDYyNTQ0NTk0NzE5MDI0NzIyMTQyNjg1Mzg3MjIzOTA3NDkzMTE0MTc0ODA1OTQiLCIxMDQ4NzU1NjIxMzIxNDM4MTMzMjgyODUwOTA3ODgzMzYzOTIwMjI1NjMwMTgwNDU5MjA0NDk1ODE3NTk0NjE3MDYyMjIyMDU4NzAyNSIsIjU4NDQxMzU5NjgwOTI1MjU5ODA4MzgxNTU1MTg2OTgwMzc1NzkyMzc1NzI5NzgwMzYxMTY0NTI5MDM1MjMwOTM0MDQyMjY3MzA1MDAiLCIxNDQxMTk1NDU0NzM0NzcwNDM5NjU3MDkxMjE0MTkzOTMwMDYyNjc3Mzg3MTE2OTUzOTE4OTc0MjU4Njk5OTcxOTQ5MzQ5NjM5ODI1MCJd && \
export EPOCH_PUBLIC_SIGNALS=Unirep.epk.publicSignals.WyIxNDI3MDkxNTA5MjcyNTk3MDQ1MTI2ODIwNDUzODc3OTUwMDUwMjI3ODgwNzE2NzY5MDY2MTU5NTczNDE0OTExNjgzMzA2NTg3Mjk1MiIsIjEiLCIxNDIxNjM3NTE5Il0
```

### 7. Attesters/Users verify epoch key proof

```bash
npx ts-node cli/index.ts verifyEpochKeyProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -pf $EPOCH_KEY_PROOF  \
    -p $EPOCH_PUBLIC_SIGNALS
```

{% hint style="info" %}
**NOTE:**&#x20;

`-p` is the public signals of the epoch key proof

`-pf` is the epoch key proof

See: [cli/verifyEpochKeyProof](cli/epoch-key-and-proof.md#verifyepochkeyproof) for more `verifyEpochKeyProof` options.
{% endhint %}

If the epoch key proof is successfully verified, it prints

```bash
Verifying epoch key 1421637519 with GSTRoot 14270915092725970451268204538779500502278807167690661595734149116833065872952 in epoch 1
Verify epoch key proof with epoch key 1421637519 succeed
```

### 8. Submit epoch key proof to Unirep smart contract

The epoch key proof should be submitted to the Unirep smart contract, and obtain a proof index from Unirep smart contract, then it can be verified by others and be sent attestations.

```bash
npx ts-node cli/index.ts submitEpochKeyProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -d $USER_PRIVATE_KEY  \
    -pf $EPOCH_KEY_PROOF  \
    -p $EPOCH_PUBLIC_SIGNALS
```

{% hint style="info" %}
**NOTE:** See [cli/submitEpochKeyProof](cli/epoch-key-and-proof.md#submitepochkeyproof) for more `submitEpochKeyProof` options.
{% endhint %}

* The proof index will be printed, for example:

```
Proof index:  1
```

* Then the epoch key with the proof index should be handed to attester to be attested.

### 9. Attester attest to epoch key

```bash
npx ts-node cli/index.ts attest \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -d $ATTESTER_PRIVATE_KEY  \
    -epk $EPOCH_KEY  \
    -toi 1  \
    -pr 5  \
    -nr 4  \
    -gf 0x2098f5fb9e239eab3ceac3f27b81e481dc3124d55ffed523a839ee8446b64864  \
    -s 1
```

After attestation is submitted successfully, the message will be printed

```bash
Attesting to epoch key 1421637519 with pos rep 5, neg rep 4, graffiti 0x2098f5fb9e239eab3ceac3f27b81e481dc3124d55ffed523a839ee8446b64864 and sign up flag 1
Transaction hash: 0xcfa6432b326343c679285cebc8a7cefbb0489ad4a636d4e87b649d27892bd9f5h
```

{% hint style="info" %}
**NOTE:**&#x20;

`-epk` is the epoch key of the receiver

`-toi` is the proof index of the epoch key

`-pr` (optional) is the positive reputation given to the user

`-nr` (optional) is the negative reputation given to the user

`-gf` (optional) is the graffiti for the reputation given to the user

`-s` (optional) is the sign up flag to give to the user to indicate the attester authenticates the user's membership.

See: [cli/attest](cli/attestation.md) for more `attest` options
{% endhint %}

### 10. Epoch transition

```bash
npx ts-node cli/index.ts epochTransition \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -d $USER_PRIVATE_KEY  \
    -t 
```

{% hint style="info" %}
`NOTE:`&#x20;

`-d` private key could be anyone's private key

`-t` indicates it's testing environment so it will fast forward to the end of epoch

See: [cli/epochTransition](cli/epoch-transition.md) for more `epochTransition` options
{% endhint %}

After epoch transition transaction is submitted successfully, the message will be printed:

```bash
End of epoch: 1
```

### 11. User state transition

User should perform user state transition to receive reputation and do further actions in the protocol.

```bash
npx ts-node cli/index.ts userStateTransition \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -d $USER_PRIVATE_KEY  \
    -id $USER_IDENTITY 
```

{% hint style="info" %}
See [cli/userStateTransition](cli/user-state-transition.md) for more `userStateTransition` options
{% endhint %}

If user performs user state transition successfully, the message will be printed:

```
User transitioned from epoch 1 to epoch 2
```

### 12. User generates reputation proof

```bash
npx ts-node cli/index.ts genReputationProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -id $USER_IDENTITY  \
    -a $ATTESTER_ID  \
    -mr 1  \
    -n 0 \
    -gp 0
```

{% hint style="info" %}
**NOTE:**&#x20;

`-a` is attester's id

`-mr` is the minimum reputation score, i.e, user wants to prove that the attester gave the user a (positive reputation - negative reputation) score that's larger than the minimum reputation score

`-gp` is the pre-image of the graffiti for the reputation. `gp` in this case, `0` being the hash pre-image of `176ff05d9c7c4528b04553217098a71cd076d52623dab894a7f7ee34116ca170`

`-n` is the nonce of the output epoch key, it can be used to receive attestation

See: [cli/genReputationProof](cli/reputation-proof.md#genreputationproof) for more `genReputationProof` options
{% endhint %}

The proof will be printed and it should be handed to the receiver of this proof, for example,

```bash
Proof of reputation from attester 1:
Epoch key of the user: 364193153
Unirep.reputation.proof.WyI5MDkwMzIyMjgxNDIyNTU0NjMwOTk3NTg2MDA1NzU0MjAwODMwMjk1OTk1MTMyNjI0MjE3Njk2ODYzMzAxODQ1OTMxOTU1ODMzMDIwIiwiMTA0NzExNjQ3Nzk1Mjk0NjI5NzM0NDU2ODE5MDQ0NjgwMzQ0Mzc3NDYzNjk0ODY3NjQzNzY0Mjg5NDgwNjE4NTY3Mjg5ODU4NzMzMTUiLCIyMDI4NTA0MTM0NTUyOTc5Nzg4NjcyOTgyNDIzNzUwOTQ5MjYyODQ4NDQ5ODY1ODAzMDgzNTM0MjQ3NjU3MjU2NTE1MjIyMDkwNjcxOCIsIjIwMTYxMzc4NjcyODA2MzQxNjM3ODMzODA2NDIwNDQwMjc0NDg2ODA2NTQyNjA4NjM0MDc2MDM4MDQ1NzAzMzgxOTYwNDkxNDg5NDIiLCI0NzA5NTM0NzQzMTI1Njc5MzAzMjMwMTEzNzI1NzYzNTExNDI3MjM1MTA0NTc0NzcxNDcyNTEwNDgxNTcwMDc2ODY4NjY4MTcxOTY2IiwiMTk3MzcxMDE1Nzg0MTgwODMwMTExMjE0MzE1ODc2ODY3NjgwNDM1MTcxOTU5ODcwOTk3ODgyOTU2OTcwMTYyMzAwNjU0NTcxOTI1MDYiLCIxNjk5OTkzNTEyMTg2NTMwMDQ5ODY5NzgyOTU4ODA0NDA1MjU2ODE1Mjg5MDcyNjU2MzczMDAxODY4MDcwNDM1NTkzODg4NjM4MjYwMSIsIjEyNTEwNDIyNzY3OTA0MDkyMzgxODQ5MzkwMDI0NDY0Njk3ODUxODE5OTAwNDk2NDI0MzY0NDk5OTQ0Mjg1MDkxNzE2MzIwNzI1NjM4Il0
Unirep.reputation.publicSignals.WyIwIiwiMCIsIjAiLCIwIiwiMCIsIjAiLCIwIiwiMCIsIjAiLCIwIiwiMiIsIjM2NDE5MzE1MyIsIjExODk0OTczNjQ4ODI2MTE5MzkwNDA1MjUwMjczMzQyNTYxMTM2NzIyOTY3ODMyNDU5Njg0NjQ1NDYxNDMzNzYzNTkxMTA0NzIwMjAwIiwiMSIsIjAiLCIxIiwiMSIsIjAiXQ
```

* Export those values to the environment:

```bash
export REPUTATION_PROOF=Unirep.reputation.proof.WyI5MDkwMzIyMjgxNDIyNTU0NjMwOTk3NTg2MDA1NzU0MjAwODMwMjk1OTk1MTMyNjI0MjE3Njk2ODYzMzAxODQ1OTMxOTU1ODMzMDIwIiwiMTA0NzExNjQ3Nzk1Mjk0NjI5NzM0NDU2ODE5MDQ0NjgwMzQ0Mzc3NDYzNjk0ODY3NjQzNzY0Mjg5NDgwNjE4NTY3Mjg5ODU4NzMzMTUiLCIyMDI4NTA0MTM0NTUyOTc5Nzg4NjcyOTgyNDIzNzUwOTQ5MjYyODQ4NDQ5ODY1ODAzMDgzNTM0MjQ3NjU3MjU2NTE1MjIyMDkwNjcxOCIsIjIwMTYxMzc4NjcyODA2MzQxNjM3ODMzODA2NDIwNDQwMjc0NDg2ODA2NTQyNjA4NjM0MDc2MDM4MDQ1NzAzMzgxOTYwNDkxNDg5NDIiLCI0NzA5NTM0NzQzMTI1Njc5MzAzMjMwMTEzNzI1NzYzNTExNDI3MjM1MTA0NTc0NzcxNDcyNTEwNDgxNTcwMDc2ODY4NjY4MTcxOTY2IiwiMTk3MzcxMDE1Nzg0MTgwODMwMTExMjE0MzE1ODc2ODY3NjgwNDM1MTcxOTU5ODcwOTk3ODgyOTU2OTcwMTYyMzAwNjU0NTcxOTI1MDYiLCIxNjk5OTkzNTEyMTg2NTMwMDQ5ODY5NzgyOTU4ODA0NDA1MjU2ODE1Mjg5MDcyNjU2MzczMDAxODY4MDcwNDM1NTkzODg4NjM4MjYwMSIsIjEyNTEwNDIyNzY3OTA0MDkyMzgxODQ5MzkwMDI0NDY0Njk3ODUxODE5OTAwNDk2NDI0MzY0NDk5OTQ0Mjg1MDkxNzE2MzIwNzI1NjM4Il0 && \
export REPUTATION_PUBLIC_SIGNALS=Unirep.reputation.publicSignals.WyIwIiwiMCIsIjAiLCIwIiwiMCIsIjAiLCIwIiwiMCIsIjAiLCIwIiwiMiIsIjM2NDE5MzE1MyIsIjExODk0OTczNjQ4ODI2MTE5MzkwNDA1MjUwMjczMzQyNTYxMTM2NzIyOTY3ODMyNDU5Njg0NjQ1NDYxNDMzNzYzNTkxMTA0NzIwMjAwIiwiMSIsIjAiLCIxIiwiMSIsIjAiXQ
```

### 13. Attesters/ Users verify the reputation proof

```bash
npx ts-node cli/index.ts verifyReputationProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -pf $REPUTATION_PROOF  \
    -p $REPUTATION_PUBLIC_SIGNALS
```

{% hint style="info" %}
See: [cli/verifyReputationProof](cli/reputation-proof.md#verifyreputationproof) for more `verifyReputationProof` options
{% endhint %}

If the proof is verified valid, the message will be printed:

```
Epoch key of the user: 364193153
Verify reputation proof from attester 1 with min rep 1, reputation nullifiers amount 0 and graffiti pre-image 0, succeed
```

### 14. User generates sign up proof

```bash
npx ts-node cli/index.ts genUserSignUpProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -id $USER_IDENTITY  \
    -a $ATTESTER_ID
```

{% hint style="info" %}
**NOTE:** `-a` is attester's id. If the attester gives the attestation with a sign up flag, the user can generate a sign up proof to prove the membership from the attester.

See: [cli/genUserSignUpProof](cli/airdrop-reputation.md#genusersignupproof) for more `genUserSignUpProof` options.
{% endhint %}

The proof will be printed and it should be handed to the receiver of this proof, for example,

```
Proof of user sign up from attester 1:
Epoch key of the user: 364193153
Unirep.signUp.proof.WyIxODk1ODk4MTczMTgxNDk5NTAxODYxNDMyMjY5MDQ3NTkxNDE1MjQ3MzgxMzM1OTk1MjQ2NTc3Njk3MjEzODY5Njc2MDQzMTU2NzQwMiIsIjUwNjIwNjAxMTg1Njk1MDMyMDU4MTUxMjM2OTUxMTU2MTQwMTc5ODM2NDg2ODc5ODA1MDI4MTY2NTY4MjA2NTk0NzYxNjE1ODc1MTQiLCIxOTM1MDc5NTEzNzk5MzU2ODYxNjY5NTU4NDE0NTg0NjkyMTgxNTA3MzUyNTU3NDY0ODIzNDU3NDQ3ODU4MzYzMDYzMzU5MzA1MDYxOCIsIjE4ODkyNDY4NTM0NjAwNDMxMTg2ODM5MTk0NTg1Mjc2MjIwNDg3MzI2MTgyMzExNDc2NzIyNzg0OTY5NDM0NDk2Njg0ODE1MTUwNzg3IiwiODk5Mzk5NTQ1OTI3MDIxMzg0ODA2OTc2MjM5NjQ5NjQzNTMxMjg4NzkxMTM1ODQyNjk2MTY5MTY3NTQzNjE0NTU4MTc4ODIzMjE1IiwiMTIyMjA1MzY1MzA5MDU4Mjk1MDk2NTYwMzIzNjY2OTExMzU5MjI5MzQyMTUzODY3MDAyMjc3ODMzMDg3MDIyOTM1MzQ4MTI2ODk0NDAiLCIzMTk5OTM0MDg0MDA0NzQ3Njk3MDI5MzM4MDAyNDQ1Nzg1NTMzMjY0OTQwNTEwMjY4NDYzNjgxMTE4MjA1MTM0MTMzODg0Njc4NTg3IiwiNjM5MTU3NDIyNzU4MTY1Mzk2NzM2MDI0MjIyMDc4NzYzMTk3MDAzMDk0Mzc3NzE1NzM5NzQ1NDkxODYzNTc4MTUwNDkyNzk4MTcwNiJd
Unirep.signUp.publicSignals.WyIyIiwiMzY0MTkzMTUzIiwiMTE4OTQ5NzM2NDg4MjYxMTkzOTA0MDUyNTAyNzMzNDI1NjExMzY3MjI5Njc4MzI0NTk2ODQ2NDU0NjE0MzM3NjM1OTExMDQ3MjAyMDAiLCIxIiwiMSJd
```

Export those values to the environment:

```bash
export SIGNUP_PROOF=Unirep.signUp.proof.WyIxODk1ODk4MTczMTgxNDk5NTAxODYxNDMyMjY5MDQ3NTkxNDE1MjQ3MzgxMzM1OTk1MjQ2NTc3Njk3MjEzODY5Njc2MDQzMTU2NzQwMiIsIjUwNjIwNjAxMTg1Njk1MDMyMDU4MTUxMjM2OTUxMTU2MTQwMTc5ODM2NDg2ODc5ODA1MDI4MTY2NTY4MjA2NTk0NzYxNjE1ODc1MTQiLCIxOTM1MDc5NTEzNzk5MzU2ODYxNjY5NTU4NDE0NTg0NjkyMTgxNTA3MzUyNTU3NDY0ODIzNDU3NDQ3ODU4MzYzMDYzMzU5MzA1MDYxOCIsIjE4ODkyNDY4NTM0NjAwNDMxMTg2ODM5MTk0NTg1Mjc2MjIwNDg3MzI2MTgyMzExNDc2NzIyNzg0OTY5NDM0NDk2Njg0ODE1MTUwNzg3IiwiODk5Mzk5NTQ1OTI3MDIxMzg0ODA2OTc2MjM5NjQ5NjQzNTMxMjg4NzkxMTM1ODQyNjk2MTY5MTY3NTQzNjE0NTU4MTc4ODIzMjE1IiwiMTIyMjA1MzY1MzA5MDU4Mjk1MDk2NTYwMzIzNjY2OTExMzU5MjI5MzQyMTUzODY3MDAyMjc3ODMzMDg3MDIyOTM1MzQ4MTI2ODk0NDAiLCIzMTk5OTM0MDg0MDA0NzQ3Njk3MDI5MzM4MDAyNDQ1Nzg1NTMzMjY0OTQwNTEwMjY4NDYzNjgxMTE4MjA1MTM0MTMzODg0Njc4NTg3IiwiNjM5MTU3NDIyNzU4MTY1Mzk2NzM2MDI0MjIyMDc4NzYzMTk3MDAzMDk0Mzc3NzE1NzM5NzQ1NDkxODYzNTc4MTUwNDkyNzk4MTcwNiJd && \
export SIGNUP_PUBLIC_SIGNALS=Unirep.signUp.publicSignals.WyIyIiwiMzY0MTkzMTUzIiwiMTE4OTQ5NzM2NDg4MjYxMTkzOTA0MDUyNTAyNzMzNDI1NjExMzY3MjI5Njc4MzI0NTk2ODQ2NDU0NjE0MzM3NjM1OTExMDQ3MjAyMDAiLCIxIiwiMSJd
```

### 15. Attesters/ Users verify the sign up proof

```bash
npx ts-node cli/index.ts verifyUserSignUpProof \
    -x $UNIREP_CONTRACT_ADDRESS  \
    -pf $SIGNUP_PROOF  \
    -p $SIGNUP_PUBLIC_SIGNALS
```

{% hint style="info" %}
**NOTE:** See: cli/verifyUserSignUpProof for more `verifyUserSignUpProof` options.
{% endhint %}

## Computation happens <mark style="color:red;">off-chain ℹ️</mark>

After you read through the introduction above, you should have a picture of how Unirep works. User/attester registers on-chain, attester submits attestations on-chain, user submits proof to update his state and also the global state tree of current epoch in Unirep contract. These all happens on-chain, including proof verification, updating global state trees and generating epoch trees, but these computation could be very expensive!

There are no on-chain assets that required latest state of the contract in order to transfer its ownership or to apply computation on it. There's no such asset in Unirep, all you have is one's reputation and proving one's reputation does not has to be done on-chain, instead the proof is transmitted to the verifier off-chain. So there's really no need to do all these computation on-chain!

So that's why the current implementation of Unirep is taking the **LazyLedger**-like approach - the Unirep contract (i.e., the underlying Ethereum chain) is serving as the data availability layer and the computations including proof verification all happen on top of this data availability layer. We log every user/attester actions like register/submit attestation/submit state transition proof and the according data. Then we perform state transition off-chain according to the order of when these events took place and everyone that does the same should arrive at the exact same global state! (assuming no re-org in the underlying data availability layer)

You can take a look at [`Synchronizer`](https://github.com/Unirep/Unirep/blob/update-readme/packages/core/src/Synchronizer.ts) to better know how a user can fetch the events from the contract and build up the latest global state.
