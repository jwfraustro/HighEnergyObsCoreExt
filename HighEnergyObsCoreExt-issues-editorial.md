# HighEnergyObsCoreExt: editorial issues

Line numbers refer to `HighEnergyObsCoreExt.tex` at commit `22519ff` (2026-09-04). External facts were checked against GitHub `master` of `ivoa-std/ObsCore`, `ivoa-std/ObsCoreExtensionForRadioData`, `ivoa-std/ObsLocTAP`, `ivoa-std/Vocabularies`, `ivoa-std/UCDList` on 2026-11-05.

This list contains prose, typographical, structural, citation and repository-hygiene issues.
None of them changes what an implementer or user of the extension would do. They are
collected here to be addressed in the Working Draft. Technical and science issues are in
`HighEnergyObsCoreExt-issues-primary.md`.

---

## Part A: Trivial fixes (55)

### Factual slips in prose (no implementation impact)

[x] **CTI expanded incorrectly in glossary**
Line 97: CTI is Charge Transfer Inefficiency, not Efficiency.

[x] **ARF expanded as "Auxiliary" instead of "Ancillary"**
Line 195 says "Auxiliary Response File"; OGIP, the glossary (line 74) and the response-type vocabulary all say "Ancillary".

[x] **AMS-2 should be AMS-02**
Line 597: the experiment is AMS-02.

[] **hc described as CODATA-based when it is exact**
Line 313: h, c and e are defined constants, so state hc/e as exact and drop the CODATA attribution.

[x] **Tau antineutrino notation**
Line 395: `$\nu_\tau^{-}$` should be `$\bar\nu_\tau$`.

[] **`cosmological origin` should be `astrophysical origin`**
Line 585: background events are of instrumental or astrophysical/cosmic-ray origin, not cosmological.

[] **OGIP formats described as "legacy"**
Line 707: OGIP is the current format for Chandra, XMM-Newton, NuSTAR, Swift, NICER and others; replace "legacy high energy surveys at HEASARC" with "X-ray and gamma-ray missions".

[x] **"IACT" used as an instrument name**
Line 376: IACT is a class; write "IACTs such as H.E.S.S.".

[] **VEP cited via GitHub diff anchor**
Line 531: cite the VEP by number, not a pull-request diff URL.

### Internal inconsistencies

[] **HEIG formation date contradicts contribution history**
Line 138 says "Fall of 2024"; line 760 lists the first HE group meeting on 2023-05-11; reconcile.

[] **Author name spelled two ways**
Line 25 "Hallmann" vs line 752 "Hallman".

[] **Affiliation style inconsistent**
Line 22 "LUX -- ObsParis" vs lines 13, 20 "LUX -- Observatoire de Paris"; line 18 "IVOA" is not an institution.

[x] **Hyphenation of `response-function` inconsistent**
Lines 496 and 499 use both spellings; pick one.

[] **NULL quoted inconsistently**
Lines 158, 251, 259, 267, 351: use unquoted NULL throughout; it is a SQL null, not a string.

### Placeholders and editorial residue

[] **Table caption does not define "MAN"**
Line 450: state that MAN means mandatory.

[] **Remove editor comments from source**
Lines 160, 198–199, 213–215, 534, 615, 619–623, 637–639, 659–664, 683 are internal review exchanges and must not ship in a public release (the open questions they record are filed separately under Part B).

[] **Remove unused `\TODO` macro and `todonotes`**
Lines 41, 50–57: neither is used in this file.

[x] **Duplicate and redundant package loads**
Lines 35, 37: `lscape` loaded twice and already pulled in by `pdflscape`; line 47: `hyperref` is loaded by `ivoa.cls`.

[] **Glossary machinery half-enabled**
Lines 111, 720: `\makeglossaries` and `\printglossaries` are commented out; either print the glossary or drop the `glossaries` package and `\gls` calls.

[] **Prune unused glossary entries**
34 of the 49 `\newacronym` entries (lines 61–109) are never used via `\gls`/`\glspl` in any `.tex` file of the repository: CDA, CTI, EPIC, ESAC, SOC, SSC, SAS, SVOM, ORCA, ARCA, MAGIC, VERITAS, FACT, LHAASO, CAOM, GW, TAP, HRC, CXC, OGIP, NASA, HEASARC, GADF, VODF, ESA, XMM-Newton, KM3NeT, ANTARES, FITS, ACIS, HAWC, CTAO, PSF, UHE, HE; remove them or use them.

### Grammar and wording

[x] **Hard-coded section numbers**
Lines 152, 169, 209: replace "sections 3--5" and "§5" with `\ref`.

[x] **"multiple telescope simultaneously"**
Line 351: "telescopes".

[x] **"the particle astrophysics community ... use"**
Line 391: "uses".

[x] **"PDG ID very unlikely to be recognized"**
Line 397: missing "is".

[x] **"include additional columns to the"**
Line 403: "in the".

[x] **"contributed to the content and structure the text"**
Line 748: "structure of the text".

