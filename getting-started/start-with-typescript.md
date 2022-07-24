---
description: Try UniRep protocol with typescript.
---

# Start with Typescript 📠

### 0. Install packages with `npm` or `yarn`

* npm

```bash
npm i --save-dev @unirep/crypto @unirep/circuits @unirep/contracts @unirep/core
```

* yarn

```bash
yarn add -D @unirep/crypto @unirep/circuits @unirep/contracts @unirep/core
```

{% hint style="info" %}
In the following example, we use `hardhat` as the ethereum environment.

See how to create a hardhat project at: [hardhat.org](https://hardhat.org/tutorial/creating-a-new-hardhat-project)
{% endhint %}

### 1. deploy&#x20;

Spin up the testing chain with

```bash
npx hardhat node
```

Use the hardhat testing chain as an example config.

```
PROVIDER=http://127.0.0.1:8545/
PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```

{% code title="deploy.ts" %}
```typescript
import { ethers } from 'ethers'
import { deployUnirep } from '@unirep/contracts'

async function main(){
    const provider = await ethers.getDefaultProvider(PROVIDER)
    const signer = new ethers.Wallet(
        PRIVATE_KEY,
        provider
    )
    const contract = await deployUnirep(
        signer
    )
    console.log("Unirep address: ", contract.address)
}

main();
```
{% endcode %}

{% hint style="warning" %}
Now the verifiers are fixed in `@unirep/contracts` so it can be deployed with `deployUnirep` function.\
To make verifiers more flexible, it it recommended to download [Unirep repository](https://github.com/Unirep/Unirep) and deploy verifiers before deploying `Unirep.sol`.
{% endhint %}

### 2. User signs up in UniRep

Should connect to a deployed contract first.

```typescript
import { getUnirepContract } from '@unirep/contracts'
import { ZkIdentity } from '@unirep/crypto'

// user can connect the Unirep smart contract with getUnirepContract
const contract = await getUnirepContract(
    UNIREP_CONTRACT_ADDRESS,
    signer
)
// generate a semaphore identity
const identity = new ZkIdentity()
// sign up in UniRep smart contract with semaphore identity commitment
const tx = await contract.userSignUp(
    identity.genIdentityCommitment()
)
// print the semaphore identity and save it
console.log(identity.serializeIdentity())
```

### 3. Attester signs up in UniRep

```typescript
// connect a attester with a wallet
const attester = new ethers.Wallet(
    ATTESTER_PRIVATE_KEY,
    provider
)
// attester sign up in UniRep smart contract
const tx = await contract
    .connect(signer)
    .attesterSignUp()
```

### 4. User generates epoch key and epoch key proof

Before generating an epoch key proof, we should generate a current user state to know the current [global state tree](../protocol/glossary/trees.md#global-state-tree)s and the attestation histories.

We should initialize a storage to save the state.

```typescript
import { DB, SQLiteConnector } from 'anondb/node'
import { schema } from '@unirep/core'

// construct a memory db
const memoryDB = await SQLiteConnector.create(schema, ':memory:')
// construct a SQLite db
const sqliteDB = await SQLiteConnector.create(schema, 'test.sqlite')
```

Also we have to initialize a prover to generate proofs and verify proofs.

```typescript
const snarkjs = require('snarkjs')
import { Circuit, Prover } from '@unirep/circuits'
import { SnarkProof, SnarkPublicSignals } from '@unirep/crypto'
const buildPath = 'PATH/TO/THE/KEYS/'

const prover: Prover = {
    genProofAndPublicSignals: async (
        proofType: string | Circuit,
        inputs: any
    ): Promise<{
        proof: any,
        publicSignals: any
    }> => {
        const circuitWasmPath = buildPath + `${proofType}.wasm`
        const zkeyPath = buildPath + `${proofType}.zkey`
        const { proof, publicSignals } = await snarkjs.groth16.fullProve(
            inputs,
            circuitWasmPath,
            zkeyPath
        )
        return { proof, publicSignals }
    },
    verifyProof: async (
        name: string | Circuit,
        publicSignals: SnarkPublicSignals,
        proof: SnarkProof
    ): Promise<boolean> => {
        const vkey = require(buildPath + `${name}.vkey.json`)
        return snarkjs.groth16.verify(vkey, publicSignals, proof)
    },
}
```

{% hint style="warning" %}
After clone the [Unirep repository](https://github.com/Unirep/Unirep) and run

`yarn && yarn build`

The `buildPath` will be found at `packages/circuits/zksnarkBuild`

In the future it will be uploaded to a server to be downloaded.
{% endhint %}

Also, the `ZkIdentity` can be unserialized with a serialized identity, for example:

```typescript
import { ZkIdentity } from '@unirep/core'

const identity = new ZkIdentity(
    Strategy.SERIALIZED, 
    `{"identityNullifier":"27d1ae5c98aab64b851a9c668a7eec0d835867a17d4b9454a8bf9824836271d6","identityTrapdoor":"2596ecc2a1e1f6a8f279e097464e6edc3b18b946d934398dfe52a34c4e414e67","secret":["27d1ae5c98aab64b851a9c668a7eec0d835867a17d4b9454a8bf9824836271d6","2596ecc2a1e1f6a8f279e097464e6edc3b18b946d934398dfe52a34c4e414e67"]}`
)
```

And then we can use a `genUserState` to perform synchronization.

```typescript
const genUserState = async (
    provider: ethers.providers.Provider,
    address: string,
    userIdentity: ZkIdentity,
    db: DB
) => {
    const contract = getUnirepContract(address, provider)
    const userState = new UserState(
        db,
        prover,
        contract,
        userIdentity
    )
    await userState.start()
    await userState.waitForSync()
    return userState
}
```

Use the user state to generate epoch key proof.

```typescript
const userState = await genUserState(
    provider,
    "0x0165878A594ca255338adfa4d48449f69242Eb8F",
    identity,
    db
)
// genearte epoch key proof
const proof = await userState.genVerifyEpochKeyProof(0)
// verify if the epoch key proof is valid
const isValid = await proof.verify()
console.log(isValid)
```

Submit the epoch key proof

```typescript
const tx = await contract.submitEpochKeyProof(
    proof.publicSignals,
    proof.proof
)
```

Then get the proof index with the proof hash

```typescript
const proofHash = proof.hash()
const proofIdx = await contract.getProofIndex(proofHash)
console.log(proofIdx)
```

### 5. Submit attestations

