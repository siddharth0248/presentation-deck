# Conference Slide-Deck Agent Prompt

## Choosing Between COG, Zarr, and Virtual Zarr: Lessons from NASA on Cloud-Native Format Decisions

## 1. Role and deliverable

You are a scientific presentation designer and cloud-native geospatial engineer. Research, write, and build a technically accurate conference presentation from this brief and the supplied materials. Produce the completed deck, not merely an outline.

Create exactly one final deliverable named `nasa_cloud_native_format_decisions.html`, containing exactly **13 primary slides**. Use reveal.js loaded from cdnjs. Include all slide markup, custom JavaScript, speaker notes, diagrams, editable benchmark data, source records, and authored CSS in that file.

This is a single-file deliverable with permitted online dependencies, not a fully offline bundle. Allow only reveal.js core JavaScript, its required reset/layout CSS and one base theme, the matching notes plugin, and Google Fonts. Pin reveal.js assets to one verified cdnjs version. Do not mix versions.

Put all authored CSS in exactly one `<style>` block. External reveal.js styles and Google Fonts are exceptions. Do not author inline `style` attributes or separate custom stylesheets. Do not use React, Tailwind, Bootstrap, localStorage, analytics, Mermaid as a runtime dependency, or a syntax-highlighting library. Small manual code-token classes are acceptable.

Build diagrams with inline SVG or HTML/CSS. Embed necessary, authorized raster images. Slides must not depend on live NASA/NOAA data, tile services, remote iframes, or external notebooks. Optional demonstration links must not be necessary to understand a slide.

Use the previous `virtual_zarr_conference_deck_prompt.md` for design and implementation conventions only. This brief replaces its title, narrative, mandatory NLDAS-3 example, slide order, slide count, and benchmark-placeholder restrictions. Do not create another Virtual Zarr implementation tutorial.

## 2. Presentation context and narrative

**Exact title**

Choosing Between COG, Zarr, and Virtual Zarr: Lessons from NASA on Cloud-Native Format Decisions

**Track**

Cloud-Native Geo in Practice

**Duration**

Retain the previous presentation's 20-minute slot: 19 minutes of planned content and 1 minute of buffer.

**Audience**

Geospatial engineers, scientific data providers, science teams, DAAC staff, platform developers, and technically experienced researchers. Assume basic familiarity with COG and Zarr. Explain Virtual Zarr briefly, without making it the default answer.

**Central question**

Which representation serves this workload, at an acceptable lifecycle cost, with an owner who can sustain it?

**Narrative**

User workload -> access pattern -> source layout and constraints -> candidate representations -> practical advisor -> three case studies -> benchmark and lifecycle costs -> anti-patterns -> operational ownership -> reusable decision framework.

Treat COG, physically materialized Zarr, and virtual Zarr representations as architectural options, not a universal ranking. Explain when each fits, what it costs, and what would change the decision. Allow a hybrid architecture or retaining an adequate existing representation.

The title's NASA framing describes lessons from NASA-related work. It must not imply that NOAA data are NASA-produced, external examples are DSE implementations, or proposed guidance is NASA-wide policy.

**Editorial thesis**

Choose the access pattern and operating model first. Then choose the physical layout, access representation, and serving stack that support them.

## 3. Inputs, attribution, and evidence

Read these inputs before composing slides:

- This brief and the presenter's 13-slide outline.
- `decision-tree-summary.qmd`, the supplied advisor specification.
- `virtual_zarr_conference_deck_prompt.md`, for retained implementation and design requirements.
- Any subsequently supplied DSE examples, benchmark measurements, screenshots, notebooks, or deployment records.

Preserve the presenter's supplied context:

- **COG case:** GHG Center MiCASA carbon-flux monthly files, with a presenter-specified 2001-2025 coverage window.
- **Virtual Zarr case:** the ODSI DSE team is developing the Air4US portal and reports having developed a virtual Zarr store for NOAA NAQFC data, with similar work for TEMPO L3.
- **Zarr case:** no specific DSE dataset was supplied. Use the public MUR SST Zarr example provisionally, clearly attributed as an external NASA-data example and replaceable later.
- **Benchmark:** the presenter explicitly requests editable numeric placeholders, not measured findings.
- **Anti-patterns:** DSE-specific incidents will be supplied later. Do not invent them.

