# MOAC DID Method Specification

## Introduction
The MOAC distributed identity system aims to provide a secure, robust and flexible implementation of the DID and Verifiable Claims specifications published by the W3C and the Decentralised Identity Foundation. It’s core technologies are the MOAC blockchain.
### MOAC
MOAC is a public permissionless blockchain and smart contract execution environment. The Virtual Machine system provides certainty and security about code execution by being based upon a public blockchain.
## Overview
The MOAC DID method uses MOAC blockchain as a decentralized storage layer for DID Documents. A deployed smart contract provides a mapping from a DID to an MOAC blockchain hash address of the corrosponding DID Document. This enables DID Documents on MOAC blockchain to be effectively addressed via their DIDs. 
## Specification
### Method DID Format
MOAC DIDs are identifiable by their did\:moac: method string and conform to the [Generic DID Scheme](https://w3c-ccg.github.io/did-spec/#the-generic-did-scheme).

Example `moac` DID:

 `did:moac:2f2b37c890824242cb9b0fe5614fa2221b79901e`

### DID Creation
The creation of a DID follows a few steps:
1. Generate 32 bytes of entropy
2. From the entropy, generate an MOAC blockchain key pair
3. From the MOAC blockchain key pair, take the keccak256 hash of the public key

The hash from step 3 is your DID.
### DID Registration/Anchoring
DID Anchoring refers to creating the mapping from DID to MOAC blockchain address on the smart contract using the setRecord function, effectively 'anchoring' the DID to the smartcontract and making its DID Document accessable with only the DID. This anchoring process is analogous the the Create step of a CRUD database. This process is also tied to the creation of the DID, as the MOAC blockchain key pair used to generate the DID should also be the one to perform anchoring transaction on the smart contract.
### DID Document Resolution
DID Document resolution is achieved by querying the registry smartcontract getRecord function with a DID. If that DID is registered, an MOAC blockchain address will be returned. Otherwise, an empty address is returned. This MOAC blockchain address can then be resolved through smart contract to the DID Document.
### DID Document Updating and Deleting
MOAC blockchain addresses are hashes of their content, so an updated DID Document will also have a new MOAC blockchain address. Thus updating simply uses the setRecord smartcontract function again with the same DID and the new MOAC blockchain hash of the updated DID Document. Deletion, similarly, is updating the registry to return an all-0 byte string, as if it had never been initialised.
## Key Management
### Key Recovery
Writing to the MOAC registry mapping contract requires control over the private key used to anchor the DID, so any key recovery method which applies to MOAC blockchain keys can be applied here. This can involve seed phrases stored in an analogue manner among other methods.
### Key Revocation
As there is no centralised control of the registry contract, no party can revoke the keys of DID control of another party under the MOAC system.
## Privacy and Security Considerations
### Key Control
As mentioned in the Key Recovery section, the entity which controls the private key which anchored the DID also effectively controls the DID Document which the DID resolves to. Thus great care should be taken to ensure that the private key is kept private. Methods for ensuring key privacy are outside the scope of this document.
### DID Document Public Profile
The DID Document anchored with the registry contract can contain any content, though it is recommended that it conforms to the [W3C DID Document Specificaiton](https://w3c-ccg.github.io/did-spec/#did-documents). As registered DIDs can be resolved by anyone, care should be taken to only update the registry to resolve to DID Documents which DO NOT expose any sensitive personal information, or information which you may not wish to be public.

## References
[1]. MOAC foundation, https://www.moac.io

[2]. MOAC project github, https://github.com/MOACChain

[3]. W3C Decentralized Identifiers (DIDs) v0.11, https://w3c-ccg.github.io/did-spec
