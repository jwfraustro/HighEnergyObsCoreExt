# HighEnergyObsCoreExt: technical and science issues

Line numbers refer to `HighEnergyObsCoreExt.tex` at commit `22519ff` (2026-09-04). External facts were checked against GitHub `master` of `ivoa-std/ObsCore`, `ivoa-std/ObsCoreExtensionForRadioData`, `ivoa-std/ObsLocTAP`, `ivoa-std/Vocabularies`, `ivoa-std/UCDList` on 2026-11-05.

This list contains only issues that affect implementers or users of the extension (wrong or
missing definitions, conflicts with other IVOA documents, examples that do not work) and
science errors. Editorial issues (prose, typos, structure, repository hygiene) are in
`HighEnergyObsCoreExt-issues-editorial.md` and are expected to be handled in the Working Draft.

Suggested labels: `trivial` for Part A; `discussion` for Part B.

---

## Part A: Trivial fixes (40)

Each entry is an issue title followed by a one-sentence body. No decision required.

### Definitions and the normative table

[] **Wrong UCD for off-axis angle columns**
`s_ref_oaa` and `em_ref_oaa` (table lines 424, 426) use `pos.posAng`, which is a position angle; replace with `instr.offset` ("Offset angle with respect to main direction of observation").

[x] **Typo `phys.particle.antiprotron`**
Line 654: should be `phys.particle.antiproton`.

[x] **Typo `stat.fwmh`**
Line 585: should be `stat.fwhm`.

[x] **`phot.counts` should be `phot.count`**
Line 560: the UCD is `phot.count`.

[] **PDG ID format `±nn` is wrong**
Line 393: PDG numbers have up to ten digits (proton 2212, iron-56 1000260560); remove the two-digit implication.

[] **PDG ID cited via a third-party BNL page**
Lines 391, 599: cite the PDG "Monte Carlo Particle Numbering Scheme" (Review of Particle Physics) instead.

[] **Proposed MIME types lack the type/ prefix**
Lines 707–709: `x-fits-ogip` etc. are not MIME types; at minimum write them as `application/<subtype>` (scheme choice is a separate discussion issue).

[] **`phot.fluence` dimensionality contradicts its description**
Line 680: "radiant energy per unit area" is [M T^-2], not [L^-2]; make the two agree.

[x] **`on-sources` vs `on-source`**
Table line 438: should be `on-source` as in line 339.

[] **Separator for multi-valued strings inconsistent**
Line 273 uses `#` for `o_ucd`, line 286 uses commas for `proposal_id`; state one convention (design of `o_ucd` itself is a discussion issue).

[] **`obs_mode` table row has placeholder "TBU"**
Line 434: replace "(e.g., TBU)" with real example values or remove the parenthesis.

[] **`t_intervals` UCD is "TBD"**
Line 428: fill in (e.g. `time.interval`).

[] **"ObCcore" typo and muddled `energy_min`/`energy_max` descriptions**
Table lines 430, 432: fix the typo and reword to "Minimum energy of the dataset (corresponds to `em_max`)" and the converse.

### Cross-references to other IVOA documents

[] **ObsCore 1.1 requires, not suggests, NULL `o_ucd` for event lists**
Line 271: ObsCore 1.1 says `o_ucd` "must be left" NULL for event lists; change "suggests" to "requires".

[] **`ev_xel` should acknowledge ObsCore 1.1 guidance**
Line 295: ObsCore 1.1 says the number of events "should be represented in the NDCube DM"; add one sentence noting this proposal overrides that guidance for discovery purposes.

[] **`scan_mode` terms do not match the Radio extension**
Lines 342–343: the Radio extension defines `on-the-fly-cross-scan` ("cross-shaped pattern") and `on-the-fly-map` ("parallel directions"); `on-the-fly-cross-map` does not exist, so copy the Radio terms and definitions verbatim.

[] **`tracking_mode` should be `tracking_type`**
Lines 322–333: the Radio extension and ObsLocTAP both use `tracking_type` with values `Sidereal`, `Solar-system-object-tracking`, `Fixed-az-el-transit`; rename the attribute, adopt the values verbatim, and cite ObsLocTAP as the source.

[] **`response-type` definitions diverge from the vocabulary**
Lines 501–513: copy the term descriptions verbatim from `response-type/terms.csv` (e.g. `psf` says "photons" there, "particles" here).

