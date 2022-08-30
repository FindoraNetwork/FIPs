---
fip: 2
title: Auditable Privacy-Preserving Assets
author: Discreet Labs Crypto Team
discussions-to: https://discord.com/channels/789009413976883220/1014281214951170138
type: Standards Track
category: Core
status: Draft
created: 2022-08-11
requires: 
---

# FIP 2- Auditable Privacy-Preserving Assets

## Abstract

This FIP describes a systematic approach to instantiate a new kind of assets that is both auditable and privacy-preserving on the Findora UTXO chain. 

Asset issuers, such as central banks, can issue "auditable assets". These assets still have the same privacy guarantees as other assets on the chain. In particular, they can be "triple masked", such that other users on the blockchain cannot see the owner's address, the amount, or the asset type. However, different from other existing assets, they are auditable to the asset issuers.

Asset issuers can audit how these assets are being tranferred through the network. In other words, the privacy guarantees of the assets are no longer against the asset issuers. This enables compliance, and is extremely suitable for CBDC or sensitive tokens. 

The auditable assets may, in the future, work together with some sort of zk-DID (decentralized identities) to provide more fine-grained compliance, but it is beyond the scope of this FIP.

## Construction

This FIP proposes a new family of assets called "auditable assets". By using these auditable assets, users implicitly consent to the asset issuers to audit these transactions. 

To distinguish "auditable assets" from existing assets on the chain (such as FRA), this FIP first extends the asset type code, currently 32 bytes, to 33 bytes, where the last byte is used to indicate if it is an auditable asset. Auditable assets will have the last byte be '2'. 

For this FIP, we restrict the auditable assets to be on UTXO chain for now and do not permit them to go to the EVM chain. This is done by asking Prism not to transform these assets until a future FIP figures out how to provide the ability of audit on the EVM chain. 

The privacy-preserving protocols on the Findora blcokchain, one based on the Maxwell construction, one based on the Zerocash construction, will handle these auditable assets differently, as follows.

- **The Maxwell construction (confidential transactions):** Findora Zei library already has a comprehensive line of cryptographic tools for audit on the Maxwell construction. The tools include confidential credentials (for zk-DID) and verifable responsible disclosure. The Maxwell construction in Findora is instantiated by Bulletproofs. To deploy this on the auditable assets, the Maxwell construction will place some minimal but necessary restrictions to these assets, discussed as follows.
    - It is permitted to hide the amount of auditable assets, but it is not permitted to hide the asset type of auditable assets. This is done by preventing new asset-type-hidden auditable assets from being created on the UTXO chain.
    - For auditable assets, the confidential transactions will be required to attach an "auditor's memo" and a matrix sigma proof showing that the auditor's memo correctly encrypts the information of the inputs and outputs of this transaction, which enables auditing.
    
- **The Zerocash construction (triple masking):** The existing triple masking protocol will no longer accept auditable assets. To transfer auditable assets, users need to use "auditable triple masking", which is a new set of zero-knowledge proof protocols built off Jubjub-Rescue hybrid encryption, a binary-checking-friendly TurboPlonk in Findora, and Bulletproofs. The auditable triple masking has additional requirements for the transactions, as follows.
    - The auditable triple masking takes as input "the auditor's memo" and verifies that it can be correctly decrypted and it matches the masked information. The auditor's memo uses hybrid encryption, which consists of a point in Jubjub and some ciphertexts encrypted via Rescue-CTR. The zero-knowledge proof in TurboPlonk takes the input and a random linear combination of the ciphertexts (leveraging the Fiat-Shamir transform) as input and performs such verification.
    - A restriction is added to the Maxwell-Zerocash switching protocol, which prohibits switching an auditable Zerocash asset to a Maxwell asset that hides the asset type.
    - The existing triple masking protocol declines to handle auditable assets, but the auditable protocol can handle both existing assets and auditable assets.
    - Nevertheless, existing assets and auditable assets will share the same commitment-nullifier ledger to enable interoperability. 


## Specification

### Asset type code of an auditable asset

An auditable asset will have an asset type of 33 bytes, where the last byte is '2', indicating that this is an auditable asset. The first 32 bytes store the compressed representation of one of the auditor's public keys, which is a Jubjub public key.

### Auditor's public keys

Information for audit will be encrypted under the auditor's public keys. There are two such keys, as we now describe:
- A public key over the Ristretto group of the curve25519 curve, which is registered separately on chain when the asset is created. This will be used in the Maxwell construction.
- A public key over the Jubjub curve, which is embedded in the asset type code as described above, and will be used in the Zerocash construction.

