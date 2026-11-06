# Review: "Roadmap to IVOA ObsCore Extension and Discovery of High Energy Astrophysics Data"

Reviewed source: `HighEnergyObsCoreExt-2.tex` (767 lines). Line numbers below refer to that file.

**Not reviewed** (files not supplied): `Dataproducts-Summary-table`, `extendedAccessTonewtypesOfproducts.tex` (Section 6), `UseCases.tex` (Appendix A), `extendedAccessTonewtypesOfproductsAppendix.tex` (Appendix B).

**External sources checked** (GitHub `master` as of 2026-11-05): `ivoa-std/ObsCore` (ObsCore 1.1 source), `ivoa-std/ObsCoreExtensionForRadioData`, `ivoa-std/ObsLocTAP`, `ivoa-std/Vocabularies` (`product-type`, `messenger`, `response-type`), `ivoa-std/UCDList` (UCD list 1.6, 2024-12-18). The `ivoa.net` site and the GitHub API were not reachable from this environment, so the *published* state of the vocabularies could not be confirmed; the repository state is what is cited below.

---

## 1. Overall verdict

The document is a position paper wearing the clothes of a specification. It uses the `ivoa` class, the RFC 2119 boilerplate, and a normative-looking attribute table, but almost every proposal is phrased as "we propose", "we suggest", "we note in passing", "the data provider should ultimately decide". There is not a single MUST or SHOULD in the body. The title ("Roadmap to…") and the closing section ("Contributions to the Note") say it is a Note; the structure says it wants to be a Working Draft.

**Decide which it is.** If it is a Note, drop the RFC 2119 section and the attribute table's "MAN" column. If it is a Working Draft for an extension standard, then:

- Every proposal becomes a normative statement with RFC 2119 language, or it is cut.
- Definitions appear once, in one place, and are copied verbatim from (or into) the controlling vocabulary.
- Motivation is one or two sentences per attribute, not a paragraph.
- Material that is advice to the ObsCore editors (Section 3) is separated from material that defines the extension table (Section 4).

Under that discipline, the body shrinks by roughly half with no loss of normative content. Section 7 below gives a target outline.

Beyond the prose problem, there are factual errors in cross-references to other IVOA documents, several internal contradictions, and a handful of physics and notation errors. Those are listed first, because they would be caught by any RFC reviewer and will cost the document credibility.

---

## 2. Factual errors and contradictions (verified)

Each item: where, what is wrong, evidence, fix.

### 2.1 `event-list` and `event-bundle` already exist in the Data Product Type vocabulary

Lines 177–182, 465, 473–485. The document "proposes to add" `hea-event-list` and `hea-event-bundle`, and says it will "redesignate" `event-list` as `hea-event-list`.

The `product-type` vocabulary already contains:

```
event-list;2;Event List; A collection of observed events, such as incoming high-energy
  particles. A row in an event list is typically characterised by a spatial position,
  a time and an energy.
event-bundle;3;Event Bundle;An event list with associated calibration files.;ivoasem:preliminary
```

The proposed `hea-event-list` definition is a light paraphrase of the existing `event-list` definition. `event-bundle` is already there, flagged preliminary, presumably at this group's own request. The document neither acknowledges this nor explains why a published term should be renamed.

The stated justification (line 184: confusion with VOEvent) does not hold. VOEvent is not a `dataproduct_type` value, and `event-list` already disambiguates from a bare `event`. Meanwhile the document argues (lines 186, 521) that `draws`, `pdf`, `region`, `response-function` are deliberately *not* domain-prefixed because they are waveband-independent. Event lists are also not HE-specific (photon-counting optical detectors, HST TIME-TAG, MKIDs all produce them). The `hea-` prefix is inconsistent with the document's own reasoning.

**Fix:** use `event-list` and `event-bundle` throughout. Propose, via VEP, any wording change to `event-list` and the promotion of `event-bundle` from preliminary. Delete lines 184 and the duplicate definitions (see Section 4).

### 2.2 The Radio extension's attribute is `tracking_type`, not `tracking_mode`

Line 333: "We intend that the name of this attribute harmonizes with the name of the similar attribute in the proposed IVOA Obscore Extension for Radio Data".

The Radio extension source defines `tracking_type` (its Table of instrumental attributes, and the text: "The ObsLocTAP specification defines the term *tracking_type* for describing this as well as a vocabulary for these modes. We include the same terms here"). ObsLocTAP 1.0 likewise defines `tracking_type` with values `Sidereal`, `Solar-system-object-tracking`, `Fixed-az-el-transit` (capitalised).

So: the name does not harmonise, the values differ in case, and the real source of the vocabulary is ObsLocTAP, which this document never cites.

**Fix:** rename to `tracking_type`, cite ObsLocTAP as the source, adopt its values verbatim (case included), and propose `none` to ObsLocTAP if it is really needed (see 3.6 on whether it is).

### 2.3 `on-the-fly-cross-map` does not exist in the Radio extension