[] **Product-type vocabulary URL uses underscore**
Lines 160, 162, 205, 542: `rdf/product_type` should be `rdf/product-type`; line 463 should use `https`.

[] **VEP-UCD-27 header names the wrong vocabulary**
`VEPforUCD/VEP-UCD-27-stat_confidencelevel.txt` says `Vocabulary: http://www.ivoa.net/rdf/datalink/core`; should be the UCD list.

### Science

[] **Fermi-LAT given as example of e+/e- separation**
Line 597: Fermi-LAT has no magnet and separated charge sign only via the geomagnetic shadow; use PAMELA and AMS-02 as the examples.

[] **Non-sensical unit "erg cm^-2 s^-1 TeV"**
Line 560: a spectral flux density needs TeV^-1.

[] **Steradian symbol and hemisphere statement**
Line 243: "2π Sr" should be "2π sr", and 2π sr is a hemisphere, not "all-sky".

[] **Wavelength exponent range is off by three orders**
Line 311: 0.1 keV to 1 PeV corresponds to roughly 1e-8 to 1e-21 m, not 1e-5 to 1e-22.

[] **Cosmology footnote is factually wrong**
Line 189 footnote: confidence contours in the (Ω_M, Ω_Λ) plane are not statistical draws; delete the footnote.

[] **Neutrino energy floor given as both TeV and GeV**
Line 138 says "above the TeV range"; line 146 says "≳ GeV"; pick one (KM3NeT/ORCA works at GeV).

### Examples, SQL and DataLink tables

[x] **`INTERSECTS(...) = 6` in Section 6 SQL examples**
`extendedAccessTonewtypesOfproducts.tex` lines 156, 170, 185, 200: ADQL `INTERSECTS` returns 0 or 1; replace `= 6` with `= 1`.

[x] **DataLink example table: ID column inconsistent**
`extendedAccessTonewtypesOfproducts.tex` lines 99–105: rows carry ID 20316 while every URL is for observation 020136.

[] **`x-fits-bintable` and `x-fits-gadf` lack the type prefix in Section 6**
`extendedAccessTonewtypesOfproducts.tex` lines 32, 52, 91–105: Appendix A already writes `application/x-fits-gadf`; make Section 6 consistent (scheme itself is B23).

[x] **Single slash in service-descriptor URL**
`extendedAccessTonewtypesOfproducts.tex` line 125: `http:/provider.org` → `http://provider.org`.

[x] **Curly quotes inside SQL listings**
`extendedAccessTonewtypesOfproducts.tex` lines 157–201 and `UseCases.tex` line 164: replace `’` with `'` so the queries paste into a TAP client.

[] **`ivoa.obscore-hea` (hyphen) in use cases and Appendix B**
`UseCases.tex` line 2 and `extendedAccessTonewtypesOfproductsAppendix.tex` line 86 (inside SQL, where it is a syntax error): `ivoa.obscore_hea`.

[x] **`o.ucd` typo in A.1.7 SQL**
`UseCases.tex` line 286: `o_ucd`.

[] **`LIKE` without wildcard**
`UseCases.tex` lines 214 (`'CTAO-S'`) and 496 (`'CTAO-N'`): add `%` or use `=`.

[x] **A.2.2 SQL uses OR where criteria say AND**
`UseCases.tex` lines 493–494: `(dataproduct_type = 'image') AND (dataproduct_subtype = 'fluxmap')`.

[] **A.2.3 energy criteria and SQL disagree in unit and sense**
`UseCases.tex` lines 517 vs 530: criteria say keV and "within", SQL says eV and "covers"; pick one (a commented line 516 shows the editors disagreed).

[] **A.1.6 says the ObsCore result "follows the DataLink standard"**
`UseCases.tex` line 218: the TAP result is a plain VOTable; only the `access_url` targets are DataLink documents.

[] **Criteria say `access_format = "datalink"`, SQL uses the full MIME type**
`UseCases.tex` lines 153/164 and 202/213: align the criteria with the SQL.

[] **Appendix B SQL does not parse**
`extendedAccessTonewtypesOfproductsAppendix.tex`: `CIRCLE, 83.6324, …)` (64, 91) → `CIRCLE(83.6324, 22.0174, 0.083333)`; dangling comma before `FROM` (83); alias `resp` never defined (82, 94); `o.` used in a subquery with no alias `o` (88, 90).