Do not expand ODSI or DSE unless the presenter or an authoritative source establishes the intended expansion.

Distinguish these evidence statuses in notes and the source manifest:

| Status | Appropriate use |
|---|---|
| Presenter-reported | Project context or implementation statements supplied by the presenter, without independent deployment verification. |
| Observed in supplied work | A finding supported by supplied code outputs, logs, figures, or measurements. |
| Documented capability | Behavior established by relevant documentation or inspected implementation. |
| Externally reported | An identified outside project's example or finding. |
| Conceptual or proposed | An architectural illustration, recommendation, or untested integration. |
| Illustrative placeholder | Deliberately synthetic values or editable content slots. |
| Not verified | A material detail that could not be established. |

Record a supporting source, specific section or file, access date, software version or commit where relevant, evidence status, and associated slide numbers. Project attribution and engineering rationale may have different evidence statuses.

Read substantive repository files, dependency specifications, and relevant tests before adapting implementation details. A template repository, README, open issue, or unmerged change is not proof of a specific deployment.

Preserve source terminology and disclose gaps or conflicts. Separate source-derived content from researched additions and proposed interpretation. Do not execute unreviewed scripts, use private credentials, launch cloud infrastructure, or run expensive jobs to complete the presentation.

## 4. How to use the attached decision-tree summary

The attachment describes a **scored workload advisor**, not a strict sequence of mutually exclusive yes/no choices. Preserve that distinction.

Use its actual process as the basis for slide 05:

`Workload profile -> initialize candidates -> apply matching score adjustments -> record reasons and cautions -> sort -> apply compatibility filter -> shortlist -> layout guidance and validation.`

Preserve the following semantics:

1. Inputs include data structure, primary access pattern, archive context, latency priority, and storage granularity, plus relative 1-5 controls for request locality, update frequency, cost sensitivity, and parallelism.
2. Every candidate begins at zero. Matching rules add points. Cautions do not subtract points. All scoring stages remain applicable to each scenario.
3. The original candidates are COG, Zarr, Virtual Zarr, NetCDF, GeoParquet, PMTiles, and COPC. The talk focuses on raster and multidimensional workflows; acknowledge the broader advisor scope in notes.
4. Apply the attachment's compatibility gate after scoring. Its raster set includes COG, Zarr, Virtual Zarr, and NetCDF; its multidimensional set includes Zarr, Virtual Zarr, and NetCDF. Explain that this is the advisor's eligibility model, not a claim that TIFF can never encode multiple bands or time slices.
5. The recommendation set contains up to three compatible formats within six points of the leader. Keep this distinct from the separately returned list of the next three alternatives.
6. Use the exact supplied score changes and thresholds whenever displaying a computed score or implementing an interactive advisor. Do not invent tie-breaking behavior or confidence percentages.
7. Present scores as the supplied heuristic's preferences, not calibrated performance estimates, cost predictions, or NASA standards.

Label the visible diagram **"Simplified view of the supplied workload advisor."** Keep detailed rule tables, thresholds, exceptions, and source references in speaker notes. Do not crowd all seven formats or every scoring rule onto the slide.

Describe map-serving, multidimensional analysis, and archive-preservation tendencies as explanations, not replacement algorithms. Do not imply that "existing NetCDF" always selects Virtual Zarr or "time series" always selects materialized Zarr.

Keep NetCDF or the current access path visible as a valid baseline. Add an explicitly proposed validation loop and hybrid option without presenting those additions as original scoring rules. Distinguish the attachment's implementation fallback after an empty compatibility result from production advice; recommend stopping for investigation rather than endorsing an incompatible candidate.

The attachment's validation questions are a starting point. In notes, qualify that one metadata read and byte-range access are not universal requirements for every backend. Evaluate bounded initialization, selective retrieval, correctness, and acceptable request overhead for the actual stack. A virtual-versus-rewritten test should establish the trade-off, not require either one to win.