Lines 342–346: the document lists `on-the-fly-cross-scan` ("observations along a predefined spatial pattern") and `on-the-fly-cross-map` ("observations along parallel directions"), and claims "the first five predefined values for scan_mode also harmonize with that proposal".

The Radio extension defines:

```
on-the-fly-cross-scan   successive continuous measurements along a predefined spatial
                        cross-shaped pattern
on-the-fly-map          successive continuous measurements along parallel directions
```

`on-the-fly-cross-map` is a conflation of the two radio terms. The definition given for `on-the-fly-cross-scan` also drops the word "cross-shaped", which is the only thing that distinguishes it from `raster-map`.

**Fix:** copy the Radio extension's terms and definitions verbatim. Add only what HE genuinely needs.

### 2.4 Wobble observations are not on-the-fly maps

Line 343: "`on-the-fly-cross-map`: observations along parallel directions (e.g., a wobble observation for IACTs)".

IACT wobble mode is a *fixed* pointing offset (typically 0.5–0.7°) from the target, held for a run and alternated between runs. The telescope does not scan during the run. It is closer to `on-off` (with the "off" taken from the same field) than to any on-the-fly mode. Labelling it as a continuous map will mislead every non-IACT reader and annoy every IACT reader.

**Fix:** either add `wobble` as an HE-specific value with a correct definition, or map wobble to `on-source` and say so. The Radio extension source has a commented-out `Wobble` entry, so this was considered there too; coordinate.

### 2.5 `pos.posAng` is the wrong UCD for an off-axis angle

Table rows for `s_ref_oaa` and `em_ref_oaa` (lines 424, 426): UCD `pos.posAng;instr.offset;pos`.

`pos.posAng` is "Position angle of a given vector" (east of north). An off-axis angle is an angular distance from the optical axis. The UCD list already has exactly the right word: `instr.offset` (Q) "Offset angle with respect to main direction of observation".

**Fix:** `instr.offset` (optionally `instr.offset;pos` and `instr.offset;em`, though the trailing axis qualifier adds little).

### 2.6 `messenger_name` values do not match the messenger vocabulary they cite

Lines 378–385. The document says the IVOA Messengers vocabulary is "a useful starting point", then lists `cosmic-ray`, `muon`, `neutrino`, `photon`, `proton`.

The `messenger` vocabulary contains exactly two top-level terms, `Photon` (with narrower `Radio` … `Gamma-ray`) and `Neutrino`, both capitalised. None of `cosmic-ray`, `muon`, `proton` exist, and `photon`/`neutrino` in lower case do not match the identifiers. A query `messenger_name = 'photon'` would not match a vocabulary-conformant value.

Also: `cosmic-ray` is not a particle species, it overlaps with `proton`; `muon` as a *messenger* is odd (for neutrino telescopes muons are the detection channel, not the astrophysical messenger); and `electron` is missing even though line 376 says H.E.S.S. detects electrons.

**Fix:** adopt the messenger vocabulary verbatim as the controlled value set, and submit VEPs for `Cosmic-ray` (with narrower `Proton`, `Electron`, nuclei…) if those are needed. Do not define a parallel list.

### 2.7 PDG ID format `±nn` is wrong

Line 393: "The value should be a string that specifies the messenger particle's PDG ID as `±nn` (where the sign is mandatory)".

PDG Monte Carlo numbers are integers of up to ten digits (proton 2212; nuclei 10LZZZAAAI, e.g. ⁵⁶Fe = 1000260560). "nn" implies two digits. Storing an integer as a string with a mandatory leading `+` also means `messenger_pdgid = '13'` silently matches nothing, and range queries are impossible.

**Fix:** type `integer`, no sign convention needed (negative = antiparticle by PDG definition). Cite the PDG "Monte Carlo Particle Numbering Scheme" (Review of Particle Physics) rather than a third-party BNL page (lines 391, 599).

Also line 395: the tau antineutrino is $\bar\nu_\tau$, not $\nu_\tau^{-}$.

### 2.8 ObsCore 1.1 says o_ucd for event lists *must* be NULL

Line 271: "The current ObsCore Recommendation suggests that o_ucd be set to NULL for event lists."

ObsCore 1.1 §4.? (source line 921): "In the case of event lists all components could be considered as observables prior to sampling, then o_ucd must be left [NULL]". It is a MUST, not a suggestion. The proposal in 3.9 therefore requires a change to the core Recommendation, not an extension, and should be labelled as such. (See also 3.3 on why the hash-list design is a poor fit.)

### 2.9 Deleting the "together with the progenitor" caveat does not free `measurements`

Lines 203. The document proposes deleting the ObsCore 1.1 caveat that `measurements` products be exposed together with the progenitor. The source also says, two sentences later: "Progenitors and their derived data products must have the same obs_id." That is the constraint that actually breaks multi-observation advanced products, and the document does not mention it.

**Fix:** address `obs_id` explicitly, or drop the whole discussion and simply propose the new vocabulary terms (which is what §5.1.4 does anyway).

### 2.10 ObsCore 1.1 already answers the `ev_xel` question differently

