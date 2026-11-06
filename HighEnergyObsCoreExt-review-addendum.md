# Addendum: check against the upstream repository

Repository: https://github.com/ivoa/HighEnergyObsCoreExt (cloned 2026-11-05).

## 1. Diff result

The uploaded `.tex` is byte-identical to `HighEnergyObsCoreExt.tex` on `main` at commit `22519ff` (2026-09-04, "Merge pull request #71 from iannevans/Sep4Final"). That commit is also what the `auto-pdf-preview` tag points at, so the CI-built PDF (74 pages, pdfTeX, built 2026-09-04 19:08 UTC) corresponds exactly to what was reviewed. Nothing in the earlier review is invalidated by a newer upstream version.

Two things in the repository do change earlier conclusions, and several files that were not in the upload contain new problems. They are listed below. Items that could not be checked from this environment are in Section 7.

## 2. Findings that change the earlier review

### 2.1 The document is a Proposed Endorsed Note, not a Working Draft

`Makefile`: `DOCTYPE = PEN`, `DOCVERSION = 1.0`, `DOCDATE = 2026-09-04`. The PDF title page reads "Proposed Endorsed Note 2026-09-04" and the status section carries the PEN boilerplate.

This resolves discussion issue B2 ("Note or WD?") but not in the document's favour:

- An Endorsed Note is reviewed by the TCG and is citable, but it is not on the Recommendation track. The Radio extension, which this document repeatedly says it harmonises with, is a Working Draft heading for REC. Two ObsCore extension tables with different standing (`ivoa.obscore_radio` by REC, `ivoa.obscore_hea` by EN) is a governance question the TCG will ask on day one.
- The RFC 2119 section and the "MAN" column are inconsistent with the "we propose / we suggest" phrasing regardless of track. An EN can carry normative language (the UCD list is an EN) but then it must actually use it.
- "Roadmap to…" in the title and "Each of them needs to be evaluated" in Appendix A (see 4.1) describe a work-in-progress, not something ready for endorsement.

**Revised B2:** either (a) request TCG endorsement as a *roadmap/requirements* Note and strip everything that looks normative (table MAN column, RFC 2119, "must" in the join-key sentence), or (b) move to the REC track alongside the Radio extension and rewrite normatively. Mixing the two is what will draw fire.

### 2.2 The VEP files in the repository disagree with the document

The repository contains the vocabulary extension proposals the document says it has submitted. They do not match the main text:

| Topic | Document (main `.tex`) | VEP file in repo |
|---|---|---|
| Flux UCD branch | `phot.flux.energy`, `phot.flux.particle`, `phot.count.*` (lines 641–653) | `VEPforUCD/VEP-UCD-28`: `phys.flux`, `phys.flux.energy`, `phys.flux.density`, `phys.fluence` (a `phys.*` branch; several entries marked "Decision: remove", "need to clarify") |
| Asymmetric errors | `stat.error.minus`, `stat.error.plus` (lines 612, 665–666) | `VEP-UCD-30`: `stat.error.negative`/`positive`, with a counter-proposal of `.below`/`.above`; "Decision:" blank |
| Event-detection UCD root | `instr.detection` (Q), event type as `meta.code.qual;instr.detection;phys.particle` (lines 579, 593) | `VEP-UCD-31`: `obs.event` (S), event type as `meta.code.class;obs.event`; description field empty |
| `stat.confidenceLevel` | P→Q syntax change (line 613) | `VEP-UCD-27`: label amendment only, no syntax-code change; header says `Vocabulary: http://www.ivoa.net/rdf/datalink/core` (copy-paste error) |
| Home of `draws`/`pdf`/`region` | all three together, "advanced data product type vocabulary" (line 531) | `VEP-analysis-products.txt`: `draws` and `pdf` to `analysis-product-type`, `region` to `product-type` |

So the document and the proposals it points to are out of sync on names, syntax codes, and target vocabularies. Whichever is current must be propagated to the other. This supersedes the earlier B21 wording and strengthens B11.