## 5. Slide structure and timing

Create exactly these 13 primary slides in this order. Preserve the requested titles. Put supplementary material in notes rather than adding untimed slides.

| Slide | Title | Time |
|---|---|---:|
| 01 | Title / The Question | 0:30 |
| 02 | The Format Is Not the Decision | 1:00 |
| 03 | Three Formats, Three Physical Models | 1:45 |
| 04 | Start With the Access Pattern | 1:30 |
| 05 | Decision Tree | 2:00 |
| 06 | Case Study 1: When COG Makes Sense | 1:45 |
| 07 | Case Study 2: When Zarr Makes Sense | 1:45 |
| 08 | Case Study 3: When Virtual Zarr Makes Sense | 2:15 |
| 09 | Benchmark: What Do You Pay For? | 2:00 |
| 10 | Common Anti-Patterns | 1:15 |
| 11 | Operational Decision: Who Decides? | 1:15 |
| 12 | NASA - DSE Mental Model (recap) | 1:15 |
| 13 | Takeaways | 0:45 |

**Total planned speaking time: 19:00. Buffer: 1:00.**

### 01. Title / The Question

Use the exact presentation title. Include presenter, affiliation, and event details only when supplied; otherwise omit them or use explicit placeholders.

Introduce the central question with three small representation motifs. Do not open with a format tutorial, performance claim, or predetermined winner.

### 02. The Format Is Not the Decision

Shift the discussion from file extensions to requirements.

Show two contrasted approaches:

- Format-first: select a format -> transform everything -> discover the workload mismatch.
- Workload-first: identify consumers and queries -> inspect source layout -> test candidates -> publish an appropriate representation.

Frame the second as recommended practice, not a claim about a documented NASA incident.

Connect access pattern, latency, throughput, costs, downstream compatibility, updates, and ownership. Distinguish the preservation/source product from analysis and visualization representations.

**Takeaway:** A format choice is the output of a requirements decision.

### 03. Three Formats, Three Physical Models

Preserve the requested title, but add a precision note: **two physical layouts and a virtual reference layer.** Virtual Zarr is not a separate physical array encoding equivalent to GeoTIFF or the Zarr specification.

Use three comparable diagrams:

- **COG:** an internally tiled GeoTIFF, with overview levels where appropriate; a compatible reader retrieves relevant portions of an object. Show a selected spatial window or zoom level. Reference GDAL COG documentation [R10].
- **Materialized Zarr:** multidimensional arrays whose stored chunk layout and compression can be designed for target queries. Show time, y, and x axes. Do not claim every chunk must be a separate object; account for sharding and storage implementations [R11].
- **Virtual Zarr:** array metadata and references identify existing encoded source data. A compatible store/reader resolves references, reads source bytes, and decodes them. Show both references and original files [R12-R13].

Under each, show what is physically stored, how selection reaches bytes, and its main layout constraint. Distinguish Virtual Zarr, VirtualiZarr, Kerchunk representations, Icechunk, and Zarr versions.

**Takeaway:** The representation changes where layout decisions and operational costs occur.

### 04. Start With the Access Pattern

Use a small query-to-layout diagram, not an exhaustive matrix. Contrast a map window, a point or regional time series, and a multidimensional batch calculation.

Include the dimensions that actually drive the decision:

- Spatial selectivity, temporal depth, variables, levels, ensembles, and requested output size.
- Interactive latency versus batch throughput, concurrency, and request locality.
- Existing archive versus new pipeline; source chunks, compression, reader compatibility, and source stability.
- Update frequency, storage granularity, cost sensitivity, and lifecycle ownership.

Explain that these are workload descriptions, not automatic format winners. The same dataset can support multiple legitimate access patterns.

**Takeaway:** Describe the query before choosing its storage layout.

### 05. Decision Tree

Create a readable, simplified visualization of the attached scored advisor using Section 4. Show inputs, scoring and cautions, compatibility filtering, shortlist, and empirical validation as distinct steps.

Emphasize three practical questions without replacing the original algorithm:

- Is the dominant workload spatial/map-oriented or multidimensional/temporal?
- Does an existing archive need to be preserved, and is its layout usable through the required readers?
- Can a candidate meet correctness, latency, cost, update, and ownership requirements?

Include a small off-ramp: **retain the existing path or combine representations when appropriate.** Mark the hybrid/validation additions as proposed guidance.

Return to this diagram through a small criteria indicator on slides 06-08. Do not fabricate numeric workload profiles or scores for the case studies. Any illustrative profile must be labeled as such and computed using the supplied rules.

**Takeaway:** Use the advisor to produce candidates, then validate the workload.

### 06. Case Study 1: When COG Makes Sense

**Example:** GHG Center MiCASA carbon-flux monthly files, presenter-specified 2001-2025 window.

Start with [R01-R02]. Verify the selected collection, variable, native source versus published representation, monthly aggregation, units, sign convention, grid, and actual catalog coverage. Preserve the presenter's intended date window; distinguish it from any narrower publicly verified asset coverage. Do not assume that every month or variable is present, or infer a file count from calendar arithmetic.

Use a concrete map-oriented question such as examining the spatial pattern of a selected carbon-flux variable for one month and moving through other months.

Show:

`Select month and variable -> resolve the relevant published asset -> read appropriate raster tiles/overviews -> render the map.`

Explain the workload fit through spatial selection and zoom-dependent access. Verify COG use through an actual asset, notebook, or configuration; a `.tif` extension alone is insufficient evidence of a valid COG.

State the trade-off: a long time series across many monthly assets may require many reads; a map-optimized representation is not automatically the best layout for every temporal analysis.

Keep this bounded to map access. Do not describe MiCASA generically as fossil-fuel emissions or present a coarse overview as a scientifically equivalent analysis product without checking the operation.

Use an authorized real map only when its variable, month, legend, and source are known. Otherwise use a labeled workflow schematic.

**Decision statement:** COG is a candidate for this monthly spatial-browsing workload, not necessarily every MiCASA use.

### 07. Case Study 2: When Zarr Makes Sense

**Provisional example:** NASA JPL MUR Level 4 sea-surface temperature, using the publicly documented physical Zarr representation [R03-R04].

Label it **"External NASA-data example; replaceable with a DSE case."** Do not attribute its creation or operations to the presenter or DSE.

Use a multidimensional analysis question such as calculating a regional multi-day SST mean or comparing regional time series. Explain the relationship between temporal chunks, spatial chunks, over-read, parallel computation, and memory.

Show:

`Known source product -> validated materialization with a chosen chunk layout -> repeated xarray/Dask analysis.`

Distinguish a documented existing store from any proposed rechunked layout. The AWS registry lists a particular historical Zarr-store extent ending on 2020-01-20 and a specific chunk shape. Do not equate this with the current coverage of the underlying MUR product, infer a Zarr specification version from the path `zarr-v1`, or claim that this stored chunk layout is optimal for point time series.

Explain when physical reorganization may be justified: repeated important queries need a layout the source cannot efficiently provide, and expected gains justify conversion, validation, added storage, and maintenance. Present that as engineering rationale, not an already measured result for this example.

A short xarray snippet is optional, not required. Verify the exact access path and dependencies before presenting runnable code. An unexecuted analysis remains labeled as unexecuted.

**Decision statement:** Materialized Zarr is a candidate when deliberate multidimensional layout control serves repeated analysis.

### 08. Case Study 3: When Virtual Zarr Makes Sense

**Presenter-reported context:** the ODSI DSE team is developing Air4US and has developed virtual-store access for NOAA NAQFC, with similar work for TEMPO L3.

Make NAQFC the primary example and TEMPO L3 a compact parallel example. Do not imply these are one homogeneous dataset, share a parser, or have identical access controls.

Use [R05-R09] to distinguish:

- **NAQFC:** NOAA forecast products. Check the actual objects and parser. NOAA's AQM inventory includes GRIB2 products; do not relabel the source archive NetCDF/HDF5 for narrative convenience. Preserve forecast initialization, lead time, valid time, model version, and product semantics.
- **TEMPO L3:** verify the precise product, processing version, science variable, groups, quality flags, and authentication. The cited NO2 L3 documentation describes NetCDF4; do not assume that is the presenter's selected version or that L3 requires no scientific quality handling.