Line 295. ObsCore 1.1 (source line 942) says for event lists the *xel attributes are −1 and "the number of elements in such lists is a different property and should be represented in the NDCube DM". The document should acknowledge that it is overriding this guidance and say why a discovery column is preferable.

### 2.11 Internal contradictions

- **When was the HEIG formed?** Line 138: "formed in the Fall of 2024". Line 760: "2023-05-11: IVOA Bologna meeting … first IVOA HE group meeting".
- **Neutrino energy floor.** Line 138: "astrophysical neutrinos above the TeV range". Line 146: "astrophysical neutrino (≳ GeV) detectors". KM3NeT/ORCA works at GeV.
- **ARF expansion.** Glossary line 74 and the `response-type` vocabulary: "Ancillary Response File". Line 195: "Auxiliary Response File". OGIP says Ancillary.
- **`on-source` vs `on-sources`.** Line 339 vs table line 438.
- **Slewing is encoded twice.** `tracking_mode = none` (line 331, "e.g., observations obtained while the telescope is slewing") and `scan_mode = slew` (line 344). One attribute should own this.
- **Fixed-az-el instruments.** Line 329 defines `fixed-az-el-transit`, line 333 says the attribute "is not expected to be used for non-pointing instruments". HAWC and LHAASO are, literally, fixed-az-el instruments.
- **Where do the advanced-product terms live?** Line 186: proposed as `dataproduct_type` "(or `dataproduct_subtype`)". Line 531: proposed to a separate "advanced data product type vocabulary". Lines 209/515: `aeff`, `edisp`, `psf` are `dataproduct_subtype` values *and* narrower terms of `response-function` in the `response-type` vocabulary. The document never states, for a concrete file (say a Chandra ARF), what `dataproduct_type` and `dataproduct_subtype` should contain.
- **Separators for multi-valued columns.** `#` for `o_ucd` (line 273), comma for `proposal_id` (line 286).
- **Shared vs HE-specific attributes.** Line 140 states the rule: shared attributes go to base ObsCore, HE-specific ones to the extension. `t_intervals` (line 305, explicitly "any waveband"), `tracking_mode`/`scan_mode` (shared with Radio and ObsLocTAP), and arguably `messenger_name` violate the rule and sit in the extension table.
- **Author name.** "S. Hallmann" (line 25) vs "S. Hallman" (line 752).

### 2.12 Physics, notation and terminology slips

- Line 97: CTI is Charge Transfer **Inefficiency**. CTE is the efficiency.
- Line 243: "2π Sr": the symbol is `sr`; 2π sr is a hemisphere, not "all-sky"; a hemisphere swept by Earth rotation is a declination band, not "a rotated area".
- Line 311: wavelength exponents "10⁻⁵–10⁻²²". 0.1 keV is 1.2×10⁻⁸ m; 1 PeV is 1.2×10⁻²¹ m. 10⁻⁵ m is 0.1 eV.
- Line 313: since h, c and e are exact, hc/e is exact; "based on the 2022 CODATA list" is unnecessary. State the value as exact to the digits shown.
- Line 189, footnote: confidence contours in the (Ω_M, Ω_Λ) plane are not "draws". They are contours of a likelihood or posterior. The example is wrong and off-topic. Also `$\Lambda CDM$` and `$\Omega M$` render as italic products; use `\Lambda\mathrm{CDM}`, `\Omega_{\mathrm M}`.
- Lines 189, 191, 524: "the DeltaTS associated with a quantity" is listed as an example of both `draws` and `pdf`. A ΔTS profile is a likelihood profile: not a set of samples, and not a normalised density. Define TS, write ΔTS, and put the example under `pdf` only (or under the proposed `stat.distribution`, which is where line 618 puts likelihood profiles).
- Line 585: "background signal (both of instrumental and cosmological origin)" → "astrophysical" or "cosmic-ray".
- Line 585: `stat.fwmh` → `stat.fwhm`.
- Line 597: "AMS-2" → AMS-02. Fermi-LAT has no magnet; it separated e⁺ from e⁻ only via the geomagnetic-shadow technique. The canonical examples are PAMELA and AMS-02.
- Line 654: `phys.particle.antiprotron` → `antiproton`.
- Line 560: `phot.counts` → `phot.count`; "erg cm⁻² s⁻¹ TeV" is not a spectral flux density (missing ⁻¹).
- Line 680: proposed `phot.fluence` description says "radiant photon energy … per unit area" but gives dimensionality [L⁻²]. Energy per area is [M T⁻²]; [L⁻²] is a *number* fluence. One of the two is wrong.
- Lines 643/644 and 652/653: `phot.count.radiance` and `phot.count.sb` are given identical dimensionality [L⁻² T⁻¹ sr⁻¹] and near-identical descriptions; likewise `phot.flux.particle.radiance` vs `.sb`. Two UCDs for one quantity is a defect; pick one.
- Line 376: "H.E.S.S. and IACT are nominally gamma-ray instruments": IACT is a class. "IACTs such as H.E.S.S.".
- Lines 160, 162, 205, 542: footnote URL `https://www.ivoa.net/rdf/product_type` (underscore). The vocabulary is `product-type` (hyphen), which line 463 gets right (with `http`). Four wrong URLs.
- Line 531: a normative reference to a GitHub pull-request *diff anchor* is not acceptable. Cite the VEP number.
- Line 707: "OGIP specialized format … used in legacy high energy surveys at HEASARC". OGIP formats are the current standard for essentially every X-ray mission (Chandra, XMM-Newton, NuSTAR, Swift, NICER…). "Legacy" is wrong and will irritate the X-ray community.
- Lines 707–709: `x-fits-ogip` etc. are not MIME types. A MIME type is `type/subtype`. Also the `x-` prefix was deprecated by RFC 6648 (2012). Given that ObsCore already uses a parameter for DataLink (`application/x-votable+xml;content=datalink`), the consistent form is `application/fits;content=ogip` (or similar), and it should be stated whether this applies to `image/fits` as well.
- Line 760: URL path `IntropMay3023DM` (sic). Verify; if the wiki path really is misspelled, say so.

