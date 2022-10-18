# Table of contents

* [👏 Welcome](README.md)
* [🧩 Introduction](introduction.md)

## ☀ Protocol

* [Glossary](protocol/glossary/README.md)
  * [Attestations](protocol/glossary/attestation.md)
  * [Users and Attesters](protocol/glossary/users-and-attesters.md)
  * [Epoch](protocol/glossary/epoch.md)
  * [Epoch Key](protocol/glossary/epoch-key.md)
  * [Reputation](protocol/glossary/reputation.md)
  * [Trees](protocol/glossary/trees.md)
  * [Nullifiers](protocol/glossary/nullifiers.md)
  * [User State Transition](protocol/glossary/user-state-transition.md)
* [Circuits](protocol/circuits/README.md)
* [Contract](protocol/contract/README.md)

## 🌈 API <a href="#package" id="package"></a>

* [@unirep/crypto](package/crypto.md)
  * [hash\*()](./api/crypto-hashes.md#hashes)
  * [genEpochKey](./api/crypto-hashes.md#genEpochKey)
  * [genEpochNullifier()](./api/crypto-hashes.md#genEpochNullifier)
  * [genStateTreeLeaf()](./api/crypto-hashes.md#genStateTreeLeaf)
  * [IncrementalMerkleTree](./api/crypto-incremental-tree.md)
    * [constructor()](./api/crypto-incremental-tree.md#constructor)
    * [insert()](./api/crypto-incremental-tree.md#insert)
    * [update()](./api/crypto-incremental-tree.md#update)
    * [delete()](./api/crypto-incremental-tree.md#delete)
    * [createProof()](./api/crypto-incremental-tree.md#createProof)
    * [verifyProof()](./api/crypto-incremental-tree.md#verifyProof)
  * [SparseMerkleTree](./api/crypto-sparse-tree.md)
    * [constructor()](./api/crypto-sparse-tree.md#constructor)
    * [height](./api/crypto-sparse-tree.md#height)
    * [root](./api/crypto-sparse-tree.md#root)
    * [insert()](./api/crypto-sparse-tree.md#insert)
    * [update()](./api/crypto-sparse-tree.md#update)
    * [createProof()](./api/crypto-sparse-tree.md#createProof)
    * [verifyProof()](./api/crypto-sparse-tree.md#verifyProof)
* [@unirep/circuits](package/circuits.md)
* [@unirep/contracts](package/contracts.md)
* [@unirep/core](package/core.md)

## 🌻 Applications

* [Unirep Social](applications/unirep-social.md)