Show two source branches joining a common conceptual access pattern:

`Existing compatible files -> dataset-specific parsing and validation -> versioned references/store -> compatible analytical or application reader.`

Separately trace the read path from the reader back to the original encoded data. Do not imply the reference metadata contains the full scientific payload.

Explain what virtualization preserves and what it does not repair. For the documented GRIB parser, message boundaries determine chunk boundaries; referencing a message does not create arbitrary sub-message spatial chunks [R09]. Confirm the actual deployed parser before attributing that behavior to Air4US.

Treat `developmentseed/virtualizarr-data-pipelines` as a reusable infrastructure source. Its template alone does not establish the Air4US processor, deployment configuration, production status, or measurements. Keep unknown details editable: `[NAQFC_PROCESSOR]`, `[TEMPO_PRODUCT_VERSION]`, `[STORE_IMPLEMENTATION]`, `[DEPLOYMENT_STATUS]`, and `[VERIFIED_OUTPUT]`.

**Decision statement:** Virtual access is a candidate when retaining source data is valuable and inherited layout, compatibility, and operations are acceptable.

### 09. Benchmark: What Do You Pay For?

Use the editable numeric placeholders in Section 6. Display **"ILLUSTRATIVE PLACEHOLDERS. NOT MEASURED RESULTS."** prominently and retain that qualification in screenshots and notes.

Compare query latency, request count, preprocessing, added storage, and query compute. Distinguish map and temporal workloads. Include transfer and maintenance in notes or a compact lifecycle band.

Do not compare MiCASA/COG, MUR/Zarr, and NAQFC/Virtual Zarr as if their timings were a controlled format benchmark. Real comparisons must use equivalent content and outputs across representations of the same selected dataset.

Separate one-time preparation, recurring storage/maintenance, and per-query expense. Explain how COG represents the selected multidimensional benchmark data when applicable.

Do not infer winners, speedups, dollar savings, or break-even points from placeholders.

**Takeaway:** Compare the complete workload and lifecycle, not only open time or storage size.

### 10. Common Anti-Patterns

Show four concise mistake-to-correction pairs:

- **Convert the whole archive first.** Test representative workloads before committing to migration.
- **Use one layout for every query.** Distinguish map, temporal, and multidimensional access; consider justified derivatives.
- **Expect virtualization to repair physical layout.** Inspect inherited chunks/messages, reader compatibility, and source stability.
- **Benchmark only lazy initialization.** Execute equivalent data retrieval/computation and report requests, bytes, and environment.

Put additional engineering risks in notes: tiny chunks and request overhead; oversized chunks and over-read; unsuitable or missing COG overviews; external compression wrappers that defeat the intended access path; unexamined sharding; cross-region access; stale or mutated reference targets; duplicate update ingestion; schema drift; lost scientific metadata; and ownerless derivatives.

Distinguish risks from observed failures. Do not present heuristic cautions, such as single-object contention, as universal COG performance limits. Verify implementation-specific claims against [R08-R15].

Reserve `[DSE_ANTI_PATTERN_EXAMPLE_1]` and `[DSE_ANTI_PATTERN_EXAMPLE_2]` in notes and editable content. For each later example request the workload, original choice, observed issue, evidence, correction, and bounded lesson. Until supplied, use general guidance rather than fictional anecdotes.

### 11. Operational Decision: Who Decides?

Present a **proposed shared-responsibility model**, not an asserted NASA-wide mandate.

Use a compact responsibility diagram:

- Science teams define scientific meaning, valid transformations, quality requirements, and representative questions.
- DAAC/data stewards define preservation, authoritative versions, provenance, access, and distribution constraints.
- DSE/platform engineers evaluate layouts and readers, benchmark workloads, and propose maintainable infrastructure and serving patterns.
- Self-service users provide workload feedback and can create bounded derivatives using documented guardrails, budgets, provenance, and reuse practices.

