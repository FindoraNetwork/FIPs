---
fip: 3
title: Zero-Knowledge Decentralized ID (zkDID)
author: Discreet Labs
discussions-to: https://discord.com/channels/789009413976883220/1014259928896114768
type: Standards Track
category: Core
status: Draft
created: 2022-08-30
requires: 
---

### Simple Summary

A decentralized identity management system that enables the creation and viewing of decentralized identities and the related (confidential) credentials’ data values. 

### Abstract

We introduce EIP-3, which is a W3C standards-based decentralized identity (DID) system that will allow Findora users, identity issuers, credential issuers, and verifiers to create a DID that is related to a credential that can *confidentially store* valuable data (aka claims) associated with the DID owner. There will be a one-to-one relationship between the DID and credential. 

When verifying a confidential credential’s claims, the DID owner will have the option of disclosing some (or all) of the claims in full detail or disclosing some (or all) of the claims in a *zero-knowledge* fashion (i.e. disclose that the claim has valid input that exists or that the claim value is above or below a certain value).

### Motivation

Many Dapps can be limited in the features they offer or use cases they can serve because of the lack of on-chain identity and user credentials. By enabling DIDs and credentials, Dapps can access claims data from credentials to enhance their features and service offerings. 

As a simple example, a user who creates a DID with a verifiable credential that contains a claim that represents the user’s credit score value can now enable Dapps to access this credit score — a valuable piece of input needed to dynamically offer an optimal interest rate on a loan the DID owner is applying for.

### Specification

Key functionality includes:

- creating a DID with a private key
- creating a unsigned credential
- creating a verified credential (by signing an unsigned credential)
- creating credential hash (that points to the credential’s location)
- storing credential hash on-chain
- storing a confidential credential on-chain
- updating a confidential credential
- viewing a confidential credential
    - view all claims of a confidential credential (in full detail)
    - view a subset of claims of a confidential credential (in full detail)
    - verify a claim field contains valid data (i.e. verify the claim is not empty but don’t reveal the data value)
    - verify that a claim value is above or below a certain value (but don’t reveal the data value)