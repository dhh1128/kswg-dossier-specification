# Editorial Pass Report

This report summarizes the editorial pass on `spec/spec-head.md`,
`spec/spec-body.md`, `spec/terms-and-definitions-intro.md`, and the
`spec/terms-definitions/` directory, working from `origin/main` at
`32c20ef`.

**This report should be deleted before merge.** It is committed here only
so the author can review findings and decide what to do about the items
that were not auto-fixed.

## Categories of changes made

Each category corresponds roughly to one commit.

### 1. Typos, orphan footnote markers, citation format cleanup

Examples (before / after):

- `cryptopgraphic evidence` -> `cryptographic evidence`
- `bind to the dossier to all evidenta` -> `bind the dossier to all evidenta`
- `enumerated in in mgrp` -> `enumerated in mgrp`
- `ACDC specification.2` -> `ACDC specification [[2]]`
- `Autonomic Identifiers (AIDs).4` -> `Autonomic Identifiers (AIDs) [[1]]`
  (KERI is reference 1 in the bibliography; the `.4` orphan appeared to be
  a stray footnote from a source document; `[[4]]` is VVP, not KERI)
- `deterministic serialization for cryptographic operations.[[6]]` ->
  `... operations [[3]]` (CESR is reference 3; `[[6]]` is the Oh / Park /
  Ahn paper, which does not describe CESR)
- `cited.1` and `vetter.1` and `collection.1` -> trailing `.1` removed
  (could not safely guess which entry was intended)
- `[BES]` -> `[[8]]` and `[FA-SCHEMA]` -> `[[9]]` throughout
- Bibliography entries `[8]` and `[9]` reformatted to follow the
  `[N]. ... \n [N]: URL` pattern used by entries 1-7. Placeholder
  URLs were left pointing at the same provisional address used in the
  original; see "References needed" below.

### 2. Vocabulary / register

- `utilizes` -> `uses`
- `Furthermore,` -> `also` (mid-sentence)
- `employ strategies` -> `use strategies`
- vague `robust` qualifier on `long-lived auditing` removed

### 3. Clarity and sentence length

- The four-clause bridge-wrapper warning sentence was split into a
  "two numbered caveats" structure for readability.
- The `M`-operator bullet had soft framing (`Use of the M operator
  also means that ...`) tightened to direct phrasing (`The M operator
  also allows ...`) and a long compound sentence was split with a
  colon to expose the *m of n* structure.
- The "Logic" section had a long passive-voiced sentence converted
  to active voice and split.
- Throat-clearing `it is important to distinguish the dossier from
  related concepts` -> `the dossier should be distinguished from
  related concepts`.
- `fraught with challenges` -> `presents serious challenges`.
- `adjustor` -> `adjuster` (standard spelling).

### 4. Passive -> active voice

- `The integrity of the dossier ... is guaranteed by the use of
  self-addressing identifiers` -> `Self-addressing identifiers
  (SAIDs) guarantee the integrity of the dossier ...`
- `The act of issuing a dossier is made non-repudiable by digital
  signatures` -> `Digital signatures make the act of issuing a
  dossier non-repudiable`
- `non-repudiation is achieved through the collective anchors` ->
  `the collective anchors ... provide non-repudiation`
- `dossiers MUST employ Annotation Edges` -> `dossiers MUST use
  Annotation Edges`

### 5. Softened AI-authorship tells

- `signifies a maturation` -> `marks a step forward`
- `nuanced understanding ... holistic evaluation` -> `more realistic
  understanding ... evaluating many interrelated facts together`
- `vital role` -> drop `vital`
- The Evidence Curator definition was split out of the trailing
  em-dashed clause into its own sentences.
- `represents a fundamental shift from ... paradigms` -> `is a
  fundamental shift from ... models`
- `This is a critical design principle that allows issuers ...` ->
  `This design choice lets issuers ...`

### 6. RFC 2119 normative keyword promotions

Lowercase `must` / `should` promoted to uppercase MUST / SHOULD where
the sentence expresses a genuine conformance requirement:

- "A joint issuance must satisfy an m-ary threshold operator" -> MUST
- `Q` operator constraints: `must be met by signers`, `edge must
  also contain` -> MUST
- Citation replay-mitigation: `protocol ... must incorporate` and
  `This data must bind` -> MUST
- `Verifiers should consult multiple witnesses` -> SHOULD
- `implementers should use strategies` -> SHOULD
- `verifier should use it as the definitive proof` and `verifier
  must poll` -> SHOULD / MUST

Left lowercase where the sentence is descriptive or expresses
authorial reasoning rather than a conformance requirement, e.g.
`verification must be understood as a layered process`,
`verifiers must keep two caveats in mind`,
`it must link to a static artifact` (the latter is rationale for
why temporal pinning exists, not a constraint on conformant
implementations).

## Open questions for the author

These are places where I suspected an issue but could not fix it
without risking a substantive change. Please review.

1. **Orphan footnote `.4` after `Autonomic Identifiers (AIDs)` in the
   head.** I rewrote this to `[[1]]` (KERI) because that is what the
   surrounding clause is describing. If the author intended a
   different reference, please correct.

2. **Orphan footnote `[[6]]` attached to CESR in the head.** I rewrote
   this to `[[3]]` for the same reason. `[[6]]` (Oh / Park / Ahn) is
   a multi-agent formation control paper and does not describe CESR.

