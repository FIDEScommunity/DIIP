Dutch Decentralized Identity Profile
==================

**Profile Status:** Draft

**Latest Draft:**
[https://dutchblockchaincoalition.github.com/DDIP](https://dutchblockchaincoalition.github.com/DDIP)

Editors:
~ [Niels Klomp](https://www.linkedin.com/in/niels-klomp/) (Sphereon)
~ [Timo Glastra](https://www.linkedin.com/in/timoglastra/) (Animo Solutions)

Contributors:
~ [TODO]() 

**Special Thanks:**

This profile is based on a lot of work done by the SSI community, given this profile is largely based on and uses sections of the [DIF JWT VC Presentation Profile](https://identity.foundation/jwt-vc-presentation-profile/), we would like to place special thanks to the editors and contributors of that profile.

Participate:
~ [GitHub repo](https://github.com/DutchBlockchainCoalition/DDIP.git)
~ [File a bug](https://github.com/DutchBlockchainCoalition/DDIP.git/issues)
~ [Commit history](https://github.com/DutchBlockchainCoalition/DDIP.git/commits/main)

------------------------------------

## Abstract

The Dutch Decentralized Identity Profile, or DDIP for short, defines a set of requirements against existing specifications to enable the interoperable issuance and presentation of Verifiable Credentials (VCs) between Wallets and Verifiers.

This document is not a specification, but a **profile**. It outlines existing specifications required for implementations to interoperate among each other. It also clarifies mandatory to implement features for the optionalities mentioned in the referenced specifications.

The profile uses OpenID for Verifiable Presentations ([[ref: OpenID4VP ID1]]) as the base protocol for the request and verification of W3C JWT VCs as W3C Verifiable Presentations ([[ref: VC Data Model v1.1]]). A full list of the open standards used in this profile can be found in [Overview of the Open Standards Requirements](#overview-of-the-open-standards-requirements).

### Audience

The audience of the document includes Verifiable Credential implementers and/or enthusiasts. The first few sections give an overview of the problem area and profile requirements for DDIP. Subsequent sections are detailed and technical, describing the protocol flow and request-responses.

## Status of This Document

The status of the Dutch Decentralized Identity Profile v0.1.0 is a DRAFT specification under development.

### Description

The [[ref: VC Data Model v1.1]] specification defines the data model of Verifiable Credentials (VCs) but does not prescribe standards for transport protocol, key management, authentication, query language, etc. As a result, if implementers decide which standards to use for their implementations on their own, there is no guarantee that other companies will also support the same set of standards.

This document aims to provide a path to interoperability by standardizing the set of specifications that enable the presentation of JWT VCs between implementers. Future versions of this document will include details on issuance and Wallet interoperability. Ultimately, this profile will define a standardized approach to Verifiable Credentials so that distributed developers, apps, and systems can share credentials through common means.


### Scope

#### Scope

This document is currently scoped for the presentation of VCs between the Wallet and the Verifier. The Wallet is a native mobile application. The following aspects are in scope:

- Data model
- Protocol to request presentation of VCs, including query language
- User authentication layer using Self-Issued ID Token
- Mechanism to establishing trust in the DID via Domain Linkage
- Identifiers of the entities
- Revocation of VCs
- Crypto suites

#### Out of Scope

The following items are out of scope for the current version of this document:
- Issuance of the VCs
- Advanced concepts in the [[ref: VC Data Model v1.1]]: `credentialSchema` (`credentialType` is used instead), `refreshService`, `termsOfUse`, `evidence`, and Disputes.
- Selective disclosure and unlinkability
- Zero-Knowledge Proofs
- Non-native Wallets like web applications, PWAs, etc.

## Structure of this Document

First, this profile outlines open standards required to be supported. Than, it describes detailed requirements for each specification.

## Terminology

This section consolidates in one place common terms used across open standards that this profile consists of. For the details of these, as well as other useful terms, see text within each of the specification listed in [[ref: References]].

[[def:Authorization Request]]
~ OAuth 2.0 Authorization Request extended by [[ref: OpenID4VP]] and [[ref: SIOPv2]].

[[def:Authorization Response]]
~ OAuth 2.0 Authorization Response extended by [[ref: OpenID4VP]] and [[ref: SIOPv2]].

[[def: Decentralized Identifier, DID]]
~ An identifier with its core ability being enabling Clients to obtain key material and other metadata by reference, defined in [[ref: DID Core]]. 

[[def: Holder]]
~ An entity that possesses or holds Verifiable Credentials and can generate Verifiable Presentations from them as defined in [[ref: VC Data Model v1.1]].

[[def:End User]]
~ Human Participant.

[[def: OpenID Provider (OP)]]
~ OAuth 2.0 Authentication Server implementing [[ref: OpenID Connect Core]] and [[ref: OpenID4VP]]

[[def: Presentation]] 
~ Data derived from one or more Verifiable Credentials, issued by one or more issuers, that is shared with a Verifier.

[[def: Relying Party (RP)]]
~ OAuth 2.0 Client application using [[ref: OpenID Connect Core]] and [[ref: OpenID4VP]] in [[ref: SIOPv2]]. Synonymous with term Verifier.

[[def:Request Object]]
~ JWT that contains a set of Authorization Request parameters as its Claims.

[[def: Self-Issued OpenID Provider (Self-Issued OP)]]  
~ An OpenID Provider (OP) used by an End User to prove control over a cryptographically verifiable identifier such as a DID.

[[def: Verifiable Credential (VC)]]
~ A set of one or more Claims made by an issuer that is tamper-evident and has authorship that can be cryptographically verified.

[[def: Verifiable Presentation (VP)]] 
~ A Presentation that is tamper-evident and has authorship that can be cryptographically verified.

[[def: Verifier]]
~ An entity that receives one or more Verifiable Credentials inside a Verifiable Presentation for processing. During presentation of Credentials, Verifier acts as an OAuth 2.0 Client towards the Wallet that is acting as an OAuth 2.0 Authorization Server. The Verifier is a specific case of OAuth 2.0 Client, just like Relying Party (RP) in [[ref: OpenID Connect Core]].

[[def: Issuer]]
~ An entity that issues Verifiable Credentials.

[[def: Wallet]]
~ An entity that receives, stores, presents, and manages credentials and key material of the End User. During presentation of VP(s) using [[ref: OpenID4VP]], the Wallet acts as an OAuth 2.0 Authorization Server towards the Verifier that is acting as an OAuth 2.0 Client. During user authentication using [[ref: SIOPv2]], the Wallet acts as a Self Issued OpenID Provider towards the Verifier that is a specific case of the Relying Party in [[ref: OpenID Connect Core]]. 

## Profile

## Issuance
TODO: For issuance of Verifiable Credentials...


## Presentation
The presentation from the Wallet to the Verifier is done using the [[ref: OpenID4VP]] and [[ref: Presentation Exchange v1.0.0]] specifications. Actually for DDIP we follow the [[ref: JWT VC Presentation Profile]], with the below changes

### DID Methods
Implementers are required to support DID:WEB and DID:JWK. It does not have mandatory support for DID:ION, unlike the [[ref: JWT VC Presentation Profile]] which we use as a basis for the Presentation.

#### DID Web Method
Support for [[ref: DID WEB Method]] as mentioned in the [[ref: JWT VC Presentation Profile]] is required.

#### No Blockchain and thus no DID ION Method
Since we didn't want to include a blockchain based DID method in Version one of DDIP, we do not require [[ref: DID ION method]] support in DDIP, contrary to the [[ref: JWT VC Presentation Profile]]. Of course implementations are free to support the [[ref: DID ION Method]]. One of the drawbacks of course is that this means that key history as well as rotations are not really supported in an interoperable way. This is mostly a problem for organizations, since Natural Persons would never use ledger based DID methods anyway.

#### Addition of DID JWK Method
We do support the [[ref: DID JWK Method]], given this DID method is simply an encoding of a Json Web Key. As such it also supports X509 certificates. This is a very common way to encode keys and certificates in current solutions and thus we believe it is important to support this method. A DID JWK can either have a Certificate Chain incorporated (x5c) in the DID Document or linked as a URL (x5u). 

#### DID Key Method
TODO: EBSI now is using JCS with DID:key for NP. It makes sense that we once again favor did:key over did:web. It does mean we would loose the ability to use X509 certificates, as did:key can only handle RSA keys and no certificates.


### Linked Domain Verification is fully optional
Contrary to the [[ref: JWT VC Presentation Profile]], the use of [[ref: Linked Domain Verification]] is fully optional. If not present for a party, the other party should not raise an error. Although we believe Linked Domains are a nice optional trust anchor, we also wanted to keep the DDIP v1 profile as Simple as possible at this point in time. Focusing on technical interoperability first and governance interoperability later.


## Security Considerations

## Use-Cases

## Examples

Examples are listed inline in above sections as well as in complete form within [Test Vectors](#test-vectors).

## Implementations

## Testing

## Test Vectors

## References

### Normative References

[[def: OpenID Connect Core]]
~ [Open ID Connect](https://openid.net/specs/openid-connect-core-1_0.html). Nat Sakimura, John Bradley, Michael B. Jones, Breno de Medeiros, Chuck Mortimore. 2014.11. Status: Approved Specification.

[[def: DID Core]]
~ [Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/2021/PR-did-core-20210803/). Manu Sporny, Dave Longley, Markus Sabadello, Drummond Reed, Orie Steele, Christopher Allen. 2021.08. Status: W3C Proposed Recommendation.

[[def: SIOPv2 ID1]]
~ [Self-Issued OpenID Provider v2 (First Implementer’s Draft)](https://openid.net/specs/openid-connect-self-issued-v2-1_0-ID1.html). Kristina Yasuda, Michael B. Jones, Torsten Lodderstedt. 2022.04. Status: Standards Track.

[[def: OpenID4VP ID1]]
~ [OpenID for Verifiable Presentations (First Implementer’s Draft)](https://openid.net/specs/openid-connect-4-verifiable-presentations-1_0-ID1.html). Oliver Terbu, Torsten Lodderstedt, Kristina Yasuda, Adam Lemmon, Tobias Looker. 2022.04. Status: Standards Track.

[[def: VC Data Model v1.1]]
~ [Verifiable Credentials Data Model v1.1](https://www.w3.org/TR/vc-data-model/). Manu Sporny, Dave Longley, David Chadwick. 2021.08. Status: W3C Proposed Recommendation.

[[def: JWT VC Presentation profile]]
~ [JWT VC Presentation Profile](https://identity.foundation/jwt-vc-presentation-profile/), Daniel McGrogan, Kristina Yasuda, Jen Schreiber

[[def: Presentation Exchange v1.0.0]]
~ [Presentation Exchange v1.0.0](https://identity.foundation/presentation-exchange/spec/v1.0.0/). Daniel Buchner, Brent Zundel, Martin Riedel.

[[def: did-web]]
~ [Web DID Method](https://github.com/w3c-ccg/did-method-web). Oliver Terbu, Mike Xu, Dmitri Zagidulin, Amy Guy. Status: Registered in DID Specification Registry.

[[def: did-ion]]
~ [ION DID Method](https://github.com/decentralized-identity/ion-did-method). Various DIF contributors. Status: Registered in DID Specification Registry.

[[def: OIDC Registration]]
~ [OpenID Connect Dynamic Client Registration 1.0 incorporating errata set 1](https://openid.net/specs/openid-connect-registration-1_0.html). Nat Sakimura, John Bradley, Michael B. Jones. 2014.11. Status: Approved Specification.

[[def: Sidetree]]
~ [Sidetree v1.0.0](https://identity.foundation/sidetree/spec/). Daniel Buchner, Orie Steele, Troy Ronda. 2021.03. Status: DIF Ratified Specification.

[[def: Well Known DID]]
~ [Well Known DID Configuration](https://identity.foundation/.well-known/resources/did-configuration/). Daniel Buchner, Orie Steele, Tobias Looker. 2021.01. Status: DIF Working Group Approved Draft.

[[def: Identity Hub (0.0.1 Predraft)]]
~ [Identity Hub - Decentralized Web Node 0.0.1 Predraft](https://identity.foundation/decentralized-web-node/spec/0.0.1-predraft/)

[[def: Status List 2021 (0.0.1 Predraft)]]
~ [Status List 2021 0.0.1 Predraft](https://github.com/w3c/vc-status-list-2021/releases/tag/v0.0.1). Manu Sporny, Dave Longley, Orie Steele, Mike Prorock, Mahmoud Alkhraishi. 2022.04. Status: Draft Community Group Report.

### Non-Normative References

[[def: JWP]]
~ [JSON Web Proof](https://github.com/json-web-proofs/json-web-proofs/blob/main/draft-jmiller-json-web-proof.md). Jeremie Miller, David Waite, Michael B. Jones. Status: Internet-Draft.

[[def: JPA]]
~ [JSON Proof Algorithms](https://github.com/json-web-proofs/json-web-proofs/blob/main/draft-jmiller-json-proof-algorithms.md). Jeremie Miller, Michael B. Jones. Status: Internet-Draft.

[[def: Decentralized Web Node]]
~ [Decentralized Web Node](https://identity.foundation/decentralized-web-node/spec/). Daniel Buchner, Tobias Looker. Status: Draft.

[[def: SD-JWT]]
~ [Selective Disclosure for JWTs (SD-JWT)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-selective-disclosure-jwt). Daniel Fett, Kristina Yasuda, Brian Campbell. Status: Internet-Draft in Web Authorization Protocol WG