Also in `VEP-analysis-products.txt`: the `pdf` entry proposes `dataproduct_subtype` values "Differentiate pdf" (sic, for "differential") and "Integral pdf"; the main text never mentions these.

### 2.3 `Dataproducts-Summary-table.tex` (Section 5.1.5, not in the upload)

- `rmf`, `edisp`, `psf` are given parents `#response-function, #pdf`. `pdf` lives in a different vocabulary (`analysis-product-type`). A `skos:broader` relation across vocabularies is not something the IVOA vocabulary tooling supports, and the `response-type/terms.csv` on GitHub has no such relation. Either drop `#pdf` or explain the mechanism.
- `hea-event-list` is given parent `#temporally-resolved-dataset`. The published `event-list` term has no broader term. If a parent is wanted, propose it via VEP; note an event list is equally spatially and spectrally resolved.
- Typos: "containing containing", "quantitiy", bold brace misplaced in "Vocabular}y".
- The table repeats the `hea-` prefix, so B1 applies here too.

## 3. Section 6 (`extendedAccessTonewtypesOfproducts.tex`), newly reviewed

### 3.1 Substantive

- **Bundle serialisation is finally mentioned here** (line 14: "OGIP FITS, GADF FITS, or VODF FITS, or … a tarball"; line 32: `application/x-tar`). This belongs in Section 3/4 where `hea-event-bundle` is defined, and it confirms B4: the bundle is "whatever the provider ships". Say so normatively.
- **`INTERSECTS(...) = 6`** in all three SQL examples (lines 156, 170, 185, 200). ADQL `INTERSECTS` returns 0 or 1. This is a copy-paste error that renders into the PDF four times.
- **`datalink.response`** is used as a schema-qualified table name for the DataLink-as-TAP-table approach, while Appendix B uses `ivoa.response` for a different (response function) table. Two tables named `response` with different meanings in one document.
- **DataLink table example** (lines 91–105): rows 5–8 have `ID` 20316 while every `access_url` is for observation 020136; `content_type` is `x-fits-gadf` (not a MIME type; Appendix A's own table uses `application/x-fits-gadf`, so the repo is inconsistent with itself); the `#package` row's `content_qualifier` is `hea-event-bundle`.
- **`semantics = '#auxiliary'` with `content_qualifier = 'bkgimage'`** (line 188): `bkgimage` is not in any vocabulary mentioned in the document.
- **`dl_association_id`** (line 192) proposes adding a column to `ivoa.obscore` itself, in passing, in an SQL example. If it is wanted it needs a definition in Section 4.
- **Internal wiki links** (line 27, three footnotes to `wiki.ivoa.net/internal/…`) require an IVOA login and cannot be cited from a public document.
- The three "ways" in 6.2 and the "alternate table" in 6.3 are presented as options with "the choice of access method is up to the data provider" (line 21). For an EN that is fine; for anything normative it is the same unresolved-architecture problem as Appendix A (4.1 below).

### 3.2 Trivial

- "MK3NeT" (line 27) → KM3NeT.
- "fully consistant" (line 70); "a arbitrary set" (line 65); "access directly to the" (line 147); "to access directly a background image" (line 177).
- `http:/provider.org/…` single slash (line 125).
- `x-fits-bintable` (line 32) → `application/x-fits-bintable`.
- Curly quotes `’` inside SQL (lines 157, 158, 171–173, 186, 188, 201) will not paste into a TAP client.
- "Datalink" vs "DataLink" capitalisation varies (headings at lines 114, 143).
- The large `Obscore+datalink.png` (700 KB) is committed but its `\includegraphics` is commented out (lines 78–83); remove one or the other.

## 4. Appendix A (`UseCases.tex`), newly reviewed

### 4.1 The appendix exposes three unresolved architectures

Lines 16–57 present **Proposal 1** (all products in `ivoa.obscore`, `dataproduct_type` drawn from any of three vocabularies), **Proposal 2** (new terms in `product-type`, response types via `dataproduct_subtype`), and **Proposal 3** (three tables: `ivoa.obscore`, `ivoa.response`, `ivoa.adp`), then says "Each of them needs to be evaluated in various situations and explored further". The use cases are then written against different proposals: A.1.3 and A.1.9 query `dataproduct_type = 'aeff'` directly (Proposal 1); A.1.8 and A.1.13 query `dataproduct_type = 'response-function' AND dataproduct_subtype = 'psf'` (Proposal 2); Appendix B sketches Proposal 3.

This is the concrete form of B22. A document proposed for endorsement cannot leave the central design choice open, and the main text (Sections 3–5) never tells the reader that this choice is open. Add it as a top-level discussion issue: **decide between Proposals 1, 2 and 3 before PEN review, and make every use case consistent with the choice.**

### 4.2 Substantive

- **`s_fov` is a diameter.** A.1.10 and A.1.11 use `CIRCLE('ICRS', s_ra, s_dec, s_fov)` as the detector field of view. ObsCore 1.1 defines `s_fov` as the diameter of the containing circle, so the radius is `s_fov/2`. The queries return twice the intended area.
- **A.1.12** uses `CIRCLE('ICRS', s_ra, s_dec, 80)` as a stand-in for a neutrino detector's field of view and `BOX('GALACTIC', 0, 0, 30, 2)`; the 80° is unexplained, and `INTERSECTS` on a circle that large against a box in a different frame is exactly the case where the document should be using `s_region`.
- **A.1.7** (spectro-polarimetric Crab) depends on `o_ucd LIKE '%phys.polarization%'`, i.e. on the hash-list design of B5, and contains a typo `o.ucd` (line 286) that would fail to parse. It is also the only place polarimetry appears, which supports B-new on IXPE below.
- **A.1.6** step list (lines 218–226) says "The query output is a VOTable that follows the DataLink VO standard". The ObsCore query output is a TAP result; only the `access_url` targets are DataLink documents. Also `instrument_name LIKE 'CTAO-S'` (line 214) has no wildcard, so `LIKE` behaves as `=`; same at line 496 (`'CTAO-N'`).
- **A.1.6 / A.1.4** criteria say `access_format = "datalink"` while the SQL uses the full MIME type; align.
- **A.2.2** (flux maps): the criteria say `dataproduct_type = image` AND `dataproduct_subtype = fluxmap`; the SQL uses `OR` (lines 493–494), which returns every image in the collection.
- **A.2.3** (M31): criteria list `energy_min ≥ 0.3 and energy_max ≤ 7.0` (keV, and "within"), the SQL uses `energy_min <= 300.0 AND energy_max >= 7000.0` (eV, and "covers"). A commented-out line shows the editors disagreed about which. Decide, and use one unit.
- **A.2.3** also uses `TMOC(27, t_intervals)` and `TMOC(27, 56320.0, 56325.0)` with no definition of a `TMOC` function anywhere; the footnote says the function "does not yet exist in ADQL". This is the prototype UDF from Section 4.3; name it and reference it consistently.
- **A.2.4**: the parentheses in the SQL (lines 553–556) are unbalanced in meaning: `(obs_collection = 'CTAO-DR1' AND … AND target_name = …)` closes after `target_name`; harmless but sloppy. Criteria say "tmin/tmax", SQL says `t_min/t_max`.
- **A.1.13** queries `messenger_pdgid = '-16' OR messenger_pdgid = '+16'` — the string-with-sign design of B18 in action; with an integer column this is `ABS(messenger_pdgid) = 16`.
- **A.1.11** criterion (vi) "analysis mode optimized for pointsource searches" becomes `analysis_mode = 'pointsource'`; A.1.13 uses `LIKE '%pointsource%'` for the same thing. This is the free-text-mode problem of B6 made visible.
- Several use cases join `ivoa.obscore_hea` without using any of its columns (A.1.1, A.1.4, A.1.8, A.2.1, A.2.2, A.2.4); an editor comment at line 470 asks the same question. Drop the join where it is not needed, or the examples teach a bad habit.
- The appendix references use cases as "A.1.8", "A1.9", "A2.4" by hand (line 21, 34); use `\ref`.
- `\hyperref[https://doi.org/…]{…}` (line 366) is the wrong macro (`\hyperref[label]` takes an internal label); use `\href`.

### 4.3 Trivial

- Line 2: `ivoa.obscore-hea` (hyphen) → `ivoa.obscore_hea`; same in Appendix B line 86 inside SQL, where it is a syntax error.
- Line 164: curly quotes in SQL.
- Line 231: "error_messsage".
- Line 429: mismatched quotes ``aeff``.
- Line 13: `\item[]` inside an itemize after `\vspace{-20pt}` is a layout hack; restructure.
- Lines 393, 470, 506–507, 516: editor comments in a public source.

## 5. Appendix B (`extendedAccessTonewtypesOfproductsAppendix.tex`), newly reviewed

- The first SQL example (lines 60–61) does `FROM ivoa.obscore AS o NATURAL JOIN ivoa.response AS r`. Table B's `ivoa.response` carries `obs_id`, `obs_publisher_did`, `target_name`, `facility_name`, `instrument_name`, so the natural join silently requires *all five* to match. This is the B8 hazard demonstrated inside the document itself; use `JOIN … ON` or `USING (obs_publisher_did)`.
- `CIRCLE, 83.6324, +22.0174, 0.083333)` (lines 64, 91) is not valid ADQL (`CIRCLE(83.6324, 22.0174, 0.083333)`).
- Second example (lines 80–98): dangling comma before `FROM`; alias `resp` used but never defined; `o.` used inside a subquery whose table has no alias `o`; `ivoa.obscore-hea`; operator precedence of `AND … OR CONTAINS(...)` almost certainly not what was meant.
- `resp_t_intervals` typed "String (TMOC)" here, "TMOC" in the main table; see B13.
- The table defines both `obs_publisher_did` and `resp_publisher_did`, but the prose (line 5) calls the latter `resp_obs_publisher_did`.
- "Further scientific input … will be needed to further explore and generalize this solution" (line 1): an appendix in a PEN that says it is not finished.

## 6. Repository hygiene

- `HighEnergyObsCoreExt.glg` and `.gls` are committed build artefacts and are not in `.gitignore` (which lists `*.glo`, `*.ist` but not `*.glg`, `*.gls`).
- `.gitignore` contains `./github/workflows/*`, a typo for `.github/…`; since the workflows are intentionally tracked, delete the line.
- `HighEnergyObsCoreExt.bib`: `2006SPIE.6270E..1VF` (the classic CIAO paper) is defined but never cited; the main text cites only the 2026 ApJ paper. Citing both is the norm. The 2026 entry (ApJ 1005, 116, DOI 10.3847/1538-4357/ae6db4) could not be resolved from this environment (doi.org blocked); verify once.
- The `.bib` uses `\apj`, which resolves only because ivoatex ships `aas_macros.tex`; fine, but note it.
- `VEP-25_response-function.txt` is present both at the repository root and in `VEP/`; keep one.
- `VEP-25` references "section 5.1.5" of the Note for response functions; it is now 5.1.3.
- `Obscore+datalink.png` (700 KB) is unused (see 3.2).
- The main `.tex` top comment says "see ivoatexDoc for what group names to use here"; harmless template residue.

## 7. Not checked

- GitHub issues and open pull requests: the GitHub API and `github.com` HTML pages were blocked from this environment; only `raw.githubusercontent.com` and `git clone` worked. 46 pull-request refs exist (highest number 71, all of the recent ones merged into `main`). Some of the items above may already be filed; de-duplicate before opening.
- The *published* state of the `product-type`, `response-type` and `messenger` vocabularies on `ivoa.net` (the site was blocked); the GitHub `master` of `ivoa-std/Vocabularies` was used instead.
- Whether the "advanced data product type vocabulary" VEP (PR 21 on `ivoa-std/VEPs`) has been accepted, and under what name (`analysis-product-type` per the repo's VEP file).

## 8. Additions to the issue list

These have been appended to `HighEnergyObsCoreExt-issues.md` (Part A items under "Added after repository check"; Part B items B26–B31), and B2 there has been rewritten per 2.1.