3. **Section 3 / Section 10.2 cross-references** (body lines 180 and
   456). The document is not numerically sectioned; these references
   are stale. Probably want to convert to named-section links such
   as `Incorporating Evidence` and `State Management and Metadata
   Overlays` respectively.

4. **Reference list duplication / mismatch between head and body.**
   The head's "Normative references" uses alphabetic labels
   `[a]`-`[e]` for IETF RFC-2119, KERI, ACDC, CESR, JSON Schema.
   The body's bibliography uses numeric `[1]`-`[9]` for KERI, ACDC,
   CESR, VVP, three multi-agent papers, BES, and FA Schema. Inline
   citations in the body use the numeric list. Consider whether the
   head list should be removed, the body bibliography promoted to
   normative, or the two reconciled.

5. **Stripped trailing footnote markers `.1`** at the end of three
   body sentences (Curation intro, Evidence acquisition example,
   "they are cited.1"). I removed the orphan digit rather than
   guessing the intended bibliography entry. If the author can
   recall the source, please add appropriate `[[N]]` references.

6. **Bibliography entries [8] (BES) and [9] (FA Schema)**. I
   reformatted them to match the `[N]. ... \n [N]: URL` template
   used by entries 1-7, but the original placeholder text
   (`[your published URL]`, `[repo URL]`) made it clear these
   URLs are still TBD. See "References needed" below.

7. **`evidenta` as a noun usage**: the term definition says
   `evidenta` is the plural count form and `evidence` the
   uncountable collective. The phrase `MUST NOT place any
   evidenta in the a section` is grammatically singular (`any
   ... evidenta`); arguably `any evidentum` is correct. Left
   in place because the intent (no evidentary artifacts at all)
   is unambiguous.

8. **"adjustor" -> "adjuster"** was changed in the proximate-metadata
   section illustration. Both spellings exist; "adjuster" is the
   dictionary form. Revert if the author has a house preference.

## RFC-keyword inconsistencies not unilaterally resolved

- **The spec does not contain an RFC 2119 / RFC 8174 notational
  paragraph.** Per the editorial brief this should be flagged
  rather than auto-added. Suggested location: a "Conformance" or
  "Terminology" subsection before "Normative references" in the
  head, with the canonical paragraph:

  > The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT,
  > SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this
  > document are to be interpreted as described in BCP 14
  > [RFC2119] [RFC8174] when, and only when, they appear in all
  > capitals, as shown here.

- The spec uses `MUST` consistently; no `SHALL` appears. This is
  good. The choice is now de facto canonical.

- `RECOMMENDED` and `NOT RECOMMENDED` appear once each in the
  Foreign Artifact wrapper section. They are used correctly per
  RFC 2119 but might be worth visually flagging with `**` for
  consistency with how the spec sometimes bolds key normative
  terms (e.g. `**Annotation Edges**`).

## AI-tell candidates left in place (with reasons)

- `In both the physical and digital realms` (head intro): mild
  AI cadence but the phrase does load-bearing scope work for the
  reader.
- `disparate evidence`, `coherent whole` (head intro): formal
  but accurate; rewriting risks losing specificity.
- `nuanced` was changed to `realistic`, but the surrounding
  paragraph still has a slightly elevated abstract register
  (`This shift establishes a new role within digital
  ecosystems`). Further softening would start to alter authorial
  voice; left for the author to decide.
- `essential for environments with strict privacy regulations`
  (Clinical Trials profile, body line 478): mild marketing
  cadence, but `essential` here is making a real claim about
  applicability, so it stays.
- `tamper-evident`, `non-repudiable`, `verifiable`, `cryptographic
  integrity`: these are precise technical terms in this domain,
  not buzzwords. Left untouched throughout.

## References needed

The following claims could not be cited from a primary source
during this pass and warrant author attention:

1. **`[[8]]` Bytewise and Externalized SAIDs.** The bibliography
   currently lists a 2024 Hardman document with a provisional
   tooling URL. No canonical specification URL was identified.
   The body normatively references this spec for the bSAID /
   xSAID algorithms; an authoritative URL is required before
   publication.

2. **`[[9]]` Foreign Artifact Credential schema.** Same issue.
   The body refers to `[[9]]` as the publication of a reference
   schema and example. Without a stable URL the normative chain
   for Foreign Artifact wrappers is incomplete.

3. **"Verifiable Voice Protocol (VVP) specification"** is
   referenced inline several times (the `tnAllocationProof`
   edge example, the `evd` (evidence) claim of a VVP passport,
   the recursive verifier example). Reference `[[4]]` is the
   IETF draft. The wording suggests the author may want a more
   specific, section-level pointer than a top-level draft URL
   for at least one of these mentions.

4. **"Cross-File Association (CFA) concept of `precursor`
   relationships"** in the journalism use case. No reference
   is provided for CFA. If this is an external concept it
   should be cited; if novel to this spec it should probably
   be defined in the terms section.

5. **ISO 8601, ISO 3166-1 alpha-2, ISO 3166-2, ISO 6709, and
   IANA MIME types** are mentioned in the standard
   proximate-metadata field definitions. Convention varies, but
   it would be conventional to cite these by their canonical
   identifier (e.g. "[ISO8601]") in the references section.

6. **"OWF Contributor License Agreement 1.0 - Copyright"** in
   the head is fully cited; this is fine.

7. **RFC 8174** is the BCP 14 companion to RFC 2119 and would
   need to be added to normative references if the conformance
   paragraph above is adopted.