---

## 3. Design objections (antagonistic reviewer)

### 3.1 Two terms for each response component, with no stated distinction

§5.1.3 defines `aeff` and `arf` (both "effective area as a function of energy") and `edisp` and `rmf` (both "pdf from true energy to measured energy/channel"). The actual difference is community and serialisation: OGIP ARF/RMF are on a channel grid for a specific extraction; GADF AEFF/EDISP are on true-energy × offset grids. The document never says this. A reviewer will ask why a *discovery* vocabulary should carry a format distinction at all, and if it should, why the distinction is not expressed via `access_format`. Either collapse to one term each, or write one sentence per pair explaining the difference and that it is intentional.

### 3.2 `hea-event-bundle` has no defined serialisation

Lines 181, 233, 237, 484. A "compounded dataset containing … multiple files or other substructures" is proposed as a `dataproduct_type` and as the target of an `access_url`, but the document never says what a client receives: a tar, a zip, a multi-extension FITS file, a directory listing, a DataLink document? Line 237 argues clients need to know bundle content "to send the data to an appropriate visualization tool", and then §5.5 proposes MIME types per *file format*, not per bundle. Either define the container (or say it is unconstrained and that DataLink is the recommended alternative), or drop the type and use DataLink `#this` + `#calibration`.

### 3.3 Hash-separated multi-valued `o_ucd` is the wrong tool

§3.9. Packing N observables into one string column, then saying (line 275) that `o_unit`, `o_calib_status` and `o_stat_err` would all need the same treatment, is the signature of a relational model being forced into one row. The document itself proposes an extension *table* joined on `obs_publisher_did`; the obvious design is a second child table (one row per dataset per observable: ucd, unit, calib_status, stat_err). That needs no change to ObsCore 1.1's MUST on `o_ucd`, no `ivo_hashlist_has`, and no `LIKE '%string%'` (which matches `phot.flux` against `phot.flux.density`). If the authors reject this, the document should say why.

Also, a `#` inside a column whose own UCD is `meta.ucd` will fail any UCD validator.

### 3.4 Free-text "mode" attributes are not discovery attributes

`obs_mode`, `analysis_mode`, `event_type`, and in practice `array_pointing_mode` have no controlled vocabulary. The sentence "permissible values will vary from facility to facility and from instrument to instrument" appears eight times (lines 319, 333, 346, 357, 363, 370, 386, 399). A cross-facility discovery query cannot be written against them. That is acceptable for intra-facility filtering, but say so once, honestly, in one sentence at the top of Section 4, and delete the eight repetitions. The table placeholder "(e.g., TBU)" for `obs_mode` (line 434) must be filled or the row dropped.

`event_type` has a further problem: Fermi-LAT event types are *multiple orthogonal partitions* (FRONT/BACK, PSF0–3, EDISP0–3) and a dataset may contain PSF2+PSF3. A single-valued string cannot express that.

### 3.5 `convergent[-ang]` embeds a number in an enumerated value

Lines 354–357. `convergent-5d` is neither queryable as a mode nor as an angle. Use `array_pointing_mode` (enumerated) plus `array_pointing_angle` (double, deg).

### 3.6 Duplicated column names across extension tables will bite `NATURAL JOIN`

Line 407 recommends `NATURAL JOIN` between `ivoa.obscore` and `ivoa.obscore_hea`. The Radio extension recommends the same for `ivoa.obscore_radio`. If both extensions define `scan_mode` (and the HE document wants `tracking_mode` to "harmonise"), a service carrying both tables will have a three-way natural join keyed on `scan_mode` too. Either use `JOIN … USING (obs_publisher_did)` explicitly, or, better, move the genuinely shared attributes (`t_intervals`, `tracking_type`, `scan_mode`) to a common ObsCore update as line 140 says they should be.

### 3.7 Narrowing `em.gamma.hard` breaks existing annotations

Line 678 proposes `em.gamma.hard` = 500 keV–100 MeV. UCD list 1.6 has it as ">500 keV". Every Fermi/IACT column tagged `em.gamma.hard` today would become mis-tagged. New narrower terms (`em.gamma.he`, `.vhe`, `.uhe`) can be added without redefining the parent; do that instead.