### Auditor's memo

Transactions that involve an auditable asset must have an auditor's memo, which is an encrypted copy of all the input and output information of a transaction. 
- In the Maxwell construction, the auditor's memo consists of Ristretto ElGamal ciphertexts as well as a matrix sigma proof stating that these ciphertexts match the Pedersen commitments in the transaction. 
- In the Zerocash construction, the auditor's memo is a standard hybrid encryption using the Diffie-Hellman assumption. The memo consists of a point in Jubjub and several scalars in BLS12-381's scalar field, which are the Rescue-CTR encryption of all the input and output information of a transaction. The zero-knowledge proof in TurboPlonk is responsible to verify that the auditor's memo is being generated correctly.

## Cryptographic building blocks

Below we briefly describe the main cryptographic building blocks for the auditable assets. This includes some building blocks that have already been used in the existing assets.

### Bulletproofs

Bulletproofs is a zero-knowledge proof system based on inner-product arguments. There are a few advantages of Bulletproofs for the Findora blockchain:
- Transparent setup, which avoids the needs for the users to download large structured reference strings.
- FFT-free and pairing-free, which allows Bulletproofs to work on a larger selection of elliptic curves, such as curve25519 and secq256k1.
- Succinct, which is very uncommon for a zero-knowledge proof system with a transparent setup, and is a result of inner-product arguments.

We use Bulletproofs in the Maxwell construction. It does not replace the TurboPlonk in the Zerocash construction due to the inherent tradeoff of proving overhead and the linear verification time of inner-product arguments.

### Matrix sigma

Matrix sigma is a generalized interface for proving a relation between elements in a group where the discrete log problem is assumed to be difficult. It can be viewed as a generalization of the Chaum-Pedersen protocol.

We use matrix sigma in the Maxwell construction to prove that the auditor's memo includes the correct input and output information of the transaction. 

### SNARK-friendly hybrid encryption

Checking a hybrid encryption is generally difficult if the encryption is done over cryptographic schemes that are not designed to be SNARK-friendly, in which case the zero-knowledge proof that verifies such a hybrid encryption may take a prohibitively long time. 

To avoid this bottleneck, the auditor's memo in the Zerocash construction will use SNARK-friendly hybrid encryption. In particular, we use the Jubjub curve as the group where we perform the so-called Diffie-Hellman key exchange step in hybrid encryption. We do not choose the Bandersnatch curve because it is an incomplete twisted Edwards curve and we are unable to leverage the GLV endomorphism. The symmetric encyption part is done using Rescue-CTR, basically using Rescue hash function in the counter mode to encrypt the data. We choose Rescue as the hash function because it is able to leverage TurboPlonk to significantly reduce its overhead in the proof system.

We use SNARK-friendly hybrid encryption in the Zerocash construction to encrypt the auditor's memo.

### Binary-checking-friendly TurboPlonk

Findora uses a variant of TurboPlonk that is binary-checking-friendly. This serves as the core building block of the Zerocash construction as a general-prupose proof system. This variant follows the standard recipe of customizing TurboPlonk. It has five wires, thirteen selectors, high degrees, and additional polynomial identity constraints. It includes customized gates for Rescue and boolean testing. This makes it suitable to prove statements in time severalfold smaller than a non-customized proof system. 

We use TurboPlonk in the Zerocash construction to perform the checking of the auditor's memo as well as carrying out the rest of the protocol in the Zerocash construction.


## Backward Compatibility

The proposal in this FIP is backward compatible, both in the Maxwell construction and in the Zerocash construction, because the domain of existing assets and the new auditable assets are separated in terms of asset type codes, and the new assets will be handled by separate protocols. 


## Reference Implementation
The necessary building blocks for this FIP are all implemented and used in production in [the Zei library](https://github.com/FindoraNetwork/zei). A concrete instantiation of audit for the Maxwell construction can be found [here](https://github.com/FindoraNetwork/zei/blob/develop/api/src/xfr/asset_tracer.rs). 


## Preliminary benchmarks
We have preliminary benchmark result for auditable Maxwell assets. In the Maxwell construction, the auditor's memo contains a matrix sigma proof. The cost of generating the proof is four scalar multiplications over Ristretto, which is about 60 µs in our benchmark. The cost of verifying is seven scalar multiplications (256-bit) over Ristretto, or 105 µs. The proof consists of three points and two scalars over Ristretto, which is about 160 bytes.


## Copyright
Copyright and related rights waived via [CC0](../LICENSE).