[] **Appendix B column name mismatch**
`extendedAccessTonewtypesOfproductsAppendix.tex` line 5 says `resp_obs_publisher_did`; the table (line 23) says `resp_publisher_did`.

---

## Part B: Issues requiring discussion (30)

Each entry has a title, context, the problem, options where relevant, and a proposed resolution.

### B1. Drop the `hea-` prefix; use the existing `event-list` and `event-bundle` terms
*Labels: discussion, vocabulary*

[] **Context.** Lines 177–182, 465, 473–485 propose `hea-event-list` and `hea-event-bundle` and say `event-list` will be "redesignated".

[] **Problem.** The product-type vocabulary already contains `event-list` ("A collection of observed events, such as incoming high-energy particles…") and `event-bundle` ("An event list with associated calibration files", marked preliminary). The proposed `hea-event-list` definition is a paraphrase of the existing one. The document's own reasoning (lines 186, 521) keeps `draws`, `pdf`, `region` unprefixed because they are waveband-independent; event lists are also produced outside HE (optical photon counting, HST TIME-TAG, MKIDs). Renaming a published term is a breaking change to the vocabulary with no stated benefit.

[] **Proposed resolution.** Use `event-list` and `event-bundle` throughout. If wording changes to `event-list` are wanted, or `event-bundle` should leave preliminary status, file VEPs. Close by deleting the duplicate definitions in §3.1.

---

### B2. Endorsed Note status vs normative content: say which parts are provisional
*Labels: discussion, structure*

[] **Context.** The document is an Endorsed Note (`Makefile`: `DOCTYPE = PEN`, version 1.0, dated 2026-09-04) with a Recommendation-track revision expected later. The title says "Roadmap to…" and Appendix A says the three candidate architectures "need to be evaluated". Yet the document carries RFC 2119 boilerplate (line 127), an attribute table with a mandatory column, "must" in the join-key sentence (line 407), and a fixed table name `ivoa.obscore_hea` with fixed column names.

[] **Problem.** Endorsed Notes get implemented (the H.E.S.S. ObsTAP prototype already serves `hea-event-list` and `x-fits-gadf`). Anything that looks normative in the EN will be defended as backward compatibility when the REC-track draft tries to change it. At the same time there is not one MUST or SHOULD in the body, so a reader cannot tell what a conforming service has to do, and requests to the ObsCore editors (changes to 1.1 text: `s_ra`/`s_dec`, `measurements`, `o_ucd`, `proposal_id`) are interleaved with definitions of the extension table.

[] **Proposed resolution.** In the EN: remove the RFC 2119 section and the MAN column, state in the status section that table name, column names, vocabulary terms and MIME types are provisional pending the REC-track document, and move the ObsCore 1.1 amendment requests to a clearly labelled section (see B24). In the REC-track revision: every proposal becomes a normative statement or is cut; add a service-requirements section (B16); resolve B26 first. The Radio extension should get the same treatment so the two stay parallel.

---

### B3. `aeff`/`arf` and `edisp`/`rmf` are two terms each for one quantity
*Labels: discussion, vocabulary*

[] **Context.** §5.1.3 (lines 501–513) and the `response-type` vocabulary define `aeff` and `arf` (both effective area vs energy) and `edisp` and `rmf` (both true-energy to measured-energy pdf).

[] **Problem.** The actual distinction is community and serialisation (OGIP channel grid for a specific extraction vs GADF true-energy × offset grid), which is nowhere stated. A reviewer will ask why a discovery vocabulary carries a format distinction, and why that is not expressed via `access_format`.

[] **Options.**
1. One term per physical quantity (`aeff`, `edisp`, `psf`, `bkgrate`); format via `access_format`.
2. Keep both, and add one sentence per pair stating the difference is intentional and what it is.

[] **Proposed resolution.** Option 2 is the pragmatic one given the vocabulary already exists; but the sentence must be written, and the same text must go into the vocabulary descriptions via VEP.

---

### B4. `event-bundle` has no defined serialisation
*Labels: discussion, extension-table*

[] **Context.** Lines 181, 233, 237, 484 define a bundle as "a compound dataset containing … multiple files or other substructures" and allow `access_url` to point at one.

[] **Problem.** The document never says what a client receives: tar, zip, multi-extension FITS, directory, DataLink document. Line 237 motivates MIME types by the need for clients to route bundles to tools, but §5.5 then proposes MIME types per file format, not per bundle.