[x] **Tense and capitalisation in DataLink section**
Line 566: "we proposed" → "we propose"; "Each Datalink is described" → "Each DataLink link is described".

[x] **"a hea-event-bundle" / "compounded dataset"**
Line 181: "an hea-event-bundle" (as elsewhere) and "compound dataset".

[] **Math-mode italics in cosmology terms**
Line 189: `$\Lambda CDM$` → `$\Lambda$CDM`, `$\Omega M$` → `$\Omega_{\mathrm M}$` (moot if the footnote is deleted).

[x] **Stray space in `stat.distribution`**
Line 663: `{\em stat.distribution \/}`.

[x] **Remove "(reasonable)" qualifier**
Lines 249, 257, 265: "Under the (reasonable) assumption" → "Assuming".

[] **Remove redundant NULL-default remark**
Line 351: "with a default value of NULL to handle WCD and neutrino instruments" is true of every optional column.

### Editorial consolidation (no content change)

[] **Collapse the three `*_calib_status` subsections**
Lines 245–267 are the same three paragraphs repeated for spatial, time and spectral axes; merge into one subsection.

[] **Remove duplicated term definitions between §3.1 and §5.1**
Lines 177–182 and 188–196 repeat lines 473–485, 495–513 and 523–529 nearly verbatim; keep only the §5 copies and reference them from §3.

[] **Replace eight repetitions of the "values vary by facility" sentence**
Lines 319, 333, 346, 357, 363, 370, 386, 399: state once at the top of §4 that string-valued mode attributes are facility-specific unless a vocabulary is given.

[] **Delete the prose table of contents**
Line 152 restates the section headings and contains self-assessment ("rational and well-considered proposals").

[] **Delete the VOEvent-confusion paragraph**
Line 184: VOEvent is not a `dataproduct_type` value and `event-list` already disambiguates.

[] **Delete the alternative design for energy attributes**
Line 311, final parenthesis: the rejected `em_min_energy` design does not belong in the specification.

[] **Delete the "λ = c/ν is easy to remember" argument**
Line 311: the case for `energy_min`/`energy_max` is precision and massive particles, not arithmetic difficulty.

[] **Trim example inventories**
Lines 186 (Chandra Source Catalog file counts), 277 (ACIS column count and coordinate-frame list), 281 (CSC MPDF example): reduce each to at most one clause.

### Section 6, appendices, and repository hygiene

[x] **"MK3NeT" typo**
`extendedAccessTonewtypesOfproducts.tex` line 27: KM3NeT.

[x] **Typos in Section 6**
`extendedAccessTonewtypesOfproducts.tex`: "consistant" (70), "a arbitrary" (65), "access directly to the" (147), "Datalink" vs "DataLink" in headings (114, 143).

[x] **Unused 700 KB figure committed**
`Obscore+datalink.png` is only referenced from commented-out lines 78–83 of `extendedAccessTonewtypesOfproducts.tex`; delete the file or restore the figure.

[] **Internal wiki links cited in Section 6**
`extendedAccessTonewtypesOfproducts.tex` line 27: three footnotes point to `wiki.ivoa.net/internal/…`, which requires login; cite public URLs or drop.

[x] **`\hyperref[url]{…}` should be `\href`**
`UseCases.tex` line 366.

[] **Hand-written use-case numbers**
`UseCases.tex` lines 21, 34: "A.1.8", "A1.9", "A2.4" → `\ref`.

[] **"error_messsage"**
`UseCases.tex` line 231.

[x] **Mismatched quotes ``aeff``**
`UseCases.tex` line 429.

[] **Editor comments in appendix sources**
`UseCases.tex` lines 393, 470, 506–507, 516; `extendedAccessTonewtypesOfproducts.tex` lines 4–12, 17, 30, 208–212: remove before public release.

[x] **Summary table typos**
`Dataproducts-Summary-table.tex`: "containing containing", "quantitiy", bold brace in "Vocabular}y".

[] **VEP-25 section reference stale**
`VEP-25_response-function.txt` cites "section 5.1.5" for response functions; it is now 5.1.3.

[] **VEP-25 duplicated**
`VEP-25_response-function.txt` exists at the repo root and in `VEP/`; keep one.

[x] **Committed glossary build artefacts**
`HighEnergyObsCoreExt.glg` and `.gls` are build outputs; remove and add `*.glg`, `*.gls` to `.gitignore`.

[] **`.gitignore` typo**
Line `./github/workflows/*` should be deleted (the workflows are intentionally tracked; the path is also misspelled).

[] **Unused bib entry for the 2006 CIAO paper**
`2006SPIE.6270E..1VF` is defined but never cited; cite it alongside the 2026 paper or remove it.

---

## Part B: Issues requiring discussion (1)

### B25. Cut plan for the trimmed document (tracking issue)
*Labels: discussion, structure*

Umbrella issue linking the Part A "Editorial consolidation" items and B2. Target: body from §1 through §5 reduced from roughly 8,000 words to about 3,500, following the outline in B2. Specific per-section cuts are listed in the companion review file (`HighEnergyObsCoreExt-review.md`, Section 4).
