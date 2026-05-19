# Editorial pass — findings report

This report accompanies the `editorial-pass` PR. **Delete this file before merging** the PR; it is a working document for the author.

The pass covers `spec/spec-head.md`, `spec/spec-body.md`, `spec/terms-and-definitions-intro.md`, and the files under `spec/terms-definitions/`. The term files were left unchanged — they are short, clear, and free of the categories of problems the pass was looking for.

## Categories of changes made

### 1. Typos, broken citations, list numbering, casing regression (commit `fix typos, broken citations, list numbering, and casing regression`)

- Orphaned footnote markers like `specification.2`, `(AIDs).4`, `transaction.1`, `vetter.1`, `compliance.1`, `format.1`, `[[6]]` were scattered through the introduction and body. These are leftovers from the source document the spec was derived from and do not point to anything in the current bibliography. They were either removed (where the sentence stood on its own) or converted to proper `[[N]]` references aligned with the actual bibliography.
- Fixed `cryptopgraphic` → `cryptographic`.
- Fixed duplicate `in in` in the M-operator paragraph.
- Renumbered the bridging-party procedure list from `1. 1. 1. 1.` to `1. 2. 3. 4.` (markdown renders both correctly, but the source was confusing to read).
- Restored proper capitalization, bold formatting, and `[[ref: coordinator]]` markup in the "Petition / Open-Endorsement Dossier" use-case section, which had been written entirely in lowercase. The same section referenced an undefined `MGRP` operator; this was corrected to `M (optionally combined with Q)`, matching how the operator is defined earlier in the same document. Flagging this as a possible content question for the author — see "Open questions" below.

### 2. Plain-language register and AI-tell removal (commit `plain-language register and trim AI-tell phrasing`)

Examples of substitutions:

| Before | After |
|---|---|
| "in both the physical and digital realms" | "Whether physical or digital" |
| "is fraught with challenges" | "faces several challenges" |
| "It is important to distinguish the dossier from related concepts" | "a dossier should be distinguished from related concepts" |
| "utilize" / "utilizes" | "use" / "uses" |
| "facilitate" / "facilitates" / "enable" (vague) | "let" / "support" |
| "in order to" | "to" |
| "furthermore" | "also" |
| "additionally" | "also" |
| "robust, long-lived auditing" | "long-lived auditing" |
| "essential for environments" | "useful in environments" |
| "as demonstrated in" | "as shown in" |
| "augmented with" | "combined with" |
| "Such a generic verifier cannot be expected to understand" | "Such a generic verifier cannot understand" |
| "in a wide variety of contexts" | "in many contexts" |

Precise technical terms (`verifier`, `issuer`, `credential`, `evidence`, `dossier`, `SAID`, `KEL`, `AID`, `ACDC`, etc.) were preserved verbatim.

### 3. RFC 2119 / RFC 8174 keyword fixes (commit `uppercase RFC 2119 keywords for genuine normative requirements`)

The spec uses `MUST` exclusively (never `SHALL`) for the strongest level of obligation, so no canonical-keyword inconsistency had to be resolved.

Genuine normative obligations that were stated in lowercase were uppercased:

| Location | Before | After |
|---|---|---|
| Threshold mechanics | "A joint issuance must satisfy..." | `MUST satisfy` |
| Threshold mechanics | "A schema may define / may defer..." | `MAY define / MAY defer` |
| `Q` operator | "must be met / must also contain / must exist" | `MUST` for each |
| `FIN` operator | "a verifier must collect" | `MUST collect` |
| Finalization | "a verifier should use / a verifier must poll" | `SHOULD use / MUST poll` |
| Revocation | "may be defined" / "may specify" | `MAY` |
| Replay attack mitigation | "must incorporate" / "must bind" | `MUST` for each |
| Verifier trust | "Verifiers should consult multiple witnesses" | `SHOULD consult` |
| Correlation mitigation | "implementers should employ" | `SHOULD employ` |

### 4. Sentence-length breakups (commit `break up long sentences and the M-operator wall of text`)