[] **Options.**
1. Define the container (or a small set) and the MIME type for each.
2. State that the container is unconstrained and recommend DataLink (`#this` + `#calibration`) as the interoperable path.
3. Drop `event-bundle` as an `access_url` target and keep it only as a discovery type.

[] **Proposed resolution.** Option 2, with one normative sentence.

---

### B5. Multi-valued `o_ucd` via hash lists vs a child observables table
*Labels: discussion, obscore-core, extension-table*

[] **Context.** §3.10 (lines 269–281) proposes `o_ucd = 'pos.eq#time#instr.pulse;arith.sum'` and says `o_unit`, `o_calib_status`, `o_stat_err` would need the same treatment (line 275).

[] **Problem.** ObsCore 1.1 says `o_ucd` must be NULL for event lists, so this is a change to the core Recommendation, not an extension. Packing N observables into one string, then needing to do the same for three sibling columns, is a relational model forced into one row. A `#` inside a column typed `meta.ucd` fails UCD validation. `LIKE '%phot.flux%'` matches `phot.flux.density`.

[] **Options.**
1. Keep hash lists; request an ObsCore 1.2 change; accept the validation and query caveats.
2. Add a child table `ivoa.obscore_hea_observables` (obs_publisher_did, ucd, unit, calib_status, stat_err), one row per observable. No ObsCore change needed.

[] **Proposed resolution.** Option 2. If the authors reject it, the document should say why.

---

### B6. Free-text "mode" attributes are not cross-facility discovery attributes
*Labels: discussion, extension-table*

[] **Context.** `obs_mode`, `analysis_mode`, `event_type`, and in practice `array_pointing_mode` have no controlled vocabulary; eight subsections say values "vary from facility to facility".

[] **Problem.** No cross-facility query can be written against them. That is acceptable for intra-facility filtering but should be stated once and honestly. `event_type` has an additional problem: Fermi-LAT event types are orthogonal partitions (FRONT/BACK × PSF0–3 × EDISP0–3) and a dataset can contain PSF2+PSF3; a single-valued string cannot express that.

[] **Proposed resolution.** Keep the attributes, add one sentence at the top of §4 declaring them facility-scoped, and either make `event_type` multi-valued with a stated separator or document that it carries the provider's own partition label.

---

### B7. `convergent[-ang]` embeds a number in an enumerated value
*Labels: discussion, extension-table*

[] **Context.** Lines 354–357 define `convergent-5d` style values for `array_pointing_mode`.

[] **Problem.** Neither the mode nor the angle is queryable.

[] **Proposed resolution.** `array_pointing_mode` enumerated (`parallel`, `convergent`, `divergent`) plus `array_pointing_angle` (double, deg, NULL when not applicable).

---

### B8. Shared attributes placed in the HE extension table; `NATURAL JOIN` hazard
*Labels: discussion, obscore-core, extension-table*

[] **Context.** Line 140 states the rule: shared attributes go to base ObsCore, HE-specific ones to the extension. `t_intervals` (line 305, explicitly "any waveband"), `tracking_type`, `scan_mode` (shared with Radio and ObsLocTAP) and arguably `messenger_name` are nevertheless in `ivoa.obscore_hea`. Line 407 recommends `NATURAL JOIN`; the Radio extension does the same for `ivoa.obscore_radio`.

[] **Problem.** A service carrying both extension tables will natural-join on every shared column name, not just `obs_publisher_did`. Two extensions defining the same column with possibly drifting semantics is a maintenance problem.

[] **Options.**
1. Move `t_intervals`, `tracking_type`, `scan_mode` to a common ObsCore update (coordinate with Radio and DM WG).
2. Keep them in both extensions, guarantee identical definitions by reference, and replace `NATURAL JOIN` with `JOIN … USING (obs_publisher_did)`.

[] **Proposed resolution.** Option 1 for the long run; option 2's explicit join syntax immediately.

---

### B9. Gamma-ray UCD bands: narrowing `em.gamma.hard`, the HE/VHE boundary, the MeV gap
*Labels: discussion, ucd*

[] **Context.** Lines 633–635, 678 propose `em.gamma.hard` = 500 keV–100 MeV, `em.gamma.he` = 100 MeV–10 GeV, `em.gamma.vhe` = 10 GeV–100 TeV, `em.gamma.uhe` > 100 TeV. UCD list 1.6 has `em.gamma.hard` as ">500 keV".

