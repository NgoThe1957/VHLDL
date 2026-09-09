# VHLDL Revision Candidate — LINK004-001 vNext

> **Status: CANDIDATE — NOT FROZEN**
>
> This document is the controlled revision candidate for the current Golden/Regression cycle. It must not silently replace the locked baseline. Any baseline change requires Project Office review and Change Log entry.

## 1. Scope

This candidate covers controlled revision of Markdown extraction, page traceability, chapter-heading classification, page/header/footer handling, entity evidence, and cross-reference/link evidence for the VHLDL pilot.

## 2. Locked Change Requirements

### MCR-LINK004-001 — Chapter Heading vs Running Header
Distinguish true Chapter Heading from Running Header/Page Header using position, page structure, and context. Repeated running headers must not be promoted to chapter headings.

### MCR-LINK004-002 — Header/Footer/Page Number Classification
Classify header/footer/page-number material from PDF evidence. Do not delete numeric content when it belongs to source content.

### MCR-LINK004-003 — Chapter Heading OCR Normalization
Normalize OCR errors in Chapter Heading while preserving original content, order, meaning, and heading level.

### MCR-LINK004-004 — Deterministic PDF Page Anchor
Every represented PDF page must have a deterministic anchor `#pdf-page-N`, where `N` is the technical PDF page number. Anchors must be unique and exact; `#pdf-page-36` must not match `#pdf-page-360`.

### MCR-LINK004-005 — PDF Page ↔ Markdown Evidence
A page-mapping claim is PASS only when the PDF page and corresponding Markdown page marker/anchor are independently evidenced. Do not extrapolate a complete mapping from an isolated sample.

### MCR-LINK004-006 — Printed Page Separation
`Printed_Page` is distinct from `PDF_Page`. Populate `Printed_Page` only when independently evidenced by the source; otherwise retain `UNKNOWN`.

### MCR-LINK004-007 — Actual Cross-file Markdown Link
A cross-reference is PASS only when an actual Markdown link exists and points to the intended target artifact and anchor. A Workbook record alone is insufficient evidence.

### MCR-LINK004-008 — Cross-link Target Integrity
Verify source file, target file, target anchor, and link syntax for every accepted cross-file link. Missing, broken, or ambiguous links remain HOLD.

### MCR-LINK004-009 — Page 27 Chapter Heading Normalization
For LSVN_001 PDF Page 27, the source evidence establishes the chapter opening as `Chương I` followed by `VIỆT NAM THỜI KỲ NGUYÊN THỦY`, then `I. DẤU TÍCH NGƯỜI VƯỢN Ở VIỆT NAM`. The revision rule shall normalize only the OCR/layout spacing in the chapter-title line from `V I Ệ T N A M  T H Ờ I  K Ỳ  N G U Y Ê N  T H Ủ Y` to `VIỆT NAM THỜI KỲ NGUYÊN THỦY`, while preserving source wording, order, chapter level, and surrounding content. This rule is page/context-specific evidence and must not promote the repeated `Chương I ...` running headers observed on PDF Pages 29, 31, 33, and 35.

## 3. Data / Traceability Contract

The controlled traceability contract requires, as applicable:

- `BookID`
- Source Book ID
- `Volume`
- `Chapter`
- `Section`
- `PDF_Page`
- `Printed_Page`
- `Anchor_ID`
- `Relative_Link`
- Source Reference / Evidence
- reverse traceability where applicable

Stable IDs must remain unique. Existing locked mappings must not be broken by introducing new IDs.

## 4. Page Mapping Gate

Required relation:

`Printed Page ↔ PDF Page ↔ Markdown`

If Printed Page cannot be independently established, it remains `UNKNOWN`. PDF Page remains the technical traceability key. Markdown anchors must use the PDF page number, not the printed page number.

Current regression evidence includes:

- PDF Page 144 ↔ Printed Page 142
- PDF Page 145 ↔ Printed Page 143

These observations are regression evidence and do not by themselves authorize a universal page offset.

## 5. Link Gate

A Workbook CrossReference row is not sufficient for PASS. The actual Markdown link must be present and independently verified to resolve to the intended target file and target anchor.

## 6. Entity Gate

An entity may be accepted only when it has a valid stable ID, no duplicate identity, source evidence, and the correct entity type. The system must not invent entities or silently reconcile ambiguous source evidence.

## 7. Minimum Controlled Output

The extraction/revision process is expected to produce, as applicable:

- Markdown
- Workbook / Index
- TOC
- Search Index
- Entities
- Relationships
- Anchor_ID
- Relative_Link
- Traceability
- Cross-reference records
- QA Evidence

## 8. QA Status Vocabulary

Use controlled statuses such as `PASS`, `PASS WITH NOTE`, `NEEDS REVIEW`, `FAILED`, and `HOLD` where governance requires a blocking state.

## 9. Baseline Protection

The current five-sheet `index/Index.xlsx` remains the current baseline. The proposed 14-sheet model is a candidate target and does not replace the current workbook without an approved Change Request and Change Log entry.

Source PDFs are immutable. QA findings must not be hidden by changing source data.

## 10. Acceptance Sequence

`Revision Candidate → Change Requirements → Regression Tests + Evidence → QA → Project Office Review → GO/HOLD`

Golden Acceptance remains HOLD until the required evidence passes. Mass Extraction must not begin merely because an artifact exists or a workflow succeeds.
