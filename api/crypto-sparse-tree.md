### Usage

Import this class like so:

```js
import { SparseMerkleTree } from '@unirep/crypto'
```

Any functions documented here will be maintained according to semver.

### `class SparseMerkleTree`

##### constructor

Get a new tree instance.
```ts
constructor(
  height: number,
  zeroValue: bigint = 0
): SparseMerkleTree
```

##### height

Get the number of levels in the tree. (getter)
```ts
this.height
```

##### root

Get the current root of the tree. (getter)
```ts
this.root
```

##### insert

Insert a new leaf in the next free index.
```ts
this.insert(leaf: bigint)
```

##### update

Update a leaf in the tree.
```ts
this.update(index: bigint, value: bigint)
```

##### createProof

Create a merkle inclusion proof for a leaf.
```ts
this.createProof(index: bigint): bigint[]
```

##### verifyProof

Verify a merkle inclusion proof.
```ts
this.verifyProof(index: bigint, proof: bigint[]): boolean
```