[] **Problem.** Narrowing an existing term mis-tags every column already annotated `em.gamma.hard`. The 10 GeV HE/VHE boundary is non-standard (convention: 100 GeV). The MeV band (COMPTEL, COSI, AMEGO-X) is silently folded into `hard`.

[] **Proposed resolution.** Add narrower terms without redefining the parent; use 100 GeV unless a citation justifies 10 GeV; ask the MeV community whether `em.gamma.me` (or similar) is wanted. Submit as a VEP.

---

### B10. `instr.detection` duplicates the existing `instr.det` root
*Labels: discussion, ucd*

[] **Context.** Line 579 proposes `instr.detection` as a new base for event-related UCDs. UCD list 1.6 already has `instr.det` ("Detector") with `instr.det.psf`, `instr.det.qe`, `instr.det.noise`.

[] **Problem.** Two sibling roots with the same stem.

[] **Proposed resolution.** Either justify the new root in one sentence, or propose `instr.det.event` (or similar) under the existing branch.

---

### B11. Photometric UCDs: `phot.count` redefinition, fate of `phot.flux`, radiance vs surface brightness duplicates
*Labels: discussion, ucd*

[] **Context.** Lines 641–653, 679 add `phot.count.*`, `phot.flux.energy.*`, `phot.flux.particle.*` and attach dimensionalities; line 679 gives `phot.count` the dimensionality [L^-2 T^-1].

[] **Problem.**
- Nearly every X-ray catalogue uses `phot.count` for plain net counts or count rates; a hard [L^-2 T^-1] re-labels the installed base as wrong.
- `phot.flux.particle` ("Particle flux or irradiance") is the existing `phot.flux` description ("Photon flux or irradiance") with one word changed, and the document says nothing about what `phot.flux` and `phot.flux.density` (every Jy column in the VO) mean afterwards.
- `phot.count.radiance` vs `phot.count.sb`, and `phot.flux.particle.radiance` vs `.sb`, have identical dimensionality and near-identical descriptions.

[] **Proposed resolution.** Do not attach dimensionality to existing terms. State explicitly that `phot.flux` and `phot.flux.density` remain the generic terms and the new ones are refinements. Collapse radiance/sb to one term per quantity. Submit as a VEP with the Semantics WG; keep the dimensionality column as informative.

---

### B12. Deprecating `phys.electron` and `phys.electron.degen`
*Labels: discussion, ucd*

[] **Context.** Lines 601–605, 684 propose marking both obsolete and adding `phys.particle.electron`, then say "the IVOA should evaluate the benefit/cost ratio".

[] **Problem.** `phys.electron.degen` is the electron degeneracy parameter, a plasma/stellar-structure quantity unrelated to messengers. The hedged wording is not specification text.

[] **Proposed resolution.** Add `phys.particle.electron`; leave `phys.electron` and `phys.electron.degen` untouched; move any deprecation question to the VEP.

---

### B13. `t_intervals`: datatype, serialisation, xtype, and query path
*Labels: discussion, extension-table*

[] **Context.** Table line 428 gives type "TMOC" and UCD "TBD"; line 307 concedes ADQL cannot query it.

[] **Problem.** TMOC is not a VOTable/TAP datatype. No xtype is given. A discovery column that cannot be used in a discovery query needs at least a stated path forward.

[] **Proposed resolution.** Declare a string column carrying the MOC 2.0 ASCII serialisation with an explicit xtype; name the prototyped UDF and state the intent to propose it to the ADQL UDF catalogue.

---

### B14. `energy_min`/`energy_max`: massive particles, consistency with `em_*`, naming
*Labels: discussion, extension-table*

[] **Context.** Line 311 adds eV-valued bounds and says `em_min`/`em_max` "are misleading" for massive particles, without saying what to put there.

[] **Problem.** Providers need a rule. Also consider whether a bare `energy_` prefix collides with future ObsCore use.

[] **Proposed resolution.** Normative text: for photon messengers both pairs MUST be present and consistent via λ = hc/E; for non-photon messengers `em_min`/`em_max` SHOULD be NULL. Decide `energy_*` vs `em_energy_*` naming once.

---

### B15. Polarization is not addressed
*Labels: discussion, extension-table*

