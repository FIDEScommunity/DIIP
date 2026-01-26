# Decentralized Identity Interop Profile

**Profile Status:** draft v6

**Latest Release:**
[https://FIDEScommunity.github.io/DIIP](https://FIDEScommunity.github.io/DIIP)

**Latest Draft:**
[https://FIDEScommunity.github.io/DIIP/draft](https://FIDEScommunity.github.io/DIIP/draft)

**Previous Releases:**
[v4](v4.html),
[v5](v5.html)

Editors:
~ [Eelco Klaver](https://www.linkedin.com/in/eklaver/) (Credenco)
~ [Harmen van der Kooij](https://www.linkedin.com/in/harmenvanderkooij/) (FIDES Labs)
~ [Niels Klomp](https://www.linkedin.com/in/niels-klomp/) (4Sure Technology Solutions)
~ [Niels van Dijk](https://www.linkedin.com/in/creativethings/) (SURF)
~ [Samuel Rinnetmäki](https://www.linkedin.com/in/samuel/) (Findynet)
~ [Timo Glastra](https://www.linkedin.com/in/timoglastra/) (Animo Solutions)

Contributors and previous editors:
~ [Adam Eunson](https://www.linkedin.com/in/adameunson/) (Auvo)
~ [Jelle Millenaar](https://www.linkedin.com/in/jellefm/) (Impierce Technologies)
~ [Maaike van Leuken](https://www.linkedin.com/in/maaike-van-leuken-0b1b7011a/) (TNO)
~ [Thierry Thevenet](https://www.linkedin.com/in/thierrythevenet/) (Talao)

------------------------------------

## Abstract

The Decentralized Identity Interop Profile, or DIIP for short, defines requirements against existing specifications to enable the interoperable issuance and presentation of [[ref: Digital Credential]]s between [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s.

| Purpose                                                                  | Specification                                                                                  |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| Credential format                                                        | [[ref: W3C VCDM]] 2.0 (20 March 2025) and [[ref: SD-JWT VC]] (draft 13)                        |
| Signature scheme                                                         | SD-JWT as specified in [[ref: VC-JOSE-COSE]] (15 May 2025) and [[ref: SD-JWT VC]] (draft 13)   |
| Signature algorithm                                                      | [[ref: ES256]] (RFC 7518 May 2015)                                                             |
| Identifying [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s   | [[ref: did:jwk]] (Commit 8137ac4, Apr 14 2022) and [[ref: did:web]] (31 July 2024)             |
| Issuance protocol                                                        | OpenID for Verifiable Credentials Issuance 1.0 ([[ref: OID4VCI]]) (Final).                     |
| Presentation protocol                                                    | OpenID for Verifiable Presentations 1.0 ([[ref: OID4VP]]) (Final)                              |
| Revocation mechanism                                                     | [[ref: IETF Token Status List]] (Draft 15, 2026-01-08)                                         |
| Trust Establishment (optional)                                           | [[ref: OpenID Fed DCP]] (Appendix B of this profile)                                           |

The [Normative References](#normative-references) section links to the versions of specifications that DIIP-compliant implementations must support.

This document is not a specification but a **profile**. It outlines existing specifications required for implementations to interoperate with each other.
It also clarifies mandatory features for the options mentioned in the referenced specifications.

The main objective of this profile is to allow for easy adoption and use the minimum amount of functionality for a working [[ref: Digital Credential]] ecosystem.

### Status of this Document

The Decentralized Identity Interop Profile v6 is a FIDES Community draft.

The latest published DIIP profile can be found at [https://FIDEScommunity.github.io/DIIP/](https://FIDEScommunity.github.io/DIIP/). The latest working group draft can be found at [https://FIDEScommunity.github.io/DIIP/draft](https://FIDEScommunity.github.io/DIIP/draft).

### Audience

The audience of this document includes organisations aiming to issue or verify [[ref: Digital Credential]]s, as well as the implementers of [[ref: Digital Credential]] solutions ([[ref: Wallet]]s and [[ref: Agent]]s).

### Development of the DIIP Profile

Participate:
~ [GitHub repo](https://github.com/FIDEScommunity/DIIP.git)
~ [File a bug](https://github.com/FIDEScommunity/DIIP/issues)
~ [Commit history](https://github.com/FIDEScommunity/DIIP/commits/main)

The development of this interoperability profile is a collaborative process. Anyone can suggest new specifications and restrictions. The suggestions are reviewed by the community, and decisions are made through discussions.

Feel free to join the [FIDES Community Discord](https://discord.gg/dSNbNadE6W) to participate in the discussions.

There are also monthly DIIP meetings. Contact <a href="mailto:harmen@fides.community">Harmen van der Kooij</a> if you want to be invited to the meetings.

The authors intend to release new versions of the DIIP profile twice a year.

Some plans and ideas for the next version are documented in the [Appendix A: Future Directions](#appendix-a-future-directions).

## Structure of this Document

The [Goals](#goals) section explains the design  of the DIIP profile.

The [Profile](#profile) section defines the requirements for compliant solutions and explains the choices.

The [References](#references) section defines the specifications and their versions.

The [Terminology](#terminology) section explains the key terms used in this profile.

## Goals

The [[ref: W3C VCDM]] specification defines a data model for [[ref: Digital Credential]]s but does not prescribe standards for transport protocol, key management, authentication, query language, etc.

The [[ref: OID4VCI]] and [[ref: OID4VP]] protocols define the interaction between [[ref: Wallet]]s and [[ref: Agent]]s but don't specify a data model or a credential format.

This interoperability profile makes selections by combining a set of specifications. It chooses standards for credential format, signature algorithm, identifying actors, and issuance and presentation protocols. Instead of saying, "*We use [[ref: W3C VCDM]] credentials signed with [[ref: VC-JOSE-COSE]] using [[ref: ES256]] as the signature algorithm, [[ref: OID4VCI]] as the issuance protocol, and [[ref: OID4VP]] as the presentation protocol, and [[ref: OpenID Federation]] for trust establishment*", you can just say, "*We use DIIP v6*".

In addition, the DIIP profile makes selections *within* the specifications. When a standard allows multiple ways of implementing something, DIIP makes one of those ways mandatory. As an implementer, you don't need to fully support all specifications to be DIIP-compliant. DIIP makes these choices to accelerate adoption and interoperability – defining the minimum required functionality.

DIIP does not exclude anything. For example, when DIIP says that compliant implementations MUST support [[ref: did:jwk]] as an identifier of the [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s, it doesn't say that other identifiers cannot be used. The [[ref: Wallet]]s and [[ref: Agent]]s can support other identifiers as well and still be DIIP-compliant.

Trust ecosystems can also easily extend DIIP by saying, "We use the DIIP v6 profile *and allow `mDocs` as an additional credential format*". They can also switch requirements by saying, "We use the DIIP v6 profile *but use [[ref: VC-DATA-INTEGRITY]] as an embedded proof mechanism*".

The design goal for DIIP is to ensure interoperability between [[ref: Wallet]]s and [[ref: Agent]]s in cases where device binding of [[ref: Digital Credential]]s is not required and the [[ref: Wallet]] doesn't need to be trusted. Issuing, holding, and presenting certifications, diplomas, licenses, permits, etc., fit into the scope of DIIP. Using a [[ref: Wallet]] for strong customer authentication or for sharing Person Identification Data (PID) is out of DIIP's scope, and you should look into [[ref: HAIP]] instead.

### Relationship to eIDAS Regulation and HAIP Profile

The DIIP profile is intended to be complementary to the OpenID4VC High Assurance Interoperability Profile ([[ref: HAIP]]). Both profiles build on the OpenID for Verifiable Credentials specifications ([[ref: OID4VCI]] and [[ref: OID4VP]]), but they address different interoperability scopes and assurance needs.

DIIP is an interoperability profile that includes the W3C Decentralized Identifiers specification ([[ref: DID Core]]) and the Verifiable Credentials Data Model 2.0 ([[ref: W3C VCDM]]). These specifications are not part of the HAIP interoperability profile.

DIIP explicitly targets multi-party interoperability scenarios in cross-domain and global ecosystems. These scenarios require identifier-agnostic and Linked Data–based interoperability, with a focus on business wallets, rather than personal wallets only. This makes DIIP particularly relevant for ecosystems such as [Gaia-X](https://gaia-x.eu/), [Catena-X](https://catena-x.net/), [IDSA](https://internationaldataspaces.org/) and [SIMPL](https://simpl-programme.ec.europa.eu/) (Data Spaces), [UNTP](https://untp.unece.org/) and [CIRPASS-2](https://cirpass2.eu/) (Digital Product Passports), [MOSIP](https://www.mosip.io/) (Digital Public Infrastructure), [EU CitiVERSE](https://www.cu-project.eu/citiverse) (Local Digital Twins), [Open Badges](https://www.1edtech.org/standards/open-badges) (Skills/Certificates/Diplomas), and the [Swiss E-ID](https://www.eid.admin.ch/en/e-id-e) Ecosystem.

In the European context, [[ref: HAIP]] is typically used for high-assurance use cases, including those involving Qualified Electronic Attestations of Attributes (*QEAA*). DIIP can be applied to use cases with lower assurance requirements, for example where Electronic Attestations of Attributes (*EAA*) are sufficient, or where device binding or qualified attestation is not required, while still enabling interoperability based on the same foundations of [[ref: OID4VCI]], [[ref: OID4VP]], and [[ref: SD-JWT VC]].

While DIIP is a standalone interoperability profile and enables interoperability on its own, it is designed to coexist with and complement [[ref: HAIP]]. Wallets and agents may therefore support both profiles, selecting the appropriate one based on the assurance requirements of a given use case.

Wallet providers are encouraged to implement both [[ref: HAIP]] and DIIP, enabling them to support a broad range of assurance levels, use cases, and ecosystems using a common  foundation.

## Profile

In this section, we describe the interoperability profile.

### Credential Format

The W3C Verifiable Credential Data Model ([[ref: W3C VCDM]]) defines structure and vocabulary well suited for [[ref: Digital Credential]]s in DIIP's scope. For example, the [[ref: Open Badges 3]] credentials use [[ref: W3C VCDM]] as the data format.

The SD-JWT-based Verifiable Credentials specification ([[ref: SD-JWT VC]]) defines a credential format that are serialized in JSON Web Tokens ([[ref: JWT]]s) and enable selective disclosure. [[ref: SD-JWT VC]] is used as a credential format for person identification data (PID) in [[ref: HAIP]] and [[ref: ARF]] (in addition to `mDocs`).

[[ref: W3C VCDM]] recommends securing Verifiable Credentials using JOSE and COSE ([[ref: VC-JOSE-COSE]]) as an *enveloping proof* mechanism and Verifiable Credential Data Integrity 1.0 ([[ref: VC-DATA-INTEGRITY]]) as an *embedded proof* mechanism.

To keep things as simple as possible, DIIP requires implementations to use `SD-JWT` as the mechanism to secure also [[ref: W3C VCDM]]-based credentials.

**Requirement: DIIP-compliant implementations MUST support [[ref: SD-JWT VC]] as a credential format.**

**Requirement: DIIP-compliant implementations MUST support [[ref: W3C VCDM]] and more specifically [Securing JSON-LD Verifiable Credentials with SD-JWT](https://www.w3.org/TR/vc-jose-cose/#secure-with-sd-jwt) as specified in ([[ref: VC-JOSE-COSE]]).**

### Signature Algorithm

The DIIP profile chooses one key type [[ref: Secp256r1]] and one signature method [[ref: ES256]] that all implementations must support.

**Requirement: DIIP-compliant implementations MUST support [[ref: ES256]] (`ECDSA` using [[ref: Secp256r1]] curve and `SHA-256` message digest algorithm).**

### Identifiers

DIIP prefers decentralized identifiers ([[ref: DID]]s) as identifiers. An entity identified by a [[ref: DID]] publishes a [DID Document](https://www.w3.org/TR/did-1.0/#dfn-did-documents), which can contain useful metadata about the entity, e.g., various endpoints. There are many DID methods defined. The DIIP profile requires support for two of them: [[ref: did:jwk]] and [[ref: did:web]]. In many use cases, organizations are identified by [[ref: did:web]], and the natural persons are identified by [[ref: did:jwk]].

**Requirement: DIIP-compliant implementations MUST support [[ref: did:jwk]] and [[ref: did:web]] as the identifiers of the [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s.**

### Trust Establishment

Signatures in [[ref: Digital Credential]]s can be used to verify that the content of a credential has not been tampered with. But anyone can sign a credential and put anything in the issuer field. [[ref: Digital Credential]] ecosystems require that there is a way for a [[ref: Verifier]] to check who is the [[ref: Issuer]] of a [[ref: Digital Credential]]. Equally, a user might want to be informed about the trustworthiness of a [[ref: Verifier]] before choosing to share credentials.

DIIP enables trust ecosystems to use [[ref: OpenID Fed DCP]] – a light-weight profile of the [[ref: OpenID Federation]], authored for use cases including [[ref: Agent]]s, [[ref: Wallet]]s and [[ref: Digital Credential]]s. The [[ref: OpenID Fed DCP]] specification is an appendix to this version of the DIIP profile. In the future, the [[ref: OpenID Fed DCP]] profile will probably be donated to be maintained elsewhere.

**Requirement: DIIP-compliant [[ref: Issuer]] [[ref: Agent]]s MUST support publishing OpenID Federation Entity Configuration as defined in [[ref: OpenID Fed DCP]].**

**Requirement: DIIP-compliant [[ref: Issuer]] [[ref: Agent]]s MUST support issuing the `fed` claim as defined in [[ref: OpenID Fed DCP]] when issuing [[ref: SD-JWT VC]] credentials.**

**Requirement: DIIP-compliant [[ref: Issuer]] [[ref: Agent]]s MUST support issuing the `termsOfUse` attribute as defined in [[ref: OpenID Fed DCP]] when issuing [[ref: W3C VCDM]] credentials.**

**Requirement: DIIP-compliant [[ref: Verifier]] [[ref: Agent]]s MUST support publishing OpenID Federation Entity Configuration as defined in [[ref: OpenID Fed DCP]].**

### Issuance

The issuance of [[ref: Digital Credential]]s from the [[ref: Issuer]] to the [[ref: Holder]]'s [[ref: Wallet]] is done along the [[ref: OID4VCI]] specification. Other protocols exist, but [[ref: OID4VCI]] is very broadly supported and also required by [[ref: HAIP]].

#### OID4VCI

OpenID for Verifiable Credential Issuance ([[ref: OID4VCI]]) defines an API for the issuance of [[ref: Digital Credential]]s.
OID4VCI [issuance flow variations](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID2.html#name-issuance-flow-variations) leave room for optionality.

In many situations, [[ref: Digital Credential]]s are issued on the [[ref: Issuer]]'s online service (website). This online service may have already authenticated and authorized the user before displaying the credential offer. Another authentication or authorization is not needed in those situations.

Authorization Code Flow provides a more advanced way of implementing credential issuance. Proof Key for Code Exchange ([[ref: PKCE]]) defines a way to mitigate against authorization code interception attack. Pushed authorization request ([[ref: PAR]]) allows clients to push the payload of an authorization request directly to the authorization server. These features may be needed in higher assurance use cases or for protecting privacy.

**Requirement: DIIP-compliant implementations MUST support both *Pre-Authorized Code Flow* and *Authorization Code Flow*.**

**Requirement: DIIP-compliant implementations MUST support the `tx_code` when using *Pre-Authorized Code Flow*.**

**Requirement: DIIP-compliant [[ref: Wallet]]s MUST NOT assume the Authorization Server is on the same domain as the [[ref: Issuer]].**

**Requirement: DIIP-compliant implementations MUST support [[ref: PKCE]] with Code Challenge Method Parameter `S256` to prevent authorization code interception attacks.**

**Requirement: DIIP-compliant implementations MUST support [[ref: PAR]] with the [[ref: Issuer]]'s Authorization Server using `require_pushed_authorization_requests` set to `true` ensuring integrity and authenticity of the authorization request.**

It should be noted that various [Security Considerations](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-pre-authorized-code-flow-2) have been described in the [[ref: OID4VCI]] specification with respect to implementing *Pre-Authorized Code Flow*. Parties implementing DIIP are strongly suggested to implement mitigating measures, like the use of a Transaction Code.

[[ref: OID4VCI]] defines *Wallet-initiated* and *Issuer-initiated* flows. *Wallet-initiated* means that the [[ref: Wallet]] can start the flow without any activity from the [[ref: Issuer]]. The *Issuer-initiated* flow seems to be more common in many use cases and seems to be supported more widely. It also aligns better with the use cases where the [[ref: Holder]] is authenticated and authorized in an online service before the credential offer is created and shown.

**Requirement: DIIP-compliant implementations MUST support the *Issuer-initiated* flow.**

[[ref: OID4VCI]] defines *Same-device* and *Cross-device* Credential Offer. People should be able to use both their desktop browser and their mobile device's browser when interacting with the [[ref: Issuer]]'s online service.

**Requirement: DIIP-compliant implementations MUST support both *Same-device* and *Cross-device* Credential Offer.**

[[ref: OID4VCI]] defines *Immediate* and *Deferred* flows. *Deferred* is more complex to implement and not required in most use cases.

**Requirement: DIIP-compliant implementations MUST support the *Immediate* flow.**

[[ref: OID4VCI]] states that there are two possible methods for requesting the issuance of a specific credential type in an *Authorization Request*: either by utilizing the `authorization_details` parameter or by utilizing the `scope` parameter.

The `scope` parameter is a light-weight way of using an external authorization server. The `authorization_details` makes the flow much more configurable and structured. If an issuer agent does not support an external authorization server, the scope parameter is not needed.

**Requirement: DIIP-compliant [[ref: Wallet]]s MUST support the `authorization_details` parameter using the `credential_configuration_id` parameter in the Authorization Request.**

**Requirement: DIIP-compliant [[ref: Wallet]]s MUST support the `scope` parameter in the Authorization Request.**

**Requirement: DIIP-compliant [[ref: Issuer]] [[ref: Agent]]s MUST support the `authorization_details` parameter in the Authorization Request.**

[[ref: OID4VCI]] defines proof types `jwt`, `ldp_vp`, and `attestation` for binding the issued credential to the identifier of the end-user possessing that credential. DIIP requires compliant implementations to support [[ref: did:jwk]] as an identifier. Thus, in cases where cryptographic holder-binding is needed, implementations should be able to bind a credential to the [[ref: Holder]]'s [[ref: did:jwk]].

**Requirement: DIIP-compliant implementations MUST support the `jwt` proof type with a [[ref: did:jwk]] or [[ref: did:web]] as the `iss` value and use a `kid` from the `assertionMethod` Verification Method relationship of the respective [[ref: Issuer]]'s [[ref: DID]] document.**

**Requirement: DIIP-compliant implementations MUST support a `cnf` holder binding claim in the [[ref: Issuer]]'s `jwt` and it MUST include a `kid` value from the `authentication` Verification Method relationship of the respective [[ref: Holder]]'s [[ref: DID]] document.**

### Presentation

The presentation of claims from the [[ref: Holder]]'s [[ref: Wallet]] to the [[ref: Verifier]] is done along the [[ref: OID4VP]]. Other protocols exist, but [[ref: OID4VP]] is very broadly supported and also required by [[ref: HAIP]].

#### OID4VP

Using [[ref: OID4VP]], the [[ref: Holder]]s can also present cryptographically verifiable claims issued by third-party [[ref: Issuer]]s, such that the [[ref: Verifier]] can place trust in those [[ref: Issuer]]s instead of the subject ([[ref: Holder]]).

[[ref: OID4VP]] supports scenarios where the *Authorization Request* is sent both when the [[ref: Verifier]] is interacting with the [[ref: Holder]] using the device that is the same or different from the device on which requested [[ref: Digital Credential]]s are stored.

**Requirement: DIIP-compliant implementations MUST support both *Same-device Flow* and *Cross-device Flow*.**

According to [[ref: OID4VP]], the [[ref: Verifier]] may send an *Authorization Request* using either of these 3 options:

- Passing as URL with encoded parameters
- Passing a request object as value
- Passing a request object by reference

DIIP only requires support for the last option.
**Requirement: DIIP-compliant implementations MUST support passing the *Authorization Request* object by reference.**

[[ref: OID4VP]] defines two values for the `request_uri_method` in the *Authorization Request*: `get` and `post`. DIIP requires support for only the `get` method.

**Requirement: DIIP-compliant implementations MUST support the `get` value for the `request_uri_method` in the *Authorization Request*.**

[[ref: OID4VP]] defines many [Client Identifier Schemes](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html#name-defined-client-identifier-s). One way to identify [[ref: Verifier]]s is through [[ref: OpenID Federation]]. Since DIIP uses [[ref: DID]]s, it is natural to require support for the corresponding Client Identifier Scheme.

**Requirement: DIIP-compliant implementations MUST support the `did` *Client Identifier Scheme*.**

The following features of [[ref: OID4VP]] are **not** required by this version of the DIIP profile:

- Presentations Without Holder Binding Proofs (section 5.3, requirements for the `state` parameter)
- Verifier Attestations (section 5.11)
- SIOPv2 (section 8, *Response Type* value `vp_token id_token` and `scope` containing `openid`)
- Encrypted Responses (section 8.3)
- Transaction Data (section 8.4)
- Digital Credentials API (Appendix A)

### Validity and Revocation Algorithm

Expiration algorithms using [validFrom](https://www.w3.org/TR/vc-data-model-2.0/#defn-validFrom) and [validUntil](https://www.w3.org/TR/vc-data-model-2.0/#defn-validUntil) are a powerful mechanism to establish the validity of credentials. Evaluating the expiration of a credential is much more efficient than using revocation mechanisms. While the absence of `validFrom` and `validUntil` would suggest a credential is considered valid indefinitely, it is recommended that all implementations set validity expiration whenever possible to allow for clear communication to [[ref: Holder]]s and [[ref: Verifier]]s.

**Requirement: DIIP-compliant implementations MUST support checking the validity status of a [[ref: Digital Credential]] using `validFrom` and `validUntil` when they are specified.**

The [[ref: IETF Token Status List]] defines a mechanism, data structures, and processing rules for representing the status of [[ref: Digital Credential]]s (and other "Tokens"). The statuses of Tokens are conveyed via a bit array in the Status List. The Status List is embedded in a Status List Token.

The [[ref: Bitstring Status List]] is based on the same idea as the [[ref: IETF Token Status List]] and is simpler to implement since it doesn't require signing of the status list. The [[ref: IETF Token Status List]] may gain more support since it is recommended by [[ref: HAIP]].

**Requirement: DIIP-compliant implementations MUST support [[ref: IETF Token Status List]] as a status list mechanism.**

## Terminology

This section consolidates in one place common terms used across open standards that this profile consists of. For the details of these, as well as other useful terms, see the text within each of the specifications listed in [References](#references).

[[def: Agent]]
~ A software application or component that an [[ref: Issuer]] uses to issue [[ref: Digital Credential]]s or that a [[ref: Verifier]] uses to request and verify them.

[[def: Holder]]
~ An entity that possesses or holds [[ref: Digital Credential]]s and can present them to [[ref: Verifier]]s.

[[def: DID]]
~ Decentralized Identifier as defined in [[ref: DID Core]].

[[def: Issuer]]
~ A role an entity can perform by asserting claims about one or more subjects, creating a [[ref: Digital Credential]] from these claims, and transmitting the [[ref: Digital Credential]] to a [[ref: Holder]], as defined in [[ref: W3C VCDM]].

[[def: Digital Credential]]
~ A set of one or more Claims made by an [[ref: Issuer]] that is tamper-evident and has authorship that can be cryptographically verified.

<!--
[[def: Relying Party]]
~ See [[ref: Verifier]].
-->

<!--
[[def: Verifiable Presentation]]
~ A Presentation that is tamper-evident and has authorship that can be cryptographically verified.
-->

[[def: Verifier]]
~ An entity that requests and receives one or more [[ref: Digital Credential]]s for processing.

[[def: Wallet]]
~ A software application or component that receives, stores, presents, and manages credentials and key material of an entity.

## References

### Normative References

[[def: DID Core]]
~ [Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/did-1.0/). Status: W3C Recommendation.

[[def: did:jwk]]
~ [did:jwk Method Specification](https://github.com/quartzjer/did-jwk/blob/main/spec.md). Status: Draft.

[[def: did:web]]
~ [did:web Method Specification](https://w3c-ccg.github.io/did-method-web/). Status: Unofficial working group draft.

[[def: ES256]]
~ `ECDSA` using `P-256` ([[ref: Secp256r1]]) and `SHA-256` as specified in [RFC 7518 JSON Web Algorithms (JWA)](https://datatracker.ietf.org/doc/html/rfc7518). Status: RFC - Proposed Standard.

[[def: IETF Token Status List]]
~ [Token Status List - draft 15](https://datatracker.ietf.org/doc/draft-ietf-oauth-status-list/15/). Status: Internet-Draft.

[[def: OID4VCI]]
~ [OpenID for Verifiable Credential Issuance 1.0](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html). Status: Final.

[[def: OID4VP]]
~ [OpenID for Verifiable Presentations 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html). Status: Final.

[[def: OpenID Federation]]
~ [OpenID Federation 1.0 - draft 42](https://openid.net/specs/openid-federation-1_0.html). Status: draft.

[[def: OpenID Fed DCP]]
~ [OpenID Federation Digital Credentials Profile - draft 0.1.0](#appendix-b-openid-federation-digital-credentials-profile). Status: draft.

[[def: PAR]]
~ [RFC 9126 Pushed Authorization Requests](https://datatracker.ietf.org/doc/html/rfc9126). Status: RFC - Proposed Standard.

[[def: PKCE]]
~ [RFC 7636 Proof Key for Code Exchange by OAuth Public Clients](https://datatracker.ietf.org/doc/html/rfc7636). Status: RFC - Proposed Standard.

[[def: SD-JWT VC]]
~ [SD-JWT-based Verifiable Credentials (SD-JWT VC) - draft 13](hhttps://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/13/). Status: WG Document.

[[def: Secp256r1]]
~ `Secp256r1` curve in [RFC 5480 ECC SubjectPublicKeyInfo Format](https://datatracker.ietf.org/doc/html/rfc5480). Status: RFC - Proposed Standard.
~ This curve is called `P-256` in [RFC 7518 JSON Web Algorithms (JWA)](https://datatracker.ietf.org/doc/html/rfc7518). Status: RFC - Proposed Standard.

[[def: W3C VCDM]]
~ [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/). Status: W3C Recommendation.

[[def: VC-JOSE-COSE]]
~ [Securing Verifiable Credentials using JOSE and COSE](https://www.w3.org/TR/2025/REC-vc-jose-cose-20250515/). Status: W3C Recommendation.

### Non-Normative References

[[def: ARF]]
~ [Architecture and Reference Framework](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/architecture-and-reference-framework-main/). Status: Draft.

[[def: Bitstring Status List]]
~ [Bitstring Status List v1.0](https://www.w3.org/TR/vc-bitstring-status-list/). Status: W3C Proposed Recommendation.

[[def: HAIP]]
~ [OpenID4VC High Assurance Interoperability Profile](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html). Status: Draft.

[[def: JWT]]
~ [RFC 7519 JSON Web Token (JWT)](https://datatracker.ietf.org/doc/html/rfc7519). Status: RFC - Proposed Standard.

[[def: Open Badges 3]]
~ [Open Badges Specification, Spec Version 3.0, Document Version 1.2](https://www.imsglobal.org/spec/ob/v3p0). Status: This document is made available for adoption by the public community at large.

[[def: VC-DATA-INTEGRITY]]
~ [Verifiable Credential Data Integrity 1.0](https://www.w3.org/TR/vc-data-integrity/). Status: W3C Proposed Recommendation.