Also the HE/VHE boundary at 10 GeV (lines 633–634) is non-standard. The usual convention is HE 100 MeV–100 GeV, VHE 100 GeV–100 TeV, UHE >100 TeV. If 10 GeV is deliberate (CTAO-LST threshold), cite it. The MeV "medium energy" band (COMPTEL, COSI) is silently folded into `hard`; that community may object.

### 3.8 `instr.detection` sits beside an existing `instr.det`

Line 579 proposes `instr.detection` as a new base. UCD list 1.6 already has `instr.det` ("Detector") with `instr.det.psf`, `instr.det.qe`, `instr.det.noise`. Two sibling roots with the same stem will confuse. Justify, or propose `instr.det.event` (or similar) under the existing branch.

### 3.9 Redefining `phot.count` and leaving `phot.flux` undefined

Line 679 gives `phot.count` the dimensionality [L⁻² T⁻¹]. Nearly every X-ray catalogue uses `phot.count` for plain net counts (dimensionless) or count rates (T⁻¹). Attaching a hard dimensionality to the existing term re-labels the installed base as wrong.

Line 649 adds `phot.flux.particle` = "Particle flux or irradiance", which is the existing description of `phot.flux` ("Photon flux or irradiance") with one word changed. The revised-entries table says nothing about what `phot.flux` and `phot.flux.density` (used for every Jy-valued column in the VO) mean after the change. This is the single most consequential UCD change in the document and it is not discussed.

### 3.10 Deprecating `phys.electron.degen` is unjustified

Line 602 proposes marking `phys.electron` *and* `phys.electron.degen` obsolete. The degeneracy parameter is a plasma/stellar-structure quantity with nothing to do with messenger particles. Line 604 then says "the IVOA should evaluate the benefit/cost ratio". A specification proposes or it does not; this is a VEP discussion, not standard text. Propose `phys.particle.electron`, leave `phys.electron` alone, and move the deprecation question to the VEP.

### 3.11 `t_intervals` has no datatype, no UCD, and no query path

Line 428: type "TMOC", UCD "TBD". TMOC is not a VOTable/TAP datatype; specify a string column, the MOC 2.0 ASCII serialisation, and the xtype. Fill the UCD. Line 307 concedes that no ADQL function can use the column; that is fine for a Note, but a standard that defines an undiscoverable discovery column should at least point to the UDF that was prototyped and say it is expected to be proposed to the ADQL UDF catalogue.

### 3.12 `energy_min`/`energy_max` and massive particles

Line 311 says `em_min`/`em_max` "are misleading when used for energy ranges of massive particles" and never says what to put there. State it: for non-photon messengers `em_min`/`em_max` SHOULD be NULL and `energy_min`/`energy_max` carry the range; for photons the two pairs MUST be consistent via λ = hc/E. Also consider whether `em_` prefixing (`em_energy_min`) would be clearer than a new bare prefix, and check for a collision with any future ObsCore use of `energy_*`.

### 3.13 Missing: polarization

IXPE has been flying since 2021 and produces event lists with per-event Stokes parameters. The document mentions polarization only as a model component (line 150). An HE extension that says nothing about `pol_states`/`pol_xel` for X-ray polarimetry event lists will be asked about it at RFC.

### 3.14 Missing: registration and service requirements

Line 140 defers "topics related to the Registry" to the Radio extension document. A standard cannot delegate its normative content to another unpublished draft. The Radio extension has a full section (table name is fixed; a TAP service with the extension table MUST also have `ivoa.obscore`; registry record example). This document has one paragraph (line 407). Add the equivalent.

### 3.15 Source still contains unresolved reviewer comments

Lines 198–199, 213–215, 534, 615, 619–623, 637–639, 659–664, 683 are internal editorial exchanges ("mireille: …", "There is no way to rewrite this paragraph…"). Several record *open disagreements* (whether `stat.distribution` is a UCD or a product type; whether the statistics terms match what Semantics approved). Resolve them or move them to an issue tracker; a "first public release" should not ship them.

---

## 4. What to cut (copy-editor)

Target: the body from §1 through §5 is roughly 8,000 words. A specification covering the same content needs about 3,500. The cuts below get most of the way there.

### §1 Introduction (lines 136–152)

- Line 138: delete the history ("formed in the Fall of 2024… formed a roadmap for the group… coincide with similar work being carried out by the Radio Interest Group, Time Domain Interest Group…"). Keep: what HEA is, and that this document extends ObsCore for it. Two sentences.
- Line 140: keep the one useful rule (shared attributes → base ObsCore; HE-specific → extension). Delete the rest.
- Line 152: delete entirely. It is a table of contents in prose, with "rational and well-considered proposals" as self-assessment. Replace with nothing; the section headings do this job. If kept, use `\ref` throughout: "sections 3--5" and "§5" (lines 152, 169, 209) are hard-coded.

### §2 HEA Data (lines 144–150)