[] **Context.** IXPE (2021–) produces event lists with per-event Stokes parameters; the document mentions polarization only as a model component (line 150).

[] **Proposed resolution.** Add guidance on `pol_states`/`pol_xel` and `o_ucd` for X-ray polarimetry event lists, or state explicitly that polarimetry is deferred.

---

### B16. Service requirements and registration are missing
*Labels: discussion, structure*

[] **Context.** Line 140 defers Registry topics to the Radio extension document; line 407 is the only service-level statement.

[] **Problem.** A standard cannot delegate normative content to another unpublished draft. The Radio extension has a full section: fixed table name; a service with the extension table MUST also have `ivoa.obscore`; registry record example.

[] **Proposed resolution.** Add the equivalent section, mirroring the Radio extension's wording.

---

### B17. `messenger_name` values vs the IVOA messenger vocabulary
*Labels: discussion, vocabulary, extension-table*

[] **Context.** Lines 378–385 cite the messenger vocabulary as a "starting point" then list `cosmic-ray`, `muon`, `neutrino`, `photon`, `proton`.

[] **Problem.** The vocabulary has only `Photon` (narrower `Radio`…`Gamma-ray`) and `Neutrino`, capitalised; none of the other values exist and the case does not match. `cosmic-ray` is not a species and overlaps `proton`; `muon` is a detection channel, not an astrophysical messenger; `electron` is missing although line 376 says H.E.S.S. detects electrons.

[] **Proposed resolution.** Make the vocabulary the controlled value set, verbatim. File VEPs for `Cosmic-ray` (narrower: `Proton`, `Electron`, nuclei…) and anything else needed. Drop `muon` unless a use case is written.

---

### B18. `messenger_pdgid` should be an integer
*Labels: discussion, extension-table*

[] **Context.** Line 393 makes it a string with a mandatory sign.

[] **Problem.** `messenger_pdgid = '13'` silently matches nothing; range queries are impossible; PDG IDs are integers by definition (negative = antiparticle).

[] **Proposed resolution.** Type `integer`, no sign convention text.

---

### B19. Observing-mode semantics: wobble, slewing encoded twice, fixed-az-el for non-pointing instruments
*Labels: discussion, extension-table*

[] **Context.** Line 343 maps IACT wobble to `on-the-fly-cross-map`; slewing appears as `tracking_* = none` (line 331) and `scan_mode = slew` (line 344); line 329 defines `fixed-az-el-transit` while line 333 says the attribute is not for non-pointing instruments.

[] **Problem.** Wobble is a fixed offset pointing, not a scan. Slewing should be owned by one attribute. HAWC and LHAASO are literally fixed-az-el instruments.

[] **Proposed resolution.** Add `wobble` to `scan_mode` with a correct definition (or map to `on-source` and say so; the Radio extension has a commented-out `Wobble` entry, so coordinate). Choose `scan_mode = slew` and drop `none` from tracking. State that non-pointing instruments SHOULD set `tracking_type = Fixed-az-el-transit`.

---

### B20. `measurements` and the ObsCore `obs_id` constraint
*Labels: discussion, obscore-core*

[] **Context.** Line 203 proposes deleting the ObsCore 1.1 caveat that `measurements` be exposed "together with the progenitor".

[] **Problem.** The same ObsCore paragraph also says "Progenitors and their derived data products must have the same obs_id", which is the constraint that actually breaks multi-observation products, and the document does not mention it.

[] **Proposed resolution.** Either address `obs_id` explicitly in the request to the ObsCore editors, or drop the `measurements` discussion entirely in favour of the new vocabulary terms.

---

### B21. Statistics terms: `draws`, `pdf`, ΔTS, `stat.distribution`, and alignment with Semantics
*Labels: discussion, vocabulary, ucd*

[] **Context.** Lines 189, 191, 524 list "the DeltaTS associated with a quantity" as an example of both `draws` and `pdf`; line 618 proposes `stat.distribution` for likelihood profiles; commented lines 615, 619–623 record open disagreement with the Semantics WG about whether `stat.distribution` is a UCD or a product type and whether the proposed `stat.*` terms match what was approved.

[] **Problem.** A ΔTS profile is a likelihood profile: not samples, not a normalised density. The role of `stat.distribution` is unresolved in the authors' own comments.

