```markdown
Verifiable Dossiers
================================================

**Specification Status**: v0.4 Draft

**Latest Draft:**

[https://github.com/trustoverip/kswg-dossier-specification](https://github.com/trustoverip/kswg-dossier-specification)

**Author:**

- [Daniel Hardman](https://github.com/dhh1128), [Provenant](https://provenant.net/)

**Editors:**

**Contributors:**

**Participate:**

~ [GitHub repo](https://github.com/trustoverip/kswg-dossier-specification)
~ [Commit history](https://github.com/trustoverip/kswg-dossier-specification/commits/main)

**Abstract**

This document provides a normative definition for a dossier, a data structure for compiling and attesting to collections of verifiable evidence. A dossier is an Authentic Chained Data Container (ACDC) issued by the party assembling the evidence, functioning as a cryptographically verifiable affidavit rather than a traditional credential. It enables the creation of arbitrarily rich, tamper-evident data graphs of evidence. This specification defines the dossier's data model, its operational lifecycle of curation, citation, and verification, and its underlying security and privacy mechanisms, including graduated disclosure. It provides implementation guidance through detailed use cases in telecommunications, law enforcement, and investigative journalism.

[//]: # (\maketitle)

[//]: # (\newpage)

[//]: # (\toc)

[//]: # (\newpage)

[//]: # (::: forewordtitle)

## Introduction

[//]: # (:::)

### The Challenge of Verifiable Evidence Aggregation
In both the physical and digital realms, critical decisions are rarely based on a single piece of information. Instead, decision makers rely on a collection of disparate evidence, curated to form a coherent whole. A loan officer assembles a file of financial statements, credit reports, and employment verifications to assess creditworthiness; a law enforcement official compiles a case file containing forensic reports, witness statements, and crime scene documentation to build a case. The confidence of the decision depends not only on the validity of the individual pieces of evidence but also on the integrity of the collection itself.

In the digital world, this aggregation process is fraught with challenges. The core problem is the absence of a standardized, cryptographically secure method that aggregates diverse pieces of evidence, attests to the integrity of the collection, and manages its lifecycle in a decentralized and interoperable manner. Existing systems for evidence management are often siloed within proprietary platforms, dependent on centralized trusted parties, or lack the cryptographic guarantees necessary for high-assurance environments. This fragmentation creates friction, inhibits interoperability across domains (e.g., between different jurisdictions or industries), and introduces single points of failure that can be compromised or become unavailable.

### Introducing the Dossier: An Issuer-Centric Evidence Container
This specification introduces the dossier as a solution to these challenges. A dossier is formally defined as an Authentic Chained Data Container (ACDC) that references an arbitrarily rich collection of signed evidence and is issued by the party that assembles it. It is a container designed to create a verifiable data graph from evidentiary artifacts.

A critical distinction separates a dossier from a traditional verifiable credential. A credential typically makes an assertion about a specific subject, or issuee, conferring some right or attribute upon them. A dossier, by contrast, has no issuee. It has only an issuer—the entity that curates the collection. In this sense, a dossier functions more like a notarized affidavit than a passport; the issuer is making a formal, verifiable attestation about the composition and integrity of the evidence collection itself. This issuer-centric model represents a fundamental shift from traditional subject-centric identity paradigms.

The primary purpose of a dossier is to empower decisions that are likely to be made based on the attested collection of evidence it includes. This contrasts with systems that generate proofs just-in-time in response to a specific verifier query. By pre-assembling evidence into a stable, long-lived, and verifiable container, the dossier enables efficient and scalable proof presentation in a wide variety of contexts.

### Relationship to KERI, ACDC, and Verifiable Credentials
The dossier is not a standalone concept; it is deeply rooted in a stack of emerging open standards for decentralized identity. Its technical foundation is the Authentic Chained Data Containers (ACDC) specification, which defines a format for verifiable, chainable data structures.2 ACDCs, in turn, are secured by the Key Event Receipt Infrastructure (KERI), a protocol for decentralized key management that provides secure, rotatable, and auditable Autonomic Identifiers (AIDs).4 The canonical data representation is provided by the Composable Event Streaming Representation (CESR), an encoding format that ensures deterministic serialization for cryptographic operations.[[6]]

Within this ecosystem, it is important to distinguish the dossier from related concepts, including a conventional ACDC credential or a "bespoke ACDC". A bespoke ACDC may also contain custom links to evidence, but it is typically generated just-in-time to answer a specific verifier's query. A dossier, conversely, is a pre-curated collection intended for broader, repeated use. The emergence of the dossier signifies a maturation in the field of decentralized identity. The ecosystem is moving beyond simple, atomic claims about a subject (the purpose of a traditional credential) to support the creation of complex, curated narratives backed by a body of evidence. This reflects a more nuanced understanding of trust, recognizing that in the real world, assurance is often derived from a holistic evaluation of multiple, interrelated facts. This paradigm shift establishes a new and vital role within digital ecosystems: the "Evidence Curator." This role—which could be filled by an individual managing their own data, a lawyer building a case, a journalist protecting sources, or an automated service—is responsible for the assembly and attestation of evidence collections, and the dossier provides the formal data structure to support this function.

The following table clarifies these distinctions.

Feature | Dossier | ACDC Credential | Bespoke ACDC
--- | --- | --- | ---
Primary Role | Evidence Compilation | Assertion of Entitlement | Just-in-Time Proof
Recipient | No specific 'issuee' | Specific 'issuee' | Specific 'issuee'
Analogy | Affidavit / Case File | License / Passport | Custom-Generated Report
Creation Time | In advance of use | In advance of use | In response to a query
Content Focus | Graph of external evidence | Attributes of the issuee | Subset of existing evidence

## Status of This Memo

Information about the current status of this document, any errata,
and how to provide feedback on it, may be obtained at
[https://github.com/trustoverip/kswg-dossier-specification](https://github.com/trustoverip/kswg-dossier-specification).

## Copyright Notice

This specification is subject to the **OWF Contributor License Agreement 1.0 - Copyright**
available at
[https://www.openwebfoundation.org/the-agreements/the-owf-1-0-agreements-granted-claims/owf-contributor-license-agreement-1-0-copyright](https://www.openwebfoundation.org/the-agreements/the-owf-1-0-agreements-granted-claims/owf-contributor-license-agreement-1-0-copyright).

If source code is included in the specification, that code is subject to the
[Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0.txt) unless otherwise marked. In the case of any conflict or
confusion between the OWF Contributor License and the designated source code license within this specification, the terms of the OWF Contributor License MUST apply.

These terms are inherited from the Technical Stack Working Group at the Trust over IP Foundation. [Working Group Charter](https://trustoverip.org/wp-content/uploads/TSWG-2-Charter-Revision.pdf).


## Terms of Use

These materials are made available under and are subject to the [OWF CLA 1.0 - Copyright & Patent license](https://www.openwebfoundation.org/the-agreements/the-owf-1-0-agreements-granted-claims/owf-contributor-license-agreement-1-0-copyright-and-patent). Any source code is made available under the [Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0.txt).

THESE MATERIALS ARE PROVIDED “AS IS.” The Trust Over IP Foundation, established as the Joint Development Foundation Projects, LLC, Trust Over IP Foundation Series ("ToIP"), and its members and contributors (each of ToIP, its members and contributors, a "ToIP Party") expressly disclaim any warranties (express, implied, or otherwise), including implied warranties of merchantability, non-infringement, fitness for a particular purpose, or title, related to the materials. The entire risk as to implementing or otherwise using the materials is assumed by the implementer and user. 
IN NO EVENT WILL ANY ToIP PARTY BE LIABLE TO ANY OTHER PARTY FOR LOST PROFITS OR ANY FORM OF INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES OF ANY CHARACTER FROM ANY CAUSES OF ACTION OF ANY KIND WITH RESPECT TO THESE MATERIALS, ANY DELIVERABLE OR THE ToIP GOVERNING AGREEMENT, WHETHER BASED ON BREACH OF CONTRACT, TORT (INCLUDING NEGLIGENCE), OR OTHERWISE, AND WHETHER OR NOT THE OTHER PARTY HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

[//]: # (\mainmatter)

[//]: # (\doctitle)

## Scope

Creation of a flexible collection of robustly linked, formally verifiable evidence.


## Normative references

[a]. IETF RFC-2119 Key words for use in RFCs to Indicate Requirement Levels
[a]: https://www.rfc-editor.org/rfc/rfc2119.txt

[b]. Smith, S., Griffin, K., Ed., and Trust Over IP Foundation, "Key Event Receipt Infrastructure (KERI)", January 2024.
[b]: https://trustoverip.github.io/tswg-keri-specification/

[c]. Smith, S., Feairheller, P., Griffin, K., Ed., and Trust Over IP Foundation, "Authentic Chained Data Containers (ACDC)", November 2023.
[c]: https://trustoverip.github.io/tswg-acdc-specification/

[d]. Smith, S., Griffin, K., Ed., and Trust Over IP Foundation, "Composable Event Streaming Representation (CESR)", November 2023.
[d]: https://trustoverip.github.io/tswg-cesr-specification/

[e]. JSON Schema Community, "JSON Schema Specification 2020-12", June 2022.
[e]: https://json-schema.org/specification.


## Terms and Definitions

This specification assumes familiarity with the following core concepts, which are described with a short phrase here but defined normatively in their respective specifications:

* Issuer: An entity that creates and signs a data structure like an ACDC.
* Verifier: An entity that consumes and validates a data structure to make a trust decision.
* Authentic Chained Data Container (ACDC): A verifiable data container format defined in.2
* Self-Addressing Identifier (SAID): A cryptographically generated, content-addressable identifier for a data object, as defined in the ACDC and KERI specifications.1
* Autonomic Identifier (AID): A self-certifying identifier managed via the KERI protocol.1
* Key Event Log (KEL): A verifiable, append-only log of key management events for an AID, as defined in the KERI specification.1
* Edge: A property in the `e` section of an ACDC that binds the ACDC to an external digital artifact by using a hash for (or, in some cases, a digital signature embedded in) that artifact.

In addition the following terms are defined normatively, here:

* Evidentum: A single external artifact that is referenced by an edge. A countable group of these artifacts is pluralized as "evidenta", but is usually referred to with the uncounted, collective form, "evidence".
* Proximate Metdata: Metadata about the dossier as a whole that the issuer of the dossier wishes to directly attest as part of the issuance process.
* Annotation Edge: An edge in a dossier version that points to a specific edge or artifact in a previous version of the same dossier, typically used to modify its status or apply a procedural ruling (e.g., objection, admission) without altering the immutable history.
* Temporal Pinning: The process of observing dynamic state (such as a bank balance or API result) at a specific moment in time and wrapping it in a verifiable, static attestation that can be referenced by the dossier.
* Predicate Edge: An edge that references a Zero-Knowledge Proof (ZKP) or a derived cryptographic claim rather than the raw evidence itself, used to prove eligibility or factual status without revealing underlying sensitive data.
* Curator: The entity responsible for the iterative assembly of evidence artifacts and the definition of a dossier's initial data structure.
* Proposer: The entity that initiates a joint issuance action and coordinates the collection of endorsements or signatures from participant autonomic identifiers.
* Finalizer: An entity that observes the satisfaction of a joint issuance threshold and anchors the resulting proofs in a key event log to simplify verification for external parties.
* `thr` (threshold operator): An m-ary operator used within an edge group to define the cumulative weight of signatures or seals required to satisfy a joint issuance requirement.
* `fin` (finalization operator): An operator that signals the intended presence of a finalization event in a key event log, serving as a hint to verifiers regarding the proof structure.
* `rev` (revocation operator): An operator used to define a specific threshold for revoking a dossier, which may be configured independently of the issuance threshold.

[//]: # (Dossier Data Model {#sec:content})

## Dossier Data Model (Normative)

### Core Structure: The Dossier as an ACDC

A dossier MUST be a valid Authentic Chained Data Container (ACDC) as defined in the ACDC specification.2

### The Role of the Issuer

The issuer of a dossier is the entity that curates the collection of evidence and attests to its composition by digitally signing the container. The issuer's signature makes a specific, verifiable assertion: at the time of issuance, the collection of evidence referenced within the dossier is the exact collection the issuer intended to present. The issuer does not necessarily attest to the veracity of the claims within the evidence, but rather to the integrity and composition of the collection itself.

### The Edges Attribute: Linking to Evidence

The primary payload of a dossier is not a set of direct claims, but rather a graph of references to external evidence. This graph is contained within an edges block (`e`), as defined in the ACDC specification.[[2]] This block MUST contain a JSON object where each key is a semantic label for an edge, and each value is an object describing the link to the external evidence.

A dossier MAY contain an unbounded number of edges, reflecting its core purpose of aggregating an arbitrary quantity and variety of evidence. The field names (keys) for these edges MAY be any valid JSON string, allowing issuers to provide semantically meaningful labels for the linked evidence (e.g.,
"vettingCredential", "forensicReport_01", "tnAllocationProof"), as demonstrated in the Verifiable Voice Protocol (VVP) specification.

### Base JSON-Schema Definition

To ensure a baseline of interoperability while preserving the flexibility required for diverse use cases, all dossiers MUST conform to a base JSON Schema. This specification defines the normative requirements for such a schema.

A compliant schema for a dossier:

* MUST be composed of an `allOf` array, of which one object is a `$ref` that references the base dossier schema by its SAID, and other objects add additional structure as desired. The `$ref` to the base dossier signals that the schema is for a dossier and should be processed using the dossier semantics defined in this spec. Example:

    ```json
    {
        "$schema": "[https://json-schema.org/draft/2020-12/schema](https://json-schema.org/draft/2020-12/schema)",
        "$id": "EOvMDBLnGaNHqfZgEnqnQO8lpzPQ5bRxC_RdoiniiuGz",
        "title": "Mortgage Creditworthiness Dossier",
        "description": "Evidence of a borrower's qualification for a mortgage.",
        "allOf": [
            { 
                "description": "reference to dossier base schema",
                "$ref": "EBLnGaNHgEnOvMDqnQO8lpzPQqfZRxC_Rdoinii5buGz"
            },
            {
                "type": "object",
                "description": "add properties unique to this dossier in next obj",
                "properties": {
                }
            }
        ]
    }
    ```

* MUST define fields within the ACDC `a` section for proximate metadata.
* MUST use the ACDC `e` section (edges) to bind to the dossier to all evidenta, and MUST NOT place any evidenta in the `a` section.
* MAY include edges that are for traditional ACDC relationships but not for evidenta.
* SHOULD NOT include an issuee field.
* SHOULD set the `additionalProperties` keyword to true at the root level and for the edges object. This is a critical design principle that allows issuers to include arbitrary, application-specific edges without invalidating the dossier against the base schema.

This mandated flexibility has a direct consequence for implementers of verifier systems. A generic dossier verifier can be built to perform universal cryptographic validation—confirming signatures, SAIDs, and KEL consistency—for any dossier conforming to the base schema. However, such a generic verifier cannot be expected to understand the full semantics of every possible dossier. For instance, it can verify that an edge labeled "lunarPropertyDeed" is cryptographically linked, but it cannot know what that means or how to process it. Therefore, verification must be understood as a layered process. The first layer, cryptographic validation, is universal and defined by this specification. The second layer, semantic validation (e.g., "Does this dossier contain a valid TNAlloc credential for the phone number in question?"), is necessarily application-specific and requires context-dependent business logic. This separation allows the dossier format to be a universal building block for evidence aggregation across countless current and future use cases.

## Incorporating Evidence

A dossier's primary function is to serve as a container for references to external evidence. This section defines the normative methods for incorporating both ACDC-native and non-ACDC evidence formats.

### Referencing ACDC-Native Evidence

When a piece of evidence being included in a dossier is itself a valid ACDC (for example, a vettingCredential or a TNAlloc credential as defined in VVP), the corresponding edge in the dossier's edges block MUST reference that evidence by its SAID and the SAID of its schema.

The value of the edge MUST be a JSON object containing at least the following two keys:

* `n`: The SAID of the referenced ACDC. This provides a direct, tamper-evident link to the evidence artifact.
* `s`: The SAID of the schema to which the referenced ACDC conforms. This allows a verifier to correctly parse and interpret the evidence.

This pattern is exemplified by the sample dossier in the VVP specification.

### Referencing Non-ACDC Evidence

To foster broad interoperability, this specification provides mechanisms for incorporating evidence from other verifiable data ecosystems, such as W3C Verifiable Credentials or ISO mDLs.

#### Direct Reference by Signature (Discouraged)

A dossier MAY include an edge that references non-ACDC material directly by its digital signature, without declaring a schema for the content. However, this method is NOT RECOMMENDED.

The rationale for this recommendation is that direct reference places a significant and often untenable burden on the verifier. The verifier must possess the logic to parse and validate the foreign data format, understand its unique lifecycle, and locate its specific revocation mechanism, if one even exists. Such foreign evidence often has "unpredictable lifespans and undefined schemas," making robust, automated verification difficult and brittle.1

#### The ACDC Wrapper Pattern (Recommended)

The normatively RECOMMENDED pattern for including non-ACDC evidence is the "ACDC Wrapper," also referred to as bridging. This pattern externalizes the complexity of handling foreign evidence formats into a dedicated, verifiable attestation.

The process is as follows:

1. A designated entity, hereafter the "bridging party," obtains the non-ACDC evidence.
1. The bridging party verifies the foreign evidence according to its native rules and policies (e.g., validating the signature and semantics of a W3C Verifiable Credential).
1. The bridging party then issues a new, standard ACDC—the wrapper—that makes a specific, verifiable assertion, such as: "I, the bridging party, successfully verified the attached foreign evidence on date X according to policy Y".1
1. This newly created wrapper ACDC is then linked into the dossier using the standard mechanism for ACDC-native evidence described in Section 3.1.

This pattern transforms the problem of verifying a foreign format into the problem of trusting the attestation of the bridging party. While this introduces a new trust consideration, it standardizes the verification process for the dossier's consumer, who now only needs to validate a standard ACDC and assess the reputation of the bridging party. This "trust translation" is a powerful interoperability tool, but it requires careful consideration by verifiers. The verifier's trust in the wrapped evidence is now contingent on the reputation, security practices, and verification policies of the bridging party. Furthermore, the revocation lifecycles of the original foreign evidence and the ACDC wrapper are decoupled unless a specific governance framework explicitly links them. These considerations are explored further in Section 5.

#### Normative Schema for a Generic Evidence Wrapper

To support the ACDC Wrapper pattern, this specification defines the requirements for a generic wrapper ACDC schema. A schema for a generic evidence wrapper MUST include fields to capture:

* The AID of the bridging party (as the issuer of the wrapper).
* The timestamp of the verification event.
* The original foreign evidence, which MAY be embedded directly within the wrapper or referenced via a secure link (e.g., a URL with a hash).

A reference (e.g., a URL) to the verification policy or governance framework that the bridging party followed when validating the foreign evidence.

## The Operational Lifecycle: Creation, Evolution, and Verification

The dossier is a persistent, evolving data artifact with a distinct lifecycle encompassing curation, iterative assembly, state management, citation, and verification.

### Curation: Assembling and Signing the Dossier

Curation is the process of creating a dossier. This phase is typically performed in advance of any real-time transaction and involves the assembly and attestation of the evidence collection.1

The normative steps for dossier curation are as follows:

1. Evidence acquisition: The entity intending to issue the dossier first acquires the necessary evidence from their respective authoritative sources. For example, a business might obtain a legal entity vetting credential from a qualified issuer, a telephone number allocation credential from its carrier, and a brand credential from a brand vetter.1

2. Assembly: The issuer or curator constructs the dossier ACDC data structure. This involves creating an edges block and populating it with named links that point to each acquired evidence artifact, as described in Section 3.

3. Iterative assembly and versioning: Some dossiers are static, but with others, as new evidence is collected or the status of investigation changes, the dossier evolves. To support this, curators MAY issue new versions of a dossier. A new version MUST be a valid ACDC that links to the previous version via a prev edge or a schema-specific equivalent. This creates a verifiable chain of the dossier's history, allowing verifiers to traverse back through the lineage of the evidence collection.

4. Issuance initiation: For single-issuer dossiers, the curator signs the fully assembled dossier ACDC. For joint issuance, the curator provides the drafted ACDC to a proposer. The proposer then coordinates the signing or anchoring process among the designated members.

5. Signing and anchoring: The curator or finalizer uses the private keys associated with a KERI AID to sign the dossier. This act creates a non-repudiable attestation to the dossier's content. The issuance event is then anchored in a key event log (KEL), providing a permanent record. In joint issuance, this anchor may be distributed across several KELs or consolidated in a finalization event.

6. Publication: The issuer or proposer publishes the signed dossier ACDC at a stable, publicly resolvable location, typically one or more HTTP URLs. This allows authorized verifiers to fetch the dossier when it is cited.

### State Management and Metadata Overlays

For dossiers used in procedural contexts (e.g., legal proceedings, insurance adjustments), the mere existence of evidence is insufficient; its status relative to the procedure matters. An artifact may be "marked for identification," "admitted," "objected to," or "stricken." Because ACDCs are immutable, an issuer cannot simply modify the metadata of an existing edge.

To manage these state transitions, dossiers MUST employ **Annotation Edges**. An annotation edge is an edge in a new version of the dossier that points to an artifact (or an edge) in a previous version. The payload of the annotation edge carries the new state or ruling. For example, a "Court Case Dossier v2" might contain an edge labeled `ruling_101` that points to the SAID of `exhibit_A` (from v1) with the attribute `status: "admitted"`. Verifiers MUST process the dossier by traversing the graph to resolve the "effective state" of each piece of evidence, applying the latest annotations found in the chain.

### Temporal Pinning

Many dossiers require evidence of dynamic states, such as a bank balance, a credit score, or a current employment status. Direct links to live APIs are unverifiable in a static context. To include dynamic data, issuers MUST use **Temporal Pinning**.

This process requires the assembler (or a trusted "oracle" service) to:
1. Observe the dynamic state at a specific instant (`Time T`).
2. Wrap that observation in a signed ACDC (an "Observation Attestation").
3. Anchor that ACDC in a KEL.

The dossier then links to this static, timestamped Observation Attestation. This effectively "freezes" the data stream at a specific block height, allowing the dossier to assert, "The borrower had $50,000 in this account at the exact moment this dossier was assembled," rather than "The borrower has $50,000 now."

### Citation: Referencing the Dossier in Protocols

Because dossiers are designed to be stable, long-lived, and potentially large data structures, they are generally not transmitted in their entirety within real-time communication protocols. Instead, they are cited.1

A citation is a reference that allows a verifier to locate and retrieve the full dossier. The normative requirement for a dossier citation is that it MUST be a resolvable identifier that enables a verifier to fetch the complete and unmodified dossier ACDC. The canonical implementation of this is the Out-of-Band Invitation (OOBI) URL used in the evd (evidence) claim of a VVP passport. An OOBI is a specialized URL that points to a resource serving the ACDC and its associated KERI proofs.

### Verification: Algorithm for Validation

The verification process for a dossier requires a citation and a referenceTime as inputs. To support joint issuance, the algorithm follows these steps:

1. Fetch dossier: resolve the citation to retrieve the dossier ACDC.

2. Validate dossier integrity: calculate the SAID of the retrieved data and ensure it matches the expected SAID from the citation.

3. Determine issuance model: inspect the edges block for the fin operator or m-ary thr operator.

4. Validate signatures and anchors:
   a. If the fin operator is present, locate the finalization event in the primary KEL. Verify that the event contains the required threshold-satisfying signatures or seals.
   b. If the fin operator is absent but a thr operator is present, identify the member AIDs defined in the threshold group. Fetch the individual KEL for each member and verify that a valid seal to the dossier SAID exists in each KEL. Confirm that the total weight of verified members meets the threshold defined in the thr operator.
   c. For standard dossiers with a single issuer, retrieve the issuer KEL and verify the signature against the public keys authoritative at the referenceTime.

5. Recursive graph traversal: for each named edge in the edges block, fetch the referenced artifact and perform this validation algorithm recursively.

6. Check revocation status: for the dossier and every node in the evidence graph, consult the relevant KELs or status registries for revocation events effective at the referenceTime.

7. Apply semantic rules: apply application-specific policy rules once cryptographic validation is complete.

## Joint issuance

### Joint issuance logic
Joint issuance is a model for authorizing a dossier through asynchronous coordination. Unlike group multisig, which requires synchronous agreement on key event log (KEL) sequence numbers, joint issuance relies on logic within the ACDC layer. This allows members to contribute signatures or seals to a dossier at different times and via different channels without immediate impact on a shared KEL.

The validity of a jointly issued dossier is determined by the satisfaction of a threshold operator within its edge graph. The logic is decoupled from key management. This permits higher flexibility in how issuance is achieved and verified.

### Roles
Joint issuance involves specific roles that may be performed by the same or different entities:

* Curator: the entity that assembles the evidence artifacts and defines the initial dossier structure.
* Proposer: the entity that initiates the issuance action and distributes the candidate dossier for endorsement.
* Finalizer: any entity that, upon observing that an issuance threshold is met, submits a finalization event to a KEL.

### Threshold mechanics
A joint issuance must satisfy an m-ary threshold operator defined in the edge section of the dossier ACDC. A schema may define the required threshold and the set of possible signers. Alternatively, a schema may defer these definitions to the dossier instance, allowing the threshold rules to be actualized only when the issuance is proposed.

### New operators for joint issuance
The following operators are defined to support the logic of joint issuance within the edges block:

* thr: a threshold operator that defines the cumulative weight of signatures or seals required to satisfy the issuance.
* fin: a finalization operator that signals whether a verifier should expect a finalization event in a KEL.
* rev: a revocation operator that defines the threshold required to revoke the dossier, which may differ from the issuance threshold.

### Finalization
A proposer MAY choose to finalize a joint issuance to assist verifiers that do not perform recursive graph traversal.

1. Satisfaction: a finalizer observes that a sufficient number of signatures or seals have been gathered to meet the threshold.
2. Allocation: the finalizer allocates the next sequence number in the relevant KEL, which is typically a group AID.
3. Anchoring: the finalizer attaches the threshold-satisfying proofs to a KEL event and submits it for witnessing.

If a finalization event is present, a verifier should use it as the definitive proof of issuance. If absent, a verifier must poll the individual KELs of all possible participants to determine if the threshold has been met.

### Revocation
Revocation logic in a joint issuance may be defined independently of issuance logic.

* Default: if no separate revocation rule is defined, the threshold required to revoke a dossier is identical to the threshold required to issue it.
* Asymmetric thresholds: a dossier may specify different operators for creation and revocation. For example, a dossier may require a majority for issuance but allow a single administrative AID to perform a revocation.

## Security Considerations

### Integrity and Non-Repudiation Via KERI

The security of the dossier model is founded on the cryptographic primitives provided by KERI and ACDC.

Integrity: The integrity of the dossier and all ACDC-native evidence within its graph is guaranteed by the use of self-addressing identifiers (SAIDs). A SAID is a cryptographic hash of an object's canonical content. Any modification to the data results in a different SAID, making tampering immediately evident.

Non-repudiation: The act of issuing a dossier is made non-repudiable by digital signatures. These signatures are cryptographically anchored in a key event log (KEL), which serves as a permanent, publicly auditable, and tamper-evident log of all significant actions. In joint issuance, non-repudiation is achieved through the collective anchors of all participating members in their respective KELs. A finalization event, if used, provides a single cryptographic record of this consensus.

### Replay Attack Mitigation in Citation Protocols

The dossier itself is a stable, long-lived artifact designed for reuse. As such, the primary risk of replay attacks exists at the level of the protocol that cites it. An attacker could capture a valid citation message and re-submit it in a different context.

To mitigate this, any protocol that cites a dossier must incorporate ephemeral, context-specific data into the payload that is cryptographically signed. This data must bind the citation to a unique transaction using timestamps to create a narrow window of validity, originator and destination identifiers, and unique nonces to prevent identical replays.

### Verifier Trust and Root of Trust Management

The dossier model operates on a decentralized root of trust. A verifier does not rely on a single authority but makes explicit trust decisions about a plurality of evidence issuers. In joint issuance, this trust is distributed across the member AIDs defined in the threshold.

The foundation of this trust is the KERI witness infrastructure. Witnesses are independent services that act as notaries for an AID's KEL. By requiring an issuer to report its key events to a set of witnesses, the system gains high availability and duplicity detection. Verifiers should consult multiple witnesses to ensure they have a consistent and complete view of an issuer's KEL, thereby protecting against duplicity and compromise.

### Long-term Auditability and Historical Analysis

The KERI-based dossier ecosystem supports robust, long-lived auditing. Because KELs provide a complete, verifiable, and sequenced history of an identifier's key state, a verifier can perform validation for any arbitrary point in the past. 

This capability is critical for use cases involving compliance and legal discovery. An auditor can determine if a dossier and its entire evidence graph were valid at the time of a transaction, based on the key states and revocation information known at that moment. This provides non-repudiable historical accountability.

## Privacy Considerations

### Graduated Disclosure Mechanism

Dossiers MAY support privacy-preserving disclosure of their contents through the graduated disclosure mechanism inherent to the ACDC specification. An ACDC is a hierarchical JSON object, and its SAID is computed recursively: the hash of a parent object is derived from its scalar values and the SAIDs of its child objects.

This structure means that any child object within an ACDC can be replaced by its SAID without altering the SAID of the parent object and without invalidating the digital signature on the entire container. This allows the holder or issuer of a dossier to generate redacted versions of the ACDC. These versions selectively hide sensitive information while remaining cryptographically verifiable. In joint issuance, redaction does not affect the validity of member seals anchored in KELs, as those seals point to the immutable SAID of the root dossier.

### Analysis of Data Correlation Vectors

Even with the use of graduated disclosure, verifiers may be able to correlate activity across multiple transactions by observing persistent identifiers. The primary correlation vectors in a dossier-based protocol are:

* Dossier SAID: the SAID of the dossier itself is a unique and persistent identifier for that specific evidence collection.
* Citation signer AID: the AID of the entity signing the real-time citation message can be used to link all messages signed by that same identifier.
* Explicit brand information: any unredacted brand information is an intentional correlator.

### Mitigation Strategies for Unwanted Correlation

Where privacy is a requirement, implementers should employ strategies to mitigate these correlation vectors.

For the citation signer AID: the AID used for signing transactional messages can be rotated frequently without affecting the long-lived AID of the dossier issuer. Furthermore, a service provider signing on behalf of many clients can maintain a pool of AIDs to provide herd privacy and break correlation.

For the dossier SAID: to break the link between a transaction and a persistent dossier SAID, a trusted third party or blinding service MAY be used. This service can verify an original dossier and then issue a new, short-lived, derivative dossier. This derivative dossier attests to the validity of the original without revealing its SAID to the end verifier.

### Contractually Protected Disclosure

Technical privacy mechanisms can be augmented with legal and contractual controls. A server hosting a dossier MAY be configured to enforce access control policies. For example, it could serve a redacted, privacy-preserving version of a dossier to any anonymous request but require a cryptographically signed request to access a more expanded version. The act of signing the request can be tied to the verifier's agreement to terms regarding data privacy, creating a verifiable audit trail of who accessed sensitive information.

## Use Cases and Architectural Patterns

The following use cases illustrate distinct architectural patterns for deploying dossiers. Each profile highlights a different combination of grouping strategies, state management, and trust delegation, demonstrating the dossier's flexibility across diverse domains.

### Verifiable Voice Protocol (VVP): The Compositional Dossier

The Verifiable Voice Protocol (VVP) represents the **Compositional Dossier** pattern. Here, the primary goal is not to tell a story or trace a history, but to assemble a valid "permission slip" from independent authorities.

* **Goal:** Prove the right to engage in a high-trust activity (making a call).
* **Key Concept: Distributed Root of Trust.** In this pattern, the dossier assembler (the Accountable Party) does not generate the evidence. Instead, they act as a curator, bundling credentials issued by distinct, domain-specific roots of trust:
    * **Legal Identity:** Vetted by a Legal Entity Identifier (LEI) issuer.
    * **Resource Authority:** Telephone number usage vetted by a telecom carrier or regulator.
    * **Brand Rights:** Vetted by a trademark steward.
* **Verification Logic:** The verifier validates the dossier by recursively checking the issuers of the edge credentials. Trust is derived from the leaf nodes (the authorities), not merely from the dossier issuer. This pattern is ideal for access control, licensing, and regulatory compliance.

### Law Enforcement and Adjudication: The Procedural Dossier

This profile illustrates the **Procedural Dossier** pattern, which manages the complex lifecycle of evidence from field collection through courtroom adjudication. It extends the "Crime Scene" concept to handle the adversarial nature of legal proceedings.

* **Goal:** Maintain a tamper-evident Chain of Custody while allowing for the procedural evolution of evidence status.
* **Key Concept: Lifecycle of Evidence and State Transitions.**
    * **Phase 1 (Investigation):** The dossier serves as an immutable "bag" of collected artifacts (photos, DNA reports). The focus is on *completeness* and *provenance*.
    * **Phase 2 (Adjudication):** As the case moves to trial, evidence undergoes state changes. An artifact may be `Marked`, `Offered`, `Admitted`, or `Stricken`.
* **Mechanism:** This pattern employs **Annotation Edges** (see Section 10.2). To "strike" a piece of evidence, the Clerk of the Court issues a new dossier version containing an edge that targets the original evidence's SAID and applies a `status: "stricken"` attribute. This preserves the original artifact (essential for appeals) while explicitly excluding it from the current "effective" body of facts.

### Investigative Journalism: The Redacted Dossier

This profile demonstrates the **Redacted Dossier** pattern, designed to reconcile the conflict between the need for public verification and the obligation to protect confidential sources.

* **Goal:** Prove the existence and provenance of source material without revealing the source's identity.
* **Key Concept: The Precursor Link.** This pattern utilizes the Cross-File Association (CFA) concept of "precursor" relationships.
    * **The Private Graph:** The journalist holds a "Source Asset" (e.g., an unredacted recording of a whistleblower).
    * **The Public Graph:** The journalist publishes a "Redacted Asset" (e.g., a transcript with names removed).
* **Mechanism:** The public dossier links to the Redacted Asset. Internally, the Redacted Asset is cryptographically linked to the Source Asset via a "blinded" edge—typically a hash of the original file. This allows the journalist to prove, at a future date (e.g., declassification), that the redacted text was indeed derived from the specific original recording, without having exposed the source during the investigation.

### Mortgage Qualification: The Snapshot Dossier

The Mortgage Qualification profile illustrates the **Snapshot Dossier** pattern, which addresses the challenge of verifying dynamic, volatile data such as bank balances or credit scores.

* **Goal:** Prove the state of a changing system at a specific point in time.
* **Key Concept: Temporal Pinning.** A dossier cannot simply link to a bank's API, as the balance changes. It must link to a static artifact.
* **Mechanism:** This pattern employs the **Oracle** or **Observer** role. The assembler (or a trusted third-party service) queries the dynamic data source at `Time T`. This observation is then wrapped in a signed ACDC (an "Observation Attestation") that effectively says, "I observed Account X having Balance Y at Block Height Z." The dossier links to this static attestation. This converts a stream of data into a verifiable snapshot, allowing a loan officer to verify "Funds Available" at the exact moment of the application.

### Clinical Trials: The Predicate Dossier

This profile introduces the **Predicate Dossier** pattern, essential for environments with strict privacy regulations (e.g., HIPAA, GDPR) where raw data cannot be shared.

* **Goal:** Prove eligibility or compliance without disclosing the underlying sensitive data.
* **Key Concept: Zero-Knowledge Predicates.**
* **Mechanism:** Instead of linking to a raw evidence file (e.g., `blood_test_results.pdf`), the dossier links to a **Predicate Edge**. This edge points to a Zero-Knowledge Proof (ZKP) or a derived cryptographic claim generated from the raw data.
    * *Example:* The dossier asserts `inclusion_criteria_met: true`. The evidence is a ZKP proving that "Subject Age > 18 AND HIV_Status == Positive" without revealing the subject's birthdate or specific medical markers.
* **Verification:** The verifier validates the cryptographic proof rather than parsing the document, enabling high-assurance compliance without data leakage.

### The petition: the open-endorsement dossier

This profile demonstrates the open-endorsement dossier pattern, designed for cases where the set of participants is large or cannot be fully enumerated at the start of the curation process.

* goal: collect a threshold of endorsements from a distributed and potentially dynamic set of signers.
* key concept: asynchronous threshold satisfaction. Unlike a standard multisig group that requires tight coordination among a fixed set of peers, this pattern allows any aid that satisfies the criteria defined in the dossier schema to contribute an endorsement.
* mechanism: the proposer initiates the dossier and distributes the candidate acdc. participants signify their agreement by anchoring a seal to the dossier said in their individual kels. the dossier uses the thr operator to define the conditions for validity, such as a specific count of unique endorsements.
* verification: a verifier confirms the dossier is valid by observing that the required number of individual kels contain the necessary seals. the proposer may choose to finalize the dossier once the target count is reached to simplify this check for third parties.

[//]: # (\newpage)

[//]: # (\makebibliography)

## Bibliography

[[spec]]

[1]. KERI
[1]: https://trustoverip.github.io/tswg-keri-specification/

[2]. ACDC
[2]: https://trustoverip.github.io/tswg-acdc-specification/

[3]. CESR
[3]: https://trustoverip.github.io/tswg-cesr-specification/

[4]. Verifiable Voice Protocol
[4]: https://www.ietf.org/archive/id/draft-hardman-verifiable-voice-protocol-03.html

```