- Line 146: cut the "domain is now sufficiently mature… open analysis tools (CIAO, Gammapy)" sentence and the inventory of science outputs. Keep the energy ranges and messengers (after fixing the TeV/GeV contradiction).
- Line 148: keep the event-counting paragraph but at a third of the length. Delete the footnote defining "signal integrating". Delete the facility footnote or move it to the glossary.
- Line 150: keep two sentences (IRFs map physics to observables; they are usually non-invertible so forward folding is used; hence IRFs must be discoverable). Delete the terminology footnote about historical use of "IRF" (if the term is avoided normatively, just avoid it).

### §3 ObsCore attribute definitions (lines 155–286)

- Lines 158–162: three paragraphs that say "we use ObsCore 1.1 as the baseline; some of this may move to vocabularies". Line 160 is a commented-out earlier draft of line 162. Replace with one sentence.
- §3.1 (lines 164–205): this is the worst offender.
  - Line 167 footnote on "observation": one clause in the text, no footnote.
  - Lines 171–175: do not quote the ObsCore 1.1 `event` definition in full; cite it.
  - Lines 177–182: delete (definitions live in §5.1; see 2.1 above).
  - Line 184 (VOEvent): delete.
  - Line 186: cut to two sentences. Delete the Chandra Source Catalog statistics, the "Bayesian X-ray aperture photometry… frequentist adjustment of models of electron populations" parenthesis, and the footnote on "advanced" vs "enhanced" vs "analysis" data products.
  - Lines 188–196: delete; these definitions are repeated verbatim at lines 523–529 and 495–513. Keep one copy, in §5.
  - Lines 198–199: delete (editor comments).
  - Line 201: delete. "An `hea-event-list` is in fact a specific type of tabular data… an `image` has two spatial axes but the observable axis is not defined…" is musing, not specification.
  - Lines 203–205: reduce to one normative sentence about `measurements` plus the `obs_id` point (2.9), or delete entirely in favour of the new terms.
- §3.2 `dataproduct_subtype` (lines 207–215): keep the recommendation and the `exposure-map` example in three sentences. Delete lines 213–215 (commented draft).
- §3.3 `calib_level` (lines 217–225): the net content is "set `calib_level` to 1 or 2 per the provider's convention and express per-axis status with the `*_calib_status` columns". Two sentences. Delete the quoted ObsCore definitions and the "between 1 and 2" discussion.
- §3.4 `access_url` and §3.5 `access_format` (lines 227–237): §3.4 restates standard DataLink usage; delete, or merge both into one three-sentence subsection that says bundles may be served directly or via DataLink and points to §5.5.
- §3.6 `s_ra`/`s_dec` (lines 239–243): keep the proposed wording change and one sentence of motivation. Delete the drift-scan footnote, the "2π Sr" paragraph, and the Alt/Az-to-RA/Dec speculation.
- §3.7–3.9 `s_calib_status`, `t_calib_status`, `em_calib_status` (lines 245–267): three subsections with the same three paragraphs, differing in one word. They propose nothing beyond ObsCore 1.1. Collapse to one subsection, "Calibration status attributes", of four sentences: which column is which; use the most-calibrated axis when several exist; a bundle inherits its event list's values; NULL where not applicable. Delete "(reasonable)" ×3 and "the data provider should ultimately decide" ×3.
- §3.10 `o_ucd` (lines 269–281): if the hash-list design survives 3.3, this is two paragraphs, not five. Delete line 277 (Chandra ACIS column count, the list of coordinate frames) and line 281 (Chandra Source Catalog MPDF example).
- §3.11 `proposal_id` (line 286): fine as is, one sentence.

### §4 Extension attributes (lines 290–407)

Each subsection should be: one sentence of motivation, the normative definition, the value list if any. Specifically:

- Line 295: delete "as the number of events often scales with signal-to-noise" (not true for background-dominated data; and irrelevant).
- Lines 299–301: fine, but say in the table which base attributes each reference value governs, and nowhere else.
- Line 305: cut by half; delete "albeit very sparsely", "transient cosmic phenomenon". Line 307 footnote: keep one clause.
- Line 311: delete "Unlike the radio domain where λ = c/ν, where c is an almost universally remembered physical constant, the conversion λ = hc/E is not simple for the user to express". The argument for the attribute is precision and massive particles, not arithmetic difficulty. Delete the parenthetical alternative design (`em_min_energy`); rejected designs do not belong in the spec.
- Lines 317–319 `obs_mode`: the Chandra continuous-clocking example is the one genuinely useful motivation in this section; keep it in one sentence.
- Lines 324–333 `tracking_*`: rewrite per 2.2; the explanatory paragraph at 333 reduces to "values follow ObsLocTAP".
- Lines 337–346 `scan_mode`: rewrite per 2.3/2.4; paragraph 346 reduces to one sentence.
- Lines 351–357 `array_pointing_mode`: delete "with a default value of NULL to handle WCD and neutrino instruments" (NULL is the default for any optional column); split out the angle (3.5).
- Lines 361–363 `analysis_mode`, 368–370 `event_type`: one sentence each plus the honesty statement from 3.4.
- Lines 376–399 `messenger_name`/`messenger_pdgid`: lines 395–397 (why both exist, "very few would be able to cast that query… from memory") are a paragraph of justification for a decision that needs one sentence: "`messenger_name` is for humans; `messenger_pdgid` is for precision."
- Line 403 "Additional Columns": fine.
- Table (409–454): fix "ObCcore", "on-sources", "TBU", "TBD", `pos.posAng`; make `messenger_pdgid` an integer; define "MAN" in the caption.

