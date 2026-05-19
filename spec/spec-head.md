# Verifiable Dossiers

**Specification Status**: v0.6 Draft

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
Whether physical or digital, critical decisions are rarely based on a single piece of information. Decision makers rely on a collection of disparate evidence, curated to form a coherent whole. A loan officer assembles a file of financial statements, credit reports, and employment verifications to assess creditworthiness; a law enforcement official compiles a case file of forensic reports, witness statements, and crime scene documentation to build a case. The confidence of the decision depends not only on the validity of the individual pieces of evidence but also on the integrity of the collection itself.

Digital evidence aggregation faces several challenges. The core problem is the absence of a standardized, cryptographically secure method to aggregate diverse pieces of evidence, attest to the integrity of the collection, and manage its lifecycle in a decentralized and interoperable manner. Existing systems for evidence management are often siloed within proprietary platforms, dependent on centralized trusted parties, or lack the cryptographic guarantees needed for high-assurance environments. This fragmentation creates friction, blocks interoperability across domains (e.g., between different jurisdictions or industries), and introduces single points of failure that can be compromised or become unavailable.

### Introducing the Dossier: An Issuer-Centric Evidence Container
This specification introduces the dossier as a solution to these challenges. A dossier is formally defined as an Authentic Chained Data Container (ACDC) that references an arbitrarily rich collection of signed evidence and is issued by the party that assembles it. It is a container designed to create a verifiable data graph from evidentiary artifacts.

A key distinction separates a dossier from a traditional verifiable credential. A credential typically makes an assertion about a specific subject, or issuee, conferring some right or attribute upon them. A dossier has no issuee. It has only an issuer—the entity that curates the collection. In this sense, a dossier functions more like a notarized affidavit than a passport: the issuer makes a formal, verifiable attestation about the composition and integrity of the evidence collection itself. This issuer-centric model is a fundamental shift from traditional subject-centric identity paradigms.

A dossier exists to support decisions made on the basis of the attested collection of evidence it includes. This contrasts with systems that generate proofs just-in-time in response to a specific verifier query. By pre-assembling evidence into a stable, long-lived, and verifiable container, the dossier supports efficient and scalable proof presentation in many contexts.

### Relationship to KERI, ACDC, and Verifiable Credentials
The dossier is not a standalone concept; it is deeply rooted in a stack of emerging open standards for decentralized identity. Its technical foundation is the Authentic Chained Data Containers (ACDC) specification, which defines a format for verifiable, chainable data structures [[2]]. ACDCs, in turn, are secured by the Key Event Receipt Infrastructure (KERI), a protocol for decentralized key management that provides secure, rotatable, and auditable Autonomic Identifiers (AIDs) [[1]]. The canonical data representation is provided by the Composable Event Streaming Representation (CESR), an encoding format that ensures deterministic serialization for cryptographic operations [[3]].

Within this ecosystem, a dossier should be distinguished from related concepts such as a conventional ACDC credential or a "bespoke ACDC". A bespoke ACDC may also contain custom links to evidence, but it is typically generated just-in-time to answer a specific verifier's query. A dossier, by contrast, is a pre-curated collection intended for broader, repeated use.

The dossier marks a maturation in decentralized identity: the ecosystem is moving beyond simple, atomic claims about a subject (the purpose of a traditional credential) to support curated narratives backed by a body of evidence. This reflects a more nuanced understanding of trust — in the real world, assurance is often derived from evaluating multiple, interrelated facts together.

This shift establishes a new role within digital ecosystems: the "Evidence Curator." An Evidence Curator could be an individual managing their own data, a lawyer building a case, a journalist protecting sources, or an automated service. The role assembles and attests to evidence collections, and the dossier provides the formal data structure to support this function.

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
[b]: https://trustoverip.github.io/kswg-keri-specification/

[c]. Smith, S., Feairheller, P., Griffin, K., Ed., and Trust Over IP Foundation, "Authentic Chained Data Containers (ACDC)", November 2023.
[c]: https://trustoverip.github.io/kswg-acdc-specification/

[d]. Smith, S., Griffin, K., Ed., and Trust Over IP Foundation, "Composable Event Streaming Representation (CESR)", November 2023.
[d]: https://trustoverip.github.io/kswg-cesr-specification/

[e]. JSON Schema Community, "JSON Schema Specification 2020-12", June 2022.
[e]: https://json-schema.org/specification.