Explain that final accountability depends on the actual program. Leave `[ACCOUNTABLE_DECISION_OWNER]` editable instead of inventing institutional authority.

Include a concise decision record: workload, source identity, selected representation, alternatives, correctness/performance acceptance criteria, owner, update strategy, and revisit trigger.

**Takeaway:** The decision needs scientific agreement, technical evidence, and an accountable lifecycle owner.

### 12. NASA - DSE Mental Model (recap)

Label this a proposed synthesis for the talk, not a formally adopted policy. Use five memorable questions:

1. What will users read or compute most often?
2. What layout and constraints do the existing data impose?
3. Where will we pay for preparation, storage, requests, transfer, and compute?
4. Who owns validation, updates, source/reference stability, and support?
5. What representative test would justify this choice or trigger a different one?

Reuse small motifs from the decision diagram and all three case studies. Make room for a hybrid answer: preserve or virtualize an archive, materialize high-value analytical subsets, and publish map-oriented derivatives where justified.

### 13. Takeaways

Close with four lessons, not a slide containing only "Questions":

- Choose from workloads and constraints, not file extensions.
- COG, materialized Zarr, and virtual access place costs and layout control in different parts of the system.
- Benchmark equivalent work before scaling a decision.
- Plan scientific validation, ownership, updates, and justified hybrid representations.

End with a practical action: **write down one representative query, test candidate layouts, and record the trade-off.**

## 6. Editable benchmark placeholders and measurement integrity

Numeric placeholders are explicitly authorized for this draft. Use deliberately neutral values, identical across formats within each row, so they do not suggest a result.

| Metric | COG | Materialized Zarr | Virtual Zarr |
|---|---:|---:|---:|
| W1 map-query end-to-end latency, ms | 1000* | 1000* | 1000* |
| W2 time-series end-to-end latency, s | 1.0* | 1.0* | 1.0* |
| W2 requests per query, count | 100* | 100* | 100* |
| One-time preparation compute, vCPU-hours | 1.0* | 1.0* | 1.0* |
| Additional retained representation, GB | 1.0* | 1.0* | 1.0* |
| W2 query compute, vCPU-seconds | 1.0* | 1.0* | 1.0* |

`* Arbitrary editable placeholders. Not measured values, predictions, or evidence of equivalence.`

Store underlying values in one clearly marked embedded `benchmark-data` JSON object. Use numeric values without the display asterisk, explicit units, metric IDs, workload IDs, and `status: "placeholder"`. Generate the table from this object so the presenter edits values in one place.

Provide editable metadata for dataset/version, variables, subset, source format/layout, codecs, compute environment, client/cloud regions, concurrency, cache condition, repetitions, statistic, and measurement date. Leave unknown metadata `null` or visibly pending. Mark each metric independently when real evidence becomes available; one measured row must not relabel all rows as measured.

Keep the placeholder banner while any displayed value is synthetic. Do not generate performance-ranking colors, invented error bars, speedup annotations, or conclusions from placeholder values. Default to a table rather than synthetic bar charts.

For subsequent real measurements:

- Define W1 and W2 precisely and use equal content, selections, result sizes, scientific operations, and output fidelity across candidate representations.
- Include the original access path as a documented baseline, even when its timings remain in notes.
- Separate initialization, lazy query construction, executed retrieval/computation, and end-to-end latency. Measure requests and bytes rather than inferring them from logical array size.
- Keep cold/warm-cache results and client/concurrency conditions distinct. Report repetitions and the actual statistic; a single observation is not a median.
- Include both retained source storage and additional representation storage. References may avoid a full second payload but do not eliminate source storage or reads.
- Distinguish build costs from incremental updates, validation, operations, and per-query costs. Label monetary values modeled or measured and identify rate date, region, assumptions, and exclusions.

Do not import historical measurements from unrelated datasets or unequal date windows to fill this table.

## 7. Research resource library

These are entry points, not independent proof of deployment. Verify exact files, versions, collection metadata, and current availability when building the deck. Follow primary sources and attribute their actual scope.

