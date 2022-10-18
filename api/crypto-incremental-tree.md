### Usage

Import this class like so:

```js
import { IncrementalMerkleTree } from '@unirep/crypto'
```

Any functions documented here will be maintained according to semver.

### `class IncrementalMerkleTree`

##### constructor

Get a new tree instance.
```ts
constructor(
  depth: number,
  zeroValue: number = 0,
  arity: number = 2
): IncrementalMerkleTree
```

##### insert

Insert a new leaf in the next free index.
```ts
this.insert(leaf: bigint)
```

##### update

Update a leaf in the tree by index.
```ts
this.update(index: number, leaf: bigint)
```

##### delete

Delete (set to zero value) a leaf in the tree.
```ts
this.delete(index: number)
```

##### Proof

A struct for representing merkle proofs.

```ts
type Proof = {
  root: bigint,
  leaf: bigint,
  pathIndices: number[],
  siblings: bigint[][]
}
```

##### createProof

Get a merkle inclusion proof for an index.
```ts
this.createProof(index: number): Proof
```

##### verifyProof

Verify a merkle proof in the tree.
```ts
this.verifyProof(proof: Proof): boolean
```