[] **Proposed resolution.** Define TS, use ΔTS under `pdf` or a likelihood-profile term only. Resolve `stat.distribution` with Semantics before publication. Reconcile `stat.error.minus/plus`, `stat.lowerlimit/upperlimit`, `stat.confidenceLevel` P→Q with whatever Semantics has approved since UCD list 1.6.

---

### B22. Where do the new product terms live, and what goes in `dataproduct_type` vs `dataproduct_subtype`?
*Labels: discussion, vocabulary*

[] **Context.** Line 186: `draws`, `pdf`, `region` as `dataproduct_type` "(or `dataproduct_subtype`)". Line 531: proposed to a separate "advanced data product type vocabulary". Lines 209 and 515: `aeff`, `edisp`, `psf` as `dataproduct_subtype` values and as narrower terms in the `response-type` vocabulary.

[] **Problem.** For a concrete file (a Chandra ARF, a Fermi PSF, a CSC aperture-photometry pdf) the document never states what `dataproduct_type` and `dataproduct_subtype` contain.

[] **Proposed resolution.** One table: term · vocabulary it lives in · goes in `dataproduct_type` or `dataproduct_subtype` · example. Add worked examples for three real files.

---

### B23. MIME type scheme for HE FITS flavours
*Labels: discussion, vocabulary*

[] **Context.** Lines 707–709 propose `x-fits-ogip`, `x-fits-gadf`, `x-fits-vodf`.

[] **Problem.** Beyond the missing `type/` (Part A), the `x-` prefix was deprecated by RFC 6648; ObsCore already uses a parameter for DataLink (`application/x-votable+xml;content=datalink`); and "OGIP" covers events, PHA, ARF, RMF, GTI and light curves, so a single OGIP type does not help a client choose a tool, which was the stated motivation (line 237).

[] **Options.**
1. `application/fits;content=ogip-events` etc. (parameter, consistent with DataLink precedent).
2. New subtypes registered or vendor-tree (`application/vnd.ivoa.fits-ogip`).

[] **Proposed resolution.** Option 1, with granularity matching the formats clients actually dispatch on; also say whether `image/fits` takes the same parameter.

---

### B24. Core-ObsCore changes should be routed as such
*Labels: discussion, obscore-core, structure*

[] **Context.** §3 proposes changes to ObsCore 1.1 text for `dataproduct_type`, `measurements`, `s_ra`/`s_dec`, `o_ucd`, `proposal_id`.

[] **Problem.** An extension document cannot amend the base Recommendation. These are requests to the ObsCore editors (or ObsCore 1.2) and should be marked and tracked as such, separately from the extension's own normative content.

[] **Proposed resolution.** Move them to a clearly labelled "Requested changes to ObsCore" section (or appendix) with one line each, and open corresponding issues on the ObsCore repository.

---

### B26. Decide between Proposals 1, 2 and 3 (where response functions and advanced products live)
*Labels: discussion, structure, vocabulary*

[] **Context.** `UseCases.tex` lines 16–57 lay out three architectures: (1) everything in `ivoa.obscore`, `dataproduct_type` drawn from any of three vocabularies; (2) new terms in `product-type`, response types via `dataproduct_subtype`; (3) three tables, `ivoa.obscore`, `ivoa.response`, `ivoa.adp`, one vocabulary each. The appendix says each "needs to be evaluated in various situations and explored further". The use cases are then written against different proposals: A.1.3 and A.1.9 use `dataproduct_type = 'aeff'` (Proposal 1); A.1.8 and A.1.13 use `dataproduct_type = 'response-function' AND dataproduct_subtype = 'psf'` (Proposal 2); Appendix B sketches Proposal 3.

[] **Problem.** The main text never tells the reader this choice is open. It is the concrete form of B22, and a document proposed for endorsement cannot leave it open. The response-type vocabulary already exists on GitHub with `aeff` etc. as narrower terms of `response-function`, which fits Proposal 1 (or Proposal 2 if `response-function` is the type and the narrower term the subtype) but not both at once.

[] **Proposed resolution.** Choose Proposal 2 as the ObsCore-1.1-compatible default (type = `response-function`, subtype = narrower term), state it normatively in Section 3.1, and rewrite A.1.3, A.1.9 and Appendix B to match. Keep Proposal 3 as a non-normative appendix only if someone commits to prototyping it.

---

### B27. Synchronise the main text with the VEP files in the repository
*Labels: discussion, ucd, vocabulary*