| ID | Resource | Use |
|---|---|---|
| R01 | `https://earth.gov/ghgcenter/data-catalog/micasa-carbonflux-grid-v1` | MiCASA collection identity and published case context. |
| R02 | `https://github.com/US-GHG-Center/veda-config-ghg` and `https://github.com/US-GHG-Center/ghgc-docs` | Inspect actual configuration, notebooks, asset links, and visualization paths. |
| R03 | `https://registry.opendata.aws/mur/` | Public MUR Zarr representation, its attribution, listed extent, and chunk layout. |
| R04 | `https://podaac.jpl.nasa.gov/dataset/MUR-JPL-L4-GLOB-v4.1` | Authoritative scientific product identity; distinguish it from a particular derivative store. |
| R05 | `https://registry.opendata.aws/noaa-nws-naqfc-pds/` | NAQFC source archive, product context, and access information. |
| R06 | `https://www.nco.ncep.noaa.gov/pmb/products/aqm/` | AQM product inventory and GRIB2 examples; verify the selected objects independently. |
| R07 | `https://doi.org/10.5067/IS-40e/TEMPO/NO2_L3.004` | Example TEMPO NO2 L3 documentation, not an assumption about the deployed Air4US product/version. |
| R08 | `https://github.com/developmentseed/virtualizarr-data-pipelines` | Infrastructure patterns; inspect relevant processors, tests, and dependencies before attributing details. |
| R09 | `https://virtualizarr.readthedocs.io/en/stable/api/parsers/grib.html` | Version-specific GRIB parser, codec, and message/chunk behavior. |
| R10 | `https://gdal.org/en/stable/drivers/raster/cog.html` | COG construction, blocks, overviews, and layout options. |
| R11 | `https://zarr-specs.readthedocs.io/en/latest/v3/core/index.html` | Zarr data model and physical storage distinctions; use the matching version for examples. |
| R12 | `https://virtualizarr.readthedocs.io/en/stable/` | Virtual-reference construction, parser compatibility, validation, and client access. |
| R13 | `https://icechunk.io/en/stable/guides/virtual/` | Virtual chunks, repository access, and source-integrity considerations. |
| R14 | `https://docs.xarray.dev/en/stable/user-guide/io.html` | Supported opening/writing paths, laziness, and backend requirements. |
| R15 | `https://docs.dask.org/en/stable/best-practices.html` | Chunk sizing, graph overhead, parallelism, and memory trade-offs. |
| R16 | `https://github.com/NASA-IMPACT/virtual-stores-feasibility-report` | NASA-related feasibility and stewardship discussion; attribute report status and recommendations. |
| R17 | `https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html` and `https://aws.amazon.com/s3/pricing/` | Range access and region/date-specific pricing only when actually used. |
| R18 | `https://revealjs.com/installation/`, `https://revealjs.com/config/`, `https://revealjs.com/speaker-view/`, and `https://cdnjs.com/libraries/reveal.js` | Verify dependency paths, version consistency, notes, and local-serving behavior. |

Keep canonical-URL proposals, unrelated NLDAS walkthroughs, and deep infrastructure tutorials out of this talk unless directly needed to explain a decision.

## 8. Technical and scientific guardrails

Describe reduced full-payload duplication, not "zero storage," "zero I/O," or "no data movement." Reference generation can read metadata or coordinates, and consumption still reads source data. Separate logical combination from physical rechunking, recompression, reprojection, or scientific normalization.

Do not promise universal format, codec, authentication, client, or visualization compatibility. Distinguish GRIB, NetCDF variants, HDF5, and selected TEMPO products. A Zarr-capable reader does not automatically support every virtual-store backend.

Separate data preparation, catalog discovery, storage/access, computation, and rendering. Check the scientific implications of aggregation, resampling, overviews, units, calendars, quality masks, and coordinate handling. For air quality, distinguish modeled surface concentrations from satellite column quantities and forecasts from observations.

Use real scientific outputs only when supplied or actually computed. Otherwise use a visibly labeled conceptual diagram or output placeholder. Do not generate plausible-looking scientific maps or plots and present them as observations.