### §5 Vocabularies (lines 457–710)

- §5.1.1–5.1.2 (lines 471–487): these are now the *only* copies of the event-list/bundle definitions; keep, but copy the vocabulary text verbatim or state the proposed diff.
- §5.1.3 (lines 490–515): keep as the single home of the response-function terms; copy definitions verbatim from `response-type/terms.csv` (currently the two diverge: "photons" vs "particles" in `psf`, different `bkgrate` examples). Delete the duplicated "not separable into these distinct components" sentence (already at line 211).
- §5.1.4 (lines 517–531): line 519 and 521 are near-verbatim repeats of line 186. Delete the motivation; keep the three definitions (now the only copy) and a proper VEP reference.
- §5.1.5 "flux" (lines 539–562): the nine-item unit list (548–558) is a good table. The surrounding prose (544, 546, 560, 562) is three times longer than needed; the proposal is one sentence: "replace 'flux' with 'count, particle or energy flux' in the listed definitions".
- §5.3 UCDs (lines 571–694): the tables are the content. Each `\paragraph` should be at most two sentences introducing its table rows. Delete lines 583 (how PHA works), 601–605 (the hedged `phys.electron` discussion; see 3.10), 618 (why Gaussians are bad), 693–694.
- §5.5 MIME (lines 697–710): fix per 2.12; three sentences.

### Back matter

- §Contributions (lines 746–762): a Note may keep this; a standard should not have a dated list of meeting URLs in the body. Move to an acknowledgements paragraph or drop.

---

## 5. Line-level copy edits

| Line | Current | Change to |
|---|---|---|
| 5 | "{\em Roadmap to\/} IVOA ObsCore Extension and …" | "IVOA ObsCore Extension for High Energy Astrophysics Data" (if a WD) |
| 18 | "F. Bonnarel (IVOA, …)" | an institution, or "CDS (retired)" |
| 22 | "LUX -- ObsParis" | "LUX -- Observatoire de Paris" (consistency with 13, 20) |
| 23 | "Université" (literal) | `Universit\'e` (consistency; harmless in UTF-8 but mixed) |
| 97 | "Charge Transfer Efficiency" | "Charge Transfer Inefficiency" |
| 138 | "formed in the Fall of 2024" | reconcile with line 760 |
| 148 | "cosmic-rays" (noun) | "cosmic rays" (hyphenate only as adjective) |
| 148 | "{\bf hea-event-list}s" | pick one typographic convention for term names (`\texttt`) and use it everywhere; bold-in-running-text with plural "s" outside the bold is ugly |
| 152 | "make rational and well-considered proposals" | delete |
| 158 | "set to ``NULL''" | NULL without quotes; it is a SQL null, not a string |
| 181 | "a hea-event-bundle"; "compounded dataset" | "an …" (elsewhere "an hea-"); "compound dataset" |
| 186 | "$\sim\!90$ million files" | delete |
| 189 | "DeltaTS"; "$\Lambda CDM$"; "$\Omega M$" | "ΔTS (where TS is the test statistic)"; `$\Lambda$CDM`; `$\Omega_{\mathrm M}$` |
| 195 | "Auxiliary Response File" | "Ancillary Response File" |
| 243 | "$2\pi\,\rm Sr$" | "$2\pi$ sr" and rewrite the sentence |
| 249, 257, 265 | "Under the (reasonable) assumption" | "Assuming" |
| 273 | "`pos.eq\#time\#instr.pulse;arith.sum'" | see 3.3; if kept, note it is not a valid UCD string |
| 311 | "$\sim\!10^{-5}$--$10^{-22}$" | "$10^{-8}$ to $10^{-21}$ m" |
| 313 | "based on the 2022 CODATA list…" | "(exact; h, c and e are defined constants)" |
| 329 | "elevation and azimuth" | match ObsLocTAP wording |
| 342–343 | `on-the-fly-cross-scan` / `on-the-fly-cross-map` | Radio terms verbatim; see 2.3 |
| 351 | "multiple telescope simultaneously" | "multiple telescopes simultaneously" |
| 376 | "\gls{HESS} and \gls{IACT} are nominally gamma-ray instruments" | "IACTs such as H.E.S.S." |
| 391 | "the particle astrophysics community … use" | "uses" |
| 393 | "`±nn'" | integer; see 2.7 |
| 395 | "$\nu_\tau^{-}$" | "$\bar\nu_\tau$" |
| 397 | "PDG ID very unlikely to be recognized" | "a PDG ID is very unlikely…" |
| 403 | "include additional columns to the" | "include additional columns in the" |
| 430 | "ObCcore" | "ObsCore" |
| 430, 432 | "Energy associated to the ObsCore attribute em_max, describing the minimal energy" | "Minimum energy of the dataset (corresponds to em_max)" |
| 434 | "(e.g., TBU)" | fill or delete |
| 438 | "on-sources" | "on-source" |
| 463 | "http://www.ivoa.net/rdf/product-type" | "https://…" (and fix the four underscore URLs) |
| 496/499 | "response-function" / "response function" | one spelling |
| 560 | "phot.counts"; "erg cm⁻² s⁻¹ TeV" | "phot.count"; "erg cm⁻² s⁻¹ TeV⁻¹" |
| 566 | "we proposed to expose… Each Datalink is described" | "we propose to expose… Each DataLink link is described" |
| 585 | "stat.fwmh"; "cosmological origin" | "stat.fwhm"; "astrophysical origin" |
| 597 | "AMS-2" | "AMS-02" (and replace the Fermi-LAT example) |
| 654 | "phys.particle.antiprotron" | "phys.particle.antiproton" |
| 663 | "{\em stat.distribution \/}" | stray space before `\/` |
| 707 | "legacy high energy surveys" | "X-ray and gamma-ray missions" |
| 707–709 | "x-fits-ogip" etc. | `application/fits;content=ogip` or a proper `type/subtype`; see 2.12 |
| 748 | "contributed to the content and structure the text" | "…and structure of the text" |
| 752 | "S. Hallman" | "S. Hallmann" |

