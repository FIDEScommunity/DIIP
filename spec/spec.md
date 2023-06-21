Dutch Decentralized Identity Profile v1
==================

**Profile Status:** Draft

**Latest Draft:**
[https://dutchblockchaincoalition.github.io/DDIP](https://dutchblockchaincoalition.github.io/DDIP)

Editors:
~ [Niels Klomp](https://www.linkedin.com/in/niels-klomp/) (Sphereon)
~ [Timo Glastra](https://www.linkedin.com/in/timoglastra/) (Animo Solutions)
~ [Maaike van Leuken](https://www.linkedin.com/in/maaike-van-leuken-0b1b7011a/) (TNO)

Contributors:
~ [TODO]() 

**Special Thanks:**

This profile is based on a lot of work done by the Decentralized Identity community, given this profile is largely based on and uses sections of the [DIF JWT VC Presentation Profile](https://identity.foundation/jwt-vc-presentation-profile/), we would like to place special thanks to the editors and contributors of that profile.

Participate:
~ [GitHub repo](https://github.com/DutchBlockchainCoalition/DDIP.git)
~ [File a bug](https://github.com/DutchBlockchainCoalition/DDIP.git/issues)
~ [Commit history](https://github.com/DutchBlockchainCoalition/DDIP.git/commits/main)

------------------------------------

## Abstract

The Dutch Decentralized Identity Profile, or DDIP for short, defines a set of requirements against existing specifications to enable the interoperable issuance and presentation of Verifiable Credentials (VCs) between Wallets and Verifiers.

This document is not a specification, but a **profile**. It outlines existing specifications required for implementations to interoperate among each other. It also clarifies mandatory to implement features for the optionalities mentioned in the referenced specifications. <<The main rationale for selecting these standards is easy adoption.>>

The profile uses OpenID for Verifiable Presentations ([[ref: OpenID4VP ID1]]) as the base protocol for the request and verification of W3C JWT VCs as W3C Verifiable Presentations ([[ref: VC Data Model v1.1]]). A full list of the open standards used in this profile can be found in [Overview of the Open Standards](#overview-of-the-open-standards).

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
<<More?>>
First, this profile outlines open standards required to be supported. Then, it describes detailed requirements for each specification.

## Terminology

This section consolidates in one place common terms used across open standards that this profile consists of. For the details of these, as well as other useful terms, see text within each of the specification listed in [[ref: References]].

[[def:Authorization Request]]
~ OAuth 2.0 Authorization Request extended by [[ref: OpenID4VP ID1]] and [[ref: SIOPv2 ID1]].

[[def:Authorization Response]]
~ OAuth 2.0 Authorization Response extended by [[ref: OpenID4VP ID1]] and [[ref: SIOPv2 ID1]].

[[def: Decentralized Identifier, DID]]
~ An identifier with its core ability being enabling Clients to obtain key material and other metadata by reference, defined in [[ref: DID Core]]. 

[[def: Holder]]
~ An entity that possesses or holds Verifiable Credentials and can generate Verifiable Presentations from them as defined in [[ref: VC Data Model v1.1]].

[[def:End User]]
~ Human Participant.

[[def: OpenID Provider (OP)]]
~ OAuth 2.0 Authentication Server implementing [[ref: OpenID Connect Core]] and [[ref: OpenID4VP ID1]]

[[def: Presentation]] 
~ Data derived from one or more Verifiable Credentials, issued by one or more issuers, that is shared with a Verifier.

[[def: Relying Party (RP)]]
~ OAuth 2.0 Client application using [[ref: OpenID Connect Core]] and [[ref: OpenID4VP ID1]] in [[ref: SIOPv2 ID1]]. Synonymous with term Verifier.

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
~ An entity that receives, stores, presents, and manages credentials and key material of the End User. During presentation of VP(s) using [[ref: OpenID4VP ID1]], the Wallet acts as an OAuth 2.0 Authorization Server towards the Verifier that is acting as an OAuth 2.0 Client. During user authentication using [[ref: SIOPv2 ID1]], the Wallet acts as a Self Issued OpenID Provider towards the Verifier that is a specific case of the Relying Party in [[ref: OpenID Connect Core]]. 

## Profile

### Overview of the Open Standards

- [[ref: OID4VCI 1.0]]
- [[ref: OpenID4VP ID1]]
- [[ref: SIOPv2 ID1]]
- [[ref: Presentation Exchange v1.0.0]]
- W3C Verifiable Credentials JWT ([[ref: VC Data Model v1.1]])
- [[ref: did-web]] and [[ref: did-jwk]] 
- ES256
- [[ref: Status List 2021 (First public draft)]]

Rationale behind the profile is to increase adoption through making the profile as accessible as possible. DDIP allows for the implementation of simple Decentralized Identity use cases, with less effort to implement the profile as opposed to other profiles. This boils down to using technologies that are well-specified and already have wide adoption. In the section below, we describe the design choices.


<For each choice: why?>

### No Blockchain

### Signature Scheme
When working with JWTs, only two signature algorithms can be used: ES256 and RS256. The first is based on the elliptic curve discrete logarithm problem, whereas the latter is based on the integer factorization problem. Elliptic-curve cryptography can achieve the same security as RSA with much shorter keys. Therefore DDIP supports ES256 (ECDSA using P-256 and SHA-256). 

 <!--ES256 is short for ECDSA using P-256 and SHA-256.  -->

<!-- ### Requirements (?) -->

### Credential Format
We wanted to work with the broadly known VC model. Proof formats that are actively being used to issue verifiable credentials according to the VC model are:
- JWT-VC. External proof. JWT-VC has a high Technology Readiness Level (TRL) and are widely adopted in other existing standards. Compatible encoding schemes are JSON and JSON-LD. DDIP v1 requires JSON, in future versions JSON-LD will also be supported.
<!-- Do we make a choice in JSON / JSON-LD? -->
- LDP-VC. Embedded proof, proof is included in the data.

For more information, see [[ref: VC Data Model v1.1]].

### Revocation Algorithm

Status List 2021 is a bit string, where each credential has a position in the list. Based on the value of the bit, the credential is either revoked or not.

## Issuance
TODO: For issuance of Verifiable Credentials...
The issuance of credentials from the Issuer to the Holder's Wallet is done along the [[ref: OID4VCI 1.0]] specification.

### OID4VCI
<!-- OID4VCI can either be used standalone, or with OAuth2 / OIDC IDP -->

## Presentation
The presentation of claims from the Holder's Wallet to the Verifier is done along the [[ref: OpenID4VP ID1]] and [[ref: Presentation Exchange v1.0.0]] specifications. For DDIP we follow the [[ref: JWT VC Presentation Profile]], as this profile is supported by big-tech companies, but with some changes. The profile and the changes are explained below.
<!-- Why do we follow this profile? -->

<!-- SIOPv2 + OID4VP + PE -->

### DID Methods
Implementers are required to support did:web and did:jwk. It does not require mandatory support for did:ion, unlike the [[ref: JWT VC Presentation Profile]] which we use as a basis for the Presentation.

#### did:web Method
Support for the [[ref: did-web]] as mentioned in the [[ref: JWT VC Presentation Profile]] is required. did:web supports key rotation, but not key history.

#### Removal of did:ion Method
<!-- Why didn't we want to include blockchain? -->
We decided to exclude blockchains from DDIP v1, as Since we didn't want to include a blockchain-based DID method in DDIP v1, we do not require [[ref: did-ion]] support in DDIP, contrary to the [[ref: JWT VC Presentation Profile]]. Of course implementations are free to support the [[ref: did-ion]]. One of the drawbacks of course is that this means that key history as well as rotations are not really supported in an interoperable way. This is mostly a problem for organizations, since Natural Persons would never use ledger-based DID methods anyway.

#### Addition of did:jwk Method
We do support the [[ref: did-jwk]], given this DID method is simply an encoding of a JSON Web Key. As such it also supports X.509 certificates. This is a very common way to encode keys and certificates in current solutions, thus we believe it is important to support this method. A did:jwk can either have a Certificate Chain incorporated (x5c) in the DID Document or linked as a URL (x5u). 

#### did:key Method
TODO: EBSI now is using JCS with did:key for NP. It makes sense that we once again favor did:key over did:web. It does mean we would lose the ability to use X509 certificates, as did:key can only handle RSA keys and no certificates.

### SIOP

<!-- SIOPv2 -->

### OID4VP
<!-- OID4VP can either be used icw SIOPv2 or with OIDC. -->

### Linked Domain Verification is fully optional
Contrary to the [[ref: JWT VC Presentation Profile]], the use of [[ref: Linked Domain Verification]] is fully optional. If not present for a party, the other party should not raise an error. Although we believe Linked Domains are a nice optional trust anchor, we also wanted to keep the DDIP v1 profile as Simple as possible at this point in time. Focusing on technical interoperability first and governance interoperability later.

## Security Considerations

The same security consideration applies to DDIP as to the JWT-VC Presentation Profile. It is important to note that Cross-device SIOP is susceptible to a session phishing attack, where an attacker relays the request from a good Verifier/RP to a victim and is able to sign in as a victim. Implementers MUST implement mitigations most suitable to the use-case. For more details and concrete mitigations, see section 15 Security Considerations in [[ref: SIOPv2 ID1]].

### Crypto Agility

JWT-VC provides crypto agility, which allows for replacing the signature scheme when desired. Crypto agility is important for the migration to post-quantum signature schemes.

## Privacy Considerations

### Selective Disclosure

JWT-VC does not allow for selective disclosure. 

### Predicates

JWT-VC does not allow for generating predicates. 

### Issuer Unlinkability

### Verifier Unlinkability

### Uncorrelateability

Verifier unlinkability is not achieved, as the signature scheme ECDSA is not capable of creating unlinkable signatures such that colluding verifiers can link verification processes together.

### Observability
The verifier has the possibility to observe the revocation status beyond the presentation, because they know the position of the credential in the bitstring.

## Use-Cases

TBD

## Examples

<!-- Examples are listed inline in above sections as well as in complete form within [Test Vectors](#test-vectors). -->

## Implementations

At time of writing, two wallets are compatible with DDIP, namely the Sphereon and Animo wallet.

## Testing
TBD

## Test Vectors
TBD

## References

### Normative References

[[def: OID4VCI 1.0]]
~ [OpenID for Verifiable Credential Issuance 1.0](https://openid.bitbucket.io/connect/openid-4-verifiable-credential-issuance-1_0.html). Torsten Lodderstedt, Kristina Yasuda, Tobias Looker. 2023.06.

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
~ [JWT VC Presentation Profile](https://identity.foundation/jwt-vc-presentation-profile/). Daniel McGrogan, Kristina Yasuda, Jen Schreiber.

[[def: Presentation Exchange v1.0.0]]
~ [Presentation Exchange v1.0.0](https://identity.foundation/presentation-exchange/spec/v1.0.0/). Daniel Buchner, Brent Zundel, Martin Riedel.

[[def: did-web]]
~ [did:web Method](https://github.com/w3c-ccg/did-method-web). Oliver Terbu, Mike Xu, Dmitri Zagidulin, Amy Guy. Status: Registered in DID Specification Registry.

[[def: did-jwk]]
~ [did:jwk Method](https://github.com/quartzjer/did-jwk/blob/main/spec.md). Status: Registered in DID Specification Registry.

[[def: Status List 2021 (First public draft)]]
~ [Status List 2021 0.0.1 Predraft](https://www.w3.org/TR/vc-status-list/). Manu Sporny, Dave Longley, Orie Steele, Mike Prorock, Mahmoud Alkhraishi. 2022.04. Status: Draft Community Group Report.

### Non-Normative References

[[def: did-ion]]
~ [did:ion Method](https://github.com/decentralized-identity/ion). 

[[def: JWP]]
~ [JSON Web Proof](https://github.com/json-web-proofs/json-web-proofs/blob/main/draft-jmiller-json-web-proof.md). Jeremie Miller, David Waite, Michael B. Jones. Status: Internet-Draft.

[[def: JPA]]
~ [JSON Proof Algorithms](https://github.com/json-web-proofs/json-web-proofs/blob/main/draft-jmiller-json-proof-algorithms.md). Jeremie Miller, Michael B. Jones. Status: Internet-Draft.

<!-- [[def: SD-JWT]]
~ [Selective Disclosure for JWTs (SD-JWT)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-selective-disclosure-jwt). Daniel Fett, Kristina Yasuda, Brian Campbell. Status: Internet-Draft in Web Authorization Protocol WG -->

[[def: OIDC Registration]]
~ [OpenID Connect Dynamic Client Registration 1.0 incorporating errata set 1](https://openid.net/specs/openid-connect-registration-1_0.html). Nat Sakimura, John Bradley, Michael B. Jones. 2014.11. Status: Approved Specification.

[[def: Well Known DID]]
~ [Well Known DID Configuration](https://identity.foundation/.well-known/resources/did-configuration/). Daniel Buchner, Orie Steele, Tobias Looker. 2021.01. Status: DIF Working Group Approved Draft.

[[def: Linked Domain Verification]]
~ [JWT-VC Presentation Profile Linked Domain Verification](https://identity.foundation/jwt-vc-presentation-profile/#linked-domain-verification). Daniel McGrogan, Kristina Yasuda, Jen Schreiber.