- The `M` operator description was a single ~250-word paragraph mixing three distinct topics. Split into three paragraphs: count semantics, the optional enumeration of potential signers, and the *m of n* approval pattern.
- The "Evidence Curator" paragraph in the introduction was split into three paragraphs.
- The `RM` operator sentence was tightened with parallel structure.

### 5. Passive → active voice (commit `flip passive voice to active where the actor is clear`)

Selected flips; passive was retained where the actor is genuinely unknown or irrelevant.

| Before | After |
|---|---|
| "is guaranteed by the use of SAIDs" | "SAIDs guarantee..." |
| "is made non-repudiable by digital signatures" | "Digital signatures make ... non-repudiable" |
| "non-repudiation is achieved through the collective anchors" | "the collective anchors ... provide non-repudiation" |
| "By requiring an issuer to report ..., the system gains" | "Requiring an issuer to report ... gives the system" |
| "This graph is contained within an edges block" | "The ACDC specification places this graph in an edges block" |

### 6. Added references (commit `add citations and missing references`)

- Expanded the normative reference to RFC 2119 to a full IETF citation (Bradner, BCP 14, 1997).
- Added a normative reference to RFC 8174 (BCP 14, Leiba, 2017), which clarifies that only uppercase RFC 2119 keywords are normative — relevant because the body uses uppercase MUST/SHOULD/MAY throughout.
- Added bibliography entries for W3C Verifiable Credentials Data Model v2.0 [[8]] (W3C TR) and ISO/IEC 18013-5:2021 (mDL) [[9]] (official ISO catalogue page), and cited them at the body's first mention of those ecosystems.

## Open questions for the author

1. **Conformance/notation paragraph missing.** The spec lists RFC 2119 (and now RFC 8174) in normative references but contains no conformance statement of the form *"The key words MUST, MUST NOT, REQUIRED, ... in this document are to be interpreted as described in BCP 14 [RFC 2119] [RFC 8174] when, and only when, they appear in all capitals, as shown here."* W3C and IETF specs conventionally include this paragraph. Recommend adding one near the start of the body or at the end of the Scope section. Not added during this pass because it inserts new content rather than editing existing.

2. **`MGRP` operator in the Petition section.** The original main-branch text referenced "the MGRP operator". This operator is not defined anywhere in the spec; the defined operators are `M`, `RM`, `Q`, and `FIN`. The edit replaces it with `M (optionally combined with Q)`, matching the language used to describe this exact use case in the `M` operator definition. Please confirm this is the intended meaning.

3. **`a` section sub-clause in spec-body §3.3.** The original phrasing `"MUST use the ACDC e section to bind to the dossier to all evidenta"` (from the march17 branch — not in the editorial-pass base) has a duplicated "to" and slightly awkward phrasing. Left unchanged in this pass because the pass is against `main`, which uses different wording. Flagging for awareness when the march17 work lands.

4. **"Section 3.1" and "Section 10.2" references.** The body contains two cross-references to section numbers ("Section 3.1", "Section 10.2", "Section 5") but the spec is structured with named headings and (presumably) auto-generated numbering. If section numbering ever changes, these hard-coded numbers will drift. Consider replacing with anchor links or named-section references. Not changed mechanically because that requires knowledge of the build system's anchor conventions.

5. **"This pattern is exemplified by the sample dossier in the VVP specification."** A reference (e.g. `[[4]]`) would be more useful than a bare prose mention; the VVP draft is in the bibliography but isn't cited here. Not changed because the existing wording is grammatically fine; flagging as a polish opportunity.

6. **Inconsistent bibliography format.** Most bibliography entries use a short label (`[1]. KERI`) and let the link definition carry the citation detail, but entries `[5]`, `[6]`, `[7]` repeat the full citation in the link definition. The newly added `[8]`/`[9]` follow the short-label convention. The author may want to harmonize.

7. **`example-markup-in-markdown.md`** is listed in `spec/` but appears unused by the spec build. Not touched (out of scope), but worth noting.

## References needed

Claims in the current main-branch text that would benefit from a primary-source citation but for which I did not insert one (either because no obvious primary source exists, or because I could not verify the claim well enough to attach a specific reference without risking misattribution):

