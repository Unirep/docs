---
description: What is UniRep?
---

# 👏 Welcome

<figure><img src=".gitbook/assets/repository-unirep.png" alt="UniRep: Privacy &#x26; provable reputation"><figcaption><p>UniRep: Privacy &#x26; provable reputation</p></figcaption></figure>

### Overview

**UniRep** (**Uni**versal **Rep**utation) is a _private_ and _non-repudiable_ **reputation system**. Users can&#x20;

1. Receive positive and negative reputation from attesters.
2. Voluntarily prove that they have at least certain amount of reputation without revealing the exact amount.&#x20;
3. Moreover, users cannot refuse to receive reputation from an attester.

The high-level goal for **UniRep** is to be a base layer on top of which anyone can easily build _custom_, yet _interoperable_, reputation systems, For instance, users could create combined zero-knowledge proofs of reputation across different social media platforms, consumer apps, or financial applications, in order to provide holistic, private, and trustworthy information about themselves to others.

UniRep is originally proposed by BarryWhiteHat in [this ethresear.ch post](https://ethresear.ch/t/anonymous-reputation-risking-and-burning/3926)

### v1.1

Version 1.1 of the protocol reduces the complexity of user proofs to constant time. Users can receive unlimited attestations while keeping the proving time constant.

v1.1 also changes the tree structure, removing the user state tree completely. The global state tree and epoch tree are replaced by a state tree and epoch tree for each attester. As a result each attester can set their own epoch length. Users also execute a user state transition per attester, instead of 1 global transition.

Read the description [here](https://github.com/unirep/unirep/issues/134).

### Quick Links

* [Protocol introduction](introduction.md)
* [Getting started](getting-started/)