Recurring style points:

- "We note in passing", "we note that", "we recognize that", "we are aware that" appear more than twenty times. Each is a cue that the following sentence is either a normative statement that should be stated plainly, or an aside that should be deleted.
- `{\em e.g.\/}` with comma, then `\null` in some places and not others. Pick `e.g.,` in roman and stop.
- Hard-coded section numbers (152, 169, 209) → `\ref`.
- NULL, −1 and string values are quoted inconsistently (``NULL'', `NULL', NULL).

---

## 6. LaTeX and source hygiene

- Line 35 and 37: `lscape` loaded twice; `pdflscape` already loads it. Keep `pdflscape` only.
- Line 47: `hyperref` is loaded by `ivoa.cls`; loading it again after `glossaries` is unnecessary and occasionally breaks `\gls` links.
- Lines 111, 720: `\makeglossaries`/`\printglossaries` commented out, so the `[toc]` glossaries option and the `\gls` machinery only serve to expand acronyms on first use. Either print the glossary or replace `\gls{X}` with plain text; the current state is the worst of both (fragile markup, no glossary).
- Glossary entries that appear unused in the main body (they may be used in the missing inputs): ACIS is used; CDA, CTI, EPIC, ESAC, SOC, SSC, SAS, SVOM, ORCA, ARCA, MAGIC, VERITAS, FACT, LHAASO, CAOM, GW, TAP are not referenced in this file. Prune after the inputs are checked.
- Line 50–57: `\TODO` macro defined, never used in this file (`\todonotes` also loaded). Remove before public release.
- Editorial comments listed in 3.15: remove.
- Citation keys to verify exist in `HighEnergyObsCoreExt.bib`: `2026ApJ..1005..116F`, `gammapy:2023`, `2019adds.book..283C`, `deil_2022_7304668`, `ogip_spectrum_1998`, `ogip_psf_2011`, `ogip_Fits_event`, `2023arXiv230813385K`, `2024ivoa.note.heig`, `2024ivoa.spec.1218C`, `2022ivoa.spec.0822E`, `2022ivoa.spec.0727F`.

---

## 7. Suggested outline for the trimmed document

1. **Introduction** (½ page): HEA scope; event-counting data and response functions in four sentences; the shared/specific partition rule; relationship to ObsCore 1.1, the Radio extension, ObsLocTAP and the vocabularies.
2. **Use of existing ObsCore attributes for HEA data** (2 pages): one short subsection per attribute whose *interpretation* is clarified (`dataproduct_type`/`subtype`, `calib_level` + `*_calib_status`, `access_url`/`access_format`, `s_ra`/`s_dec`, `o_ucd`, `proposal_id`). Each: normative statement, one sentence of rationale. Clearly label which items are requests to the ObsCore editors (changes to 1.1 text) versus guidance to HE providers.
3. **The `ivoa.obscore_hea` extension table** (3 pages): table first, then one subsection per column with definition, type, UCD, unit, value set. Service requirements (table name, join key, co-existence with `ivoa.obscore` and `ivoa.obscore_radio`, registration).
4. **Vocabulary proposals** (2 pages): product-type diffs, response-type (verbatim from the vocabulary), messenger diffs, UCD tables, MIME types. Every item cross-referenced to a VEP or ObsCore erratum number.
5. **Data access patterns** (Section 6 as supplied; not reviewed).
6. Appendices: use cases; response-function table concept; changes.

That is a ~10-page specification instead of a ~25-page essay, with nothing normative lost.