1. **"Three variants of cooperative control are regularly mentioned in the literature"** — the spec cites Beard/Lawton/Hadaegh [[5]], Oh/Park/Ahn [[6]], and Fax/Murray [[7]]. These are good representative papers but none of them is a canonical taxonomy paper that explicitly enumerates "leader-follower, behavior-based, virtual structures" as the three variants. A taxonomy reference such as **Cao, Yu, Ren, Chen, "An overview of recent progress in the study of distributed multi-agent coordination", IEEE Trans. Industrial Informatics 9(1), 2013** would be a stronger anchor for the trichotomy. The author should choose whether to add it.

2. **"Out-of-Band Invitation (OOBI) URL"** — the term OOBI is used without a citation. The primary source is the IETF Internet-Draft *"Out-Of-Band Introductions (OOBI)"* by Smith — `draft-ssmith-oobi-01` and successors at `https://datatracker.ietf.org/doc/draft-ssmith-oobi/`. A citation here would help readers who don't already know KERI's terminology.

3. **"Authentic Chained Data Container (ACDC) specification.2"** in §2 references "ACDC specification" — now cited as `[[2]]`. The bibliography entry `[2]` reads simply `ACDC` and points to the GitHub Pages site. This is functional but not citation-grade; a full bibliographic entry (authors, title, version, date) — matching the form already present in the **Normative references** section of the head — would be more rigorous. The Normative references section has the proper form; the body Bibliography duplicates the same items in a thinner form. Consider deduplicating, or upgrading the Bibliography form.

4. **"Self-addressing identifier (SAID)"** is introduced without a citation. The primary source is **Smith, S., "Self-Addressing Identifiers (SAIDs)", draft IETF Internet-Draft** — a citation would help. Same for the **Cross-File Association (CFA)** concept used in the Investigative Journalism use case; CFA is referenced but is not defined in this spec and has no citation.

5. **"Key Event Log (KEL)"** and **"AID"** are used heavily; the head's Normative reference to KERI [[b]] covers them, but a forward pointer at first use in the body would help readers. Not strictly missing — flagged for completeness.

## AI-tell candidates left in place

Phrases that read formulaically but that I judged better to leave alone:

1. **"This issuer-centric model represents a fundamental shift from traditional subject-centric identity paradigms."** Reduced "represents" → "is" in the edit, but the cliché framing ("fundamental shift / paradigm") was kept because rewriting it more aggressively risks weakening the author's intended emphasis on this design point.

2. **"The dossier marks a maturation in decentralized identity ..."** The original was much more flowery; the edit pared it down but preserved the framing because the document is clearly making a deliberate argumentative point here.

3. **"This pattern transforms the problem of verifying a foreign format into the problem of trusting the attestation of the bridging party."** This sentence is rhetorically constructed ("transforms X into Y") but technically clear, so left intact.

4. **"In this sense, a dossier functions more like a notarized affidavit than a passport"** — kept; the analogy is doing real work and matches the table in the head.

5. **"A critical distinction separates..."** softened to "A key distinction separates..." but the rhetorical move was retained.

6. **"The security of the dossier model is founded on the cryptographic primitives provided by KERI and ACDC."** Slight edit ("is founded on" → "rests on"); the structure is generic but appropriate for a Security Considerations preamble.

## RFC-keyword inconsistencies the author should resolve

None within the spec body — `MUST` is canonical and `SHALL` does not appear. The remaining unresolved item is:

- **No conformance/notation boilerplate** (see Open questions #1). Without it, a strict reading would say the uppercase keywords in the document are not formally bound to RFC 2119 semantics, even though RFC 2119 is in the normative references.

## Out of scope but worth noting

- The `spec-body.md` file has a number of cross-references to specifications/concepts that lack proper anchors or citations (CFA, Observation Attestation, OOBI). The spec would benefit from a glossary entry or normative reference for each.
- The use-case profile sections (VVP, Procedural, Redacted, Snapshot, Predicate, Open-Endorsement) are written in a markedly more flowery register than the normative core of the spec. They were tightened only lightly here; a future polish pass could harmonize them with the rest of the document.