For code, label it **tested here**, **adapted but not executed here**, or **pseudocode**. Match API versions and never include credentials or signed URLs.

## 9. Visual design, notes, and source records

Retain the previous deck's visual system:

- 16:9 canvas, preferably 1600 x 900, with consistent safe margins and left-aligned content.
- Base navy `#1F3A5F`, primary blue `#2F6FDE`, and white `#FFFFFF`; define the full working palette in `:root`.
- Lighter base and blue panel tints, dark navy around `#0F1D30`, cool-neutral dividers, blue-grey secondary text, and lifted blue around `#8DB8FF` on dark backgrounds.
- One serif heading font, one sans-serif body font, and one monospace code font from Google Fonts, with system fallbacks. Aim for approximately 30px body text.
- Retain `.kicker`, `.cols`, `.col`, `.card`, `.rowcards`, `.chip`, `.band`, `.ask`, `.num`, and restrained table styling. Include accessible evidence and placeholder badges.

Use dark treatment for the opening, one conceptual pivot, and closing recommendations. Maintain WCAG AA text contrast, including `strong` and `em` overrides on dark backgrounds. Do not rely on color alone to communicate representation type, evidence status, or proposed steps. Do not use organization logos.

Give slides 06-08 a consistent structure: **workload -> representation -> trade-off -> what would change the choice.** Use solid/dashed connectors with a legend where evidence status differs. Prefer diagrams over paragraphs and one principal message per slide. Do not hide overflow or shrink text into illegibility.

Use precise, non-promotional language. Avoid marketing claims, generic superlatives, em dashes in visible slide text, and colons used to splice sentences. Exact supplied titles, code, URLs, and technical syntax are exceptions. Keep short labels consistent with the previous deck's punctuation conventions.

Every slide must contain `<aside class="notes">...</aside>` with duration, spoken explanation, transition, caveats, evidence status, sources, and unresolved placeholders. Write notes for the allocated speaking time rather than simply repeating visible text.

Include full citations in notes and short source labels near substantive claims or figures. Use `[Sources]` sections in notes for external claims and assets. Cite exact files or sections for code and advisor rules.

Embed a valid `source-manifest` JSON block with IDs, titles, URLs or supplied filenames, access dates, versions/commits, evidence statuses, claim locations, and slide numbers. Also include an embedded, clearly labeled list of unresolved presenter edits. Neither requires a separate output file or extra slide.

## 10. Build, inspect, and deliver

Complete research, composition, implementation, and inspection in the current task. Do not block on missing benchmark values, presenter details, DSE incidents, or inaccessible services when an honest placeholder is sufficient.

Before delivery verify:

1. One HTML deliverable contains exactly 13 primary slides, in the specified order, with notes on every slide and timing totaling 19 minutes.
2. The title and balanced decision-oriented narrative match this brief; the old NLDAS-3-only structure is absent.
3. Slide 05 faithfully represents the supplied scored advisor, with simplified and proposed elements labeled.
4. MiCASA, MUR, NAQFC, and TEMPO retain correct identity and attribution; missing case details remain explicit.
5. Slide 09 uses a single editable data source and conspicuous placeholder labels. No conclusion is based on invented measurements.
6. DSE incidents and organizational authority are not fabricated; proposed guidance is labeled.
7. Authored CSS and external assets obey Section 1; dependencies use matching versions; no live data is needed to render a slide.
8. Both embedded JSON blocks parse correctly; all markup and optional code are escaped safely.
9. Diagrams, tables, bands, titles, and source labels fit without clipping, overlap, unreadable text, or horizontal code scrolling.
10. Source records and unresolved-edit markers are complete, and keyboard navigation and speaker-note access are checked where supported.

When browser tools are available, serve the file locally, wait for dependencies and fonts, inspect console/network errors, and capture every slide. Fix layout defects and repeat. Check system-font fallback. Do not claim reveal.js or speaker-view testing when only a fallback layout was inspected.

Return the completed HTML file with brief local-serving instructions, the correct local URL, navigation and notes instructions, and a summary of checks actually performed. Identify remaining presenter edits and verification gaps without claiming they were resolved.