[] **Context.** The repository's own proposals disagree with the document: `VEP-UCD-28` proposes a `phys.flux` branch while the text proposes `phot.flux.energy`/`phot.flux.particle`/`phot.count.*`; `VEP-UCD-30` proposes `stat.error.negative`/`positive` (or `.below`/`.above`) while the text has `.minus`/`.plus`; `VEP-UCD-31` proposes `obs.event` (S) while the text has `instr.detection` (Q) and a different event-type UCD; `VEP-UCD-27` has no P→Q change for `stat.confidenceLevel`; `VEP-analysis-products.txt` sends `draws`/`pdf` to `analysis-product-type` and `region` to `product-type`, while line 531 lumps all three together.

[] **Problem.** Reviewers will read both. Whichever is current must win everywhere, and several VEP files have blank "Decision:" fields or "remove this suggestion" notes that the text does not reflect.

[] **Proposed resolution.** Make the VEP files the source of truth, update the text's tables from them, and reference each VEP by number from the relevant table row. Fold B11, B12 and B21 into this once the names settle.

---

### B28. Cross-vocabulary `broader` relations in the summary table
*Labels: discussion, vocabulary*

[] **Context.** `Dataproducts-Summary-table.tex` gives `rmf`, `edisp`, `psf` the parents `#response-function, #pdf`, and `hea-event-list` the parent `#temporally-resolved-dataset`.

[] **Problem.** `pdf` is in a different vocabulary; IVOA vocabulary tooling has no mechanism for `skos:broader` across vocabularies, and the published `response-type/terms.csv` has no such relation. The published `event-list` has no broader term, and an event list is equally spatially and spectrally resolved.

[] **Proposed resolution.** Drop `#pdf` as a parent (or make it a `skos:related` note in the description). Propose any parent for `event-list` via VEP, not in this table.

---

### B29. `s_fov` is a diameter; neutrino field-of-view queries use it as a radius
*Labels: discussion, extension-table*

[] **Context.** A.1.10 and A.1.11 use `CIRCLE('ICRS', s_ra, s_dec, s_fov)`; A.1.12 uses a fixed `80` degrees.

[] **Problem.** ObsCore 1.1 defines `s_fov` as the diameter of the containing circle, so these queries cover four times the intended area. More fundamentally, for a detector whose instantaneous field of view is a hemisphere swept by Earth rotation, neither `s_fov` nor a circle is the right representation; `s_region` is. This ties to §3.6 (`s_ra`/`s_dec` for non-pointing instruments).

[] **Proposed resolution.** Add one paragraph to §3.6 stating what non-pointing and all-sky instruments SHOULD put in `s_ra`, `s_dec`, `s_fov` and `s_region`, then fix the three use cases to use `s_region`.

---

### B30. `TMOC()` function and `dl_association_id` column appear only inside SQL examples
*Labels: discussion, extension-table*

[] **Context.** A.2.3 (line 531) calls `TMOC(27, t_intervals)` and `TMOC(27, 56320.0, 56325.0)`; Section 6 (line 192) introduces a `dl_association_id` column on `ivoa.obscore`.

[] **Problem.** Neither is defined anywhere. The `TMOC` UDF is the prototype mentioned in §4.3's footnote; `dl_association_id` is a new ObsCore column proposed in passing.

[] **Proposed resolution.** Define the UDF (name, signature, semantics, intended ADQL UDF-catalogue entry) in §4.3 and reference it from A.2.3, or mark the query as illustrative pseudo-ADQL. Either define `dl_association_id` in §4 or remove the example.

---

### B31. Use cases join `ivoa.obscore_hea` without using it
*Labels: discussion, structure*

[] **Context.** A.1.1, A.1.4, A.1.8, A.2.1, A.2.2, A.2.4 all `NATURAL JOIN ivoa.obscore_hea` but constrain only base ObsCore columns; an editor comment at `UseCases.tex` line 470 asks why.

[] **Problem.** The examples teach that the join is always needed, which it is not, and they obscure which use cases actually motivate the extension (only A.1.2, A.1.3, A.1.5, A.1.6, A.1.7, A.1.11–A.1.13, A.2.3 use an extension column).

[] **Proposed resolution.** Remove the join where unused; add a sentence to the appendix introduction saying the join is only required when extension columns are constrained or selected.
