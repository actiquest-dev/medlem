1. # **Medlem: Ontology & Data Foundation Overview**

## **Purpose**

The goal of this work was to build a **practical, scalable ontology core** for a future product — not a theoretical medical taxonomy.

We focused on:

* real-world data,  
* proven standards,  
* and a bottom-up approach grounded in actual usage and evidence.

The result is a lightweight but extensible semantic foundation that can grow into a full reasoning and knowledge layer.

## **Step 1\. Building the Initial Data Backbone**

*(PubMed \+ Drug Normalization)*

We started by constructing a reliable base that connects **medical language in the wild** with **stable identifiers**.

### **PubMed as the evidence layer**

* PubMed articles were used as a primary source of real-world medical text  
* Articles provide:  
  * context  
  * terminology in use  
  * scientific grounding

PubMed was not treated as a taxonomy, but as a **semantic signal source**.

## **Step 2\. Normalization via UMLS 2025AB**

*(Minimal, targeted import)*

We did **not** ingest the full UMLS database.

Instead, we selectively used parts of **UMLS 2025AB** to solve one concrete problem:

**Normalize entities across large volumes of heterogeneous medical text.**

## **Use of UMLS 2025AB (Scope and Constraints)**

### **What was used**

#### **RxNorm (UMLS 2025AB)**

* Used as the source of truth for drugs and substances  
* Version fixed: **UMLS 2025AB**  
* Role:  
  * unifying brand names, generics, and spelling variants  
  * ensuring stable identifiers across sources

#### **RXNCONSO**

Used for:

* **RXCUI** as the primary stable identifier  
* Canonical drug names  
* Synonyms and variants (brand, generic, alternative spellings)

Purpose:

* Normalize mentions from PubMed and external sources  
* Resolve multiple surface forms into a single entity

#### **RXNREL (limited use)**

* Used selectively  
* Only high-confidence, product-relevant relations:  
  * ingredient ↔ clinical drug  
  * brand ↔ ingredient

We did **not** build a full RxNorm graph.

Only the minimal relationship layer required for reliable entity resolution.

#### **SNOMED CT (via UMLS 2025AB)**

* Used as a **clinical concept normalization layer**  
* Scope was intentionally constrained

How it was used:

* Core SNOMED concepts for diseases, conditions, and clinical terms  
* Anchoring medical meaning in text  
* Enabling alignment between:  
  * substances  
  * conditions  
  * supporting evidence

SNOMED was treated as a **reference layer**, not as a fully expanded hierarchy.

#### **PubMed linkage (outside UMLS)**

* PubMed remained an external semantic layer  
* Mapping flow:  
  * text mention → normalized name → RXCUI or SNOMED concept

No MeSH hierarchy or semantic type system was modeled.

### **What was intentionally not used**

To keep the system lightweight and product-focused, we explicitly excluded:

* MRCONSO beyond the RxNorm subset  
* MRSTY semantic types  
* Full MRREL graph  
* Full SNOMED hierarchy traversal  
* MeSH as an ontological layer  
* LOINC or ICD as primary sources

This was a deliberate design decision, not a limitation.

## **Step 3\. Ontology Construction Approach**

*(Bottom-up, not academic)*

The ontology was built **bottom-up**, based on actual data flows:

* No attempt to recreate a “complete medical ontology”  
* No heavy upfront schema  
* No static taxonomy-first design

Instead:

* **RxNorm** provides normalized substance entities  
* **SNOMED** provides normalized clinical meaning  
* **PubMed** provides context and evidence

Together, they form a graph of:

real-world mentions → normalized entities → grounded meaning

## **Result: Core Ontology of the Future Product**

What we created is the **core ontology layer** of the future product:

* Based on established, trusted standards  
* Focused on normalization and interoperability  
* Lightweight enough to scale and evolve  
* Ready to support:  
  * enrichment  
  * reasoning  
  * cross-source linking  
  * future AI and decision systems

This foundation leverages existing solutions where they are strongest, while avoiding unnecessary complexity — creating a solid base for a product that can grow over time.

2. # **Medlem: Conference Data Collection Strategy Report**

## **1\. WORK COMPLETED**

### **Phase 1: Initial Data Collection**

* **Source:** ConferenceIndex (conferenceindex.org)  
* **Scope:** 4,656 URLs extracted from HTML files  
* **Output:** `waset_urls.txt` with conference links  
* **Infrastructure:** Python botasaurus library with headless Chrome automation

### **Phase 2: Data Parsing**

* **Achieved:** Successfully parsed 4,656 conference pages  
* **Data extracted:** Committee members (name \+ affiliation) and selected papers  
* **Result:** `waset_conferences_data.json` with structured data  
* **Technical improvements:** Implemented smart timeouts, HTML validation, batch processing with state recovery

## **2\. CRITICAL ERRORS DISCOVERED**

### **Error 1: WASET is Predatory Publisher**

* **Finding:** WASET (World Academy of Science, Engineering and Technology) is a known predatory conference organizer  
* **Evidence:**  
  * Listed by Jeffrey Beall as "potential predatory publisher"  
  * Investigated by journalists (Süddeutsche Zeitung, ARD, Nature) in 2018  
  * Generated $4M+ annual revenue from fake conferences  
  * Committee members identical across all locations/dates (29 members, 15 papers everywhere)  
* **Impact:** All 4,656 collected records are invalid data

### **Error 2: ConferenceIndex is WASET Aggregator**

* **Finding:** ConferenceIndex primarily indexes WASET conferences, not legitimate conferences  
* **Evidence:**  
  * Of 2,941 non-WASET URLs extracted: 1,054,388 links point back to conferenceindex.org  
  * Only 64 truly external conference URLs found  
  * Real conferences mixed with template-generated fakes  
* **Impact:** Data collection strategy fundamentally flawed from source selection

## **3\. VALIDATED LEGITIMATE SOURCES**

### **Tier 1: Specialized Aggregators (Recommended)**

1. **eMedEvents** (emedevents.com)  
   * 15M medical professionals served  
   * Harvard Business School founders  
   * CME/CE credit tracking  
   * **Challenge:** Dynamically loaded content (React/Next.js)  
2. **MDLinx** (mdlinx.com/conferences)  
   * Medical-focused event directory  
   * Specialty filtering available  
   * Static HTML structure (easier parsing)  
3. **The Conference Website** (theconferencewebsite.com)  
   * 1,500+ legitimate medical conferences  
   * Organized by specialty  
   * Free access

### **Tier 2: Official Organizational Sites**

* American Heart Association (aha.org)  
* American College of Cardiology (acc.org)  
* Healthcare IT Society (himss.org)  
* IEEE Medical conferences

## **4\. NEXT STEPS**

### **Immediate Actions**

1. **Pivot to MDLinx** \- Start with easiest source (static HTML structure)  
2. **Reverse-engineer eMedEvents API** \- Capture Network requests to find GraphQL/REST endpoints

### **Phase 3: Legitimate Data Collection**

Priority 1: MDLinx (static HTML)

  → Parse specialty pages

  → Extract: name, date, location, organizer

  → Estimated: 500-1000 real medical conferences

Priority 2: eMedEvents (dynamic content)

  → Intercept API calls or use Playwright with API mocking

  → Extract structured conference data with CME credits

  → Estimated: 2000+ real conferences

Priority 3: Official sites

  → ACC, AHA, IEEE sites for premium conferences

  → Manual or semi-automated collection

  → High-quality data for flagship events

### **Technical Approach**

* **For static sources (MDLinx, The Conference Website):** Continue botasaurus \+ BeautifulSoup  
* **For dynamic sources (eMedEvents):**  
  * Option A: Capture browser Network requests → find API endpoint  
  * Option B: Use Playwright with full JavaScript rendering  
  * Option C: Check if public API exists

## **5\. LESSONS LEARNED**

| Lesson | Application |
| ----- | ----- |
| "Should work" ≠ "does work" | Always test with sample data first |
| Validate source legitimacy | Check Beall's list for predatory publishers |
| Aggregators can be fake | Verify original sources, not just listings |
| Dynamic content requires different tools | Not all HTML parsers work for React apps |
| State management critical | Use recovery files for long-running scrapes |

## **6\. SUCCESS METRICS (Phase 3\)**

* Collect 1,000+ legitimate medical conferences  
*  Data validation: Cross-reference with official org websites  
*  Structure: Standardized format (name, date, location, specialty, organizer)  
*  Quality: Zero predatory conference records  
*  Coverage: Mix of major (ACC, AHA) and specialized conferences

**Status:** Previous approach invalidated | Strategy pivoting to legitimate sources | Ready for Phase 3 implementation

**Recommendation:** Start with MDLinx this week, parallel work on eMedEvents API discovery.



---

## **Database Architecture (Current MySQL Implementation)**

This is the **exact structure we have right now** in MySQL, based on the live schemas you dumped.

### **Schema 1 — `rxnorm` (Normalization Layer)**

Purpose: keep a **clean, versioned RxNorm import** that we use to normalize drug mentions and build lightweight drug relationships.

Key tables (RxNorm / UMLS-derived):

#### `RXNCONSO` (names, synonyms, canonical strings)
- `RXCUI` varchar(8) **NOT NULL** (indexed)
- `LAT` varchar(3) **NOT NULL** (default `ENG`)
- `TS` varchar(1)
- `LUI` varchar(8)
- `STT` varchar(3)
- `SUI` varchar(8)
- `ISPREF` varchar(1)
- `RXAUI` varchar(8) **NOT NULL**
- `SAUI` varchar(50)
- `SCUI` varchar(50)
- `SDUI` varchar(50)
- `SAB` varchar(20) **NOT NULL**
- `TTY` varchar(20) **NOT NULL**
- `CODE` varchar(50) **NOT NULL**
- `STR` varchar(3000) **NOT NULL** (indexed)
- `SRL` varchar(10)
- `SUPPRESS` varchar(1)
- `CVF` varchar(50)

#### `RXNREL` (relationships between RxNorm concepts)
- `RXCUI1` varchar(8) (indexed)
- `RXAUI1` varchar(8)
- `STYPE1` varchar(50)
- `REL` varchar(4)
- `RXCUI2` varchar(8) (indexed)
- `RXAUI2` varchar(8)
- `STYPE2` varchar(50)
- `RELA` varchar(100)
- `RUI` varchar(10)
- `SRUI` varchar(50)
- `SAB` varchar(20) **NOT NULL**
- `SL` varchar(1000)
- `DIR` varchar(1)
- `RG` varchar(10)
- `SUPPRESS` varchar(1)
- `CVF` varchar(50)

#### `RXNSAT` (attributes)
- `RXCUI` varchar(8) (indexed)
- `LUI` varchar(8)
- `SUI` varchar(8)
- `RXAUI` varchar(9)
- `STYPE` varchar(50)
- `CODE` varchar(50)
- `ATUI` varchar(11)
- `SATUI` varchar(50)
- `ATN` varchar(1000) **NOT NULL**
- `SAB` varchar(20) **NOT NULL**
- `ATV` varchar(4000)
- `SUPPRESS` varchar(1)
- `CVF` varchar(50)

#### `RXNSTY` (RxNorm semantic typing)
- `RXCUI` varchar(8) **NOT NULL**
- `TUI` varchar(4)
- `STN` varchar(100)
- `STY` varchar(50)
- `ATUI` varchar(11)
- `CVF` varchar(50)

#### `RXNSAB` (source metadata)
- `VCUI` varchar(8)
- `RCUI` varchar(8)
- `VSAB` varchar(40)
- `RSAB` varchar(20) **NOT NULL**
- `SON` varchar(3000)
- `SF` varchar(20)
- `SVER` varchar(20)
- `VSTART` varchar(10)
- `VEND` varchar(10)
- `IMETA` varchar(10)
- `RMETA` varchar(10)
- `SLC` varchar(1000)
- `SCC` varchar(1000)
- `SRL` int
- `TFR` int
- `CFR` int
- `CXTY` varchar(50)
- `TTYL` varchar(300)
- `ATNL` varchar(1000)
- `LAT` varchar(3)
- `CENC` varchar(20)
- `CURVER` varchar(1)
- `SABIN` varchar(1)
- `SSN` varchar(3000)
- `SCIT` varchar(4000)

Other imported/support tables:
- `RXNATOMARCHIVE` (archive of atom strings + timestamps)
- `RXNCUICHANGES` (old ↔ new RXCUI mapping)
- `RXNCUI` (version mapping fields: `cui1`, `ver_start`, `ver_end`, `cardinality`, `cui2`)
- `RXNDOC` (documentation key/value)

Custom “product-facing” helper tables in `rxnorm`:
- `rxnorm_allowed_drug` (`RXCUI` bigint **PK**)
- `rxnorm_antidiabetic_rxcui` (`RXCUI` bigint **PK**)

UMLS subset tables stored in `rxnorm` (for convenience / join performance):
- `umls_mrconso` (CUI/LAT/SAB/TTY/CODE/STR…)
- `umls_mrrel` (CUI1/REL/CUI2/RELA…)


---

### **Schema 2 — `umls2025ab` (Product / Evidence Layer)**

Purpose: keep the **product-facing ontology + evidence tables** (PubMed, disease–drug scoring, conference extraction), while also retaining raw UMLS tables for reference where needed.

#### A) Raw UMLS tables (imported)
- `MRCONSO` (concept strings)
- `MRREL` (concept relationships)
- `MRSTY` (semantic types)
- `MRCOLS` (metadata about columns)

> Note: in practice we did *not* treat MRREL/MRSTY as a full “knowledge graph.” We used UMLS as a **grounded normalization backbone** and built our ontology bottom‑up from actual evidence sources.

#### B) Concept label utilities (fast label lookup)
These are lightweight, query-friendly projections of MRCONSO:
- `concept_label`  
  - `CUI` char(8) (indexed), `SAB` varchar(40) (indexed), `TTY` varchar(20), `CODE` varchar(100), `STR` text (indexed)
- `concept_best_label`  
  - same columns as `concept_label`, but intended as “best available label” per CUI/source.

#### C) PubMed ingestion + entity mentions
- `pubmed_article`
  - `pmid` bigint **PK**
  - `title` text
  - `journal` varchar(255)
  - `pub_year` smallint
  - `abstract_text` mediumtext
  - `mesh_terms` json
  - `created_at` timestamp (default current)
- `pubmed_entity_mention` (normalized mentions)
  - `pmid` bigint **PK**
  - `entity_type` enum('CUI','RXCUI') **PK**
  - `entity_id` varchar(32) **PK**
  - `score` float
  - `source` varchar(50) (default `pubmed`)
- `pubmed_query_run`
  - `run_id` bigint **PK** auto_increment
  - `query_text` text **NOT NULL**
  - `retmax` int **NOT NULL**
  - `note` varchar(255)
  - `created_at` timestamp (default current)
- `pubmed_query_pmid` (run → pmids)
  - `run_id` bigint **PK**
  - `pmid` bigint **PK**

#### D) Disease ↔ Drug evidence (from PubMed linkage + normalization)
- `disease_drug_evidence`
  - `disease_cui` char(8)
  - `disease_name` text
  - `disease_sab` varchar(40)
  - `drug_rxcui` bigint unsigned
  - `drug_name` text
  - `pubmed_count` bigint **NOT NULL** default 0
- `disease_drug_evidence_full` (row-per PMID)
  - `disease_cui` varchar(32) **NOT NULL**
  - `drug_rxcui` varchar(32) **NOT NULL**
  - `pmid` bigint **NOT NULL**
  - `pub_year` smallint
  - `title` text
- `disease_drug_evidence_view` (convenience view table)
  - `disease_cui` varchar(32) **NOT NULL**
  - `drug_rxcui` varchar(32) **NOT NULL**
  - `drug_rxcui_num` bigint unsigned
  - `drug_name` text
  - `pmid` bigint **NOT NULL**
  - `pub_year` smallint
  - `journal` varchar(255)
  - `title` text

Ranking tables (successive iterations of scoring):
- `disease_drug_rank_v2` (`pair_pmids`, `recency_sum`, `disease_pmids`, `drug_pmids`, `score_v2`)
- `disease_drug_rank_v3` (`pair_pmids`, `recency_sum`, `score_v3`)
- `disease_drug_rank_v4` (`pair_pmids`, `recency_sum` decimal(29,5), `score_v4`)
- `disease_drug_rank_view` (joins drug label + final score fields)

#### E) Conferences ingestion (event → talks → claims)
This is the “events” half of the ontology: it lets us attach **what was said**, by **whom**, and map it to normalized entities.

- `conference_event`
  - `event_id` bigint **PK** auto_increment
  - `name` varchar(255) **NOT NULL**
  - `year` smallint
  - `location` varchar(255)
  - `url` text
  - `created_at` timestamp (default current)

- `conference_talk`
  - `talk_id` bigint **PK** auto_increment
  - `event_id` bigint (indexed)  → references `conference_event.event_id`
  - `title` text **NOT NULL**
  - `speaker` varchar(255)
  - `video_url` text
  - `slides_url` text
  - `talk_date` date
  - `created_at` timestamp (default current)

- `conference_claim`
  - `claim_id` bigint **PK** auto_increment
  - `talk_id` bigint (indexed) → references `conference_talk.talk_id`
  - `entity_type` enum('CUI','RXCUI') (indexed)
  - `entity_id` varchar(32) (indexed)
  - `claim_text` text **NOT NULL**
  - `confidence` float
  - `created_at` timestamp (default current)

- `conference_disease_drug_evidence`
  - `disease_cui` varchar(32) **NOT NULL**
  - `drug_rxcui` varchar(32) **NOT NULL**
  - `talks` bigint **NOT NULL** default 0
  - `talk_ids` text

---

## **Why two MySQL databases right now (and whether to merge)**

We separated schemas for a practical reason:

- **`rxnorm`** is the *clean normalization backbone* (RxNorm tables + a few helper lists). It’s close to upstream structure and can be reloaded / replaced when we update versions.
- **`umls2025ab`** is the *product layer* (PubMed evidence, conference objects, scoring tables, and fast label tables). It changes frequently as we iterate.

If we merge everything into one schema, upgrades become riskier: a new RxNorm import can lock, bloat, or accidentally break product tables.

**Recommended path (matches the “one graph + db-vector” plan):**
1. Keep `rxnorm` and `umls2025ab` as **source-of-truth staging layers**.
2. Build a **single export model** (nodes + edges + evidence) that is stable:
   - Nodes: Disease (CUI), Drug (RXCUI), ConferenceEvent, Talk, Speaker (string → later ID), Claim, PubMedArticle
   - Edges: disease→drug (evidence), talk→claim, claim→entity (CUI/RXCUI), article→mention, etc.
3. Export into the final runtime stores:
   - Graph DB for relations and traversal
   - Vector DB for embeddings of abstracts, claims, and talk content

That gives you the “one graph” product experience without forcing MySQL to behave like a graph database.

---

## **Conference Parsing Architecture (What we’re doing now)**

### 1) Why WASET is not the primary extraction source
- Many WASET pages are behind **Cloudflare challenge** (HTTP 403 “Just a moment…”), which blocks normal HTTP scraping.
- Program pages often **don’t contain speakers yet**, so even if we fetch them, there’s nothing to enrich.

So the extractor strategy for WASET is:

**A. Only scrape WASET URLs that contain real speaker/program content.**  
In practice, this tends to be “detail” pages (when accessible) rather than program listing pages.

**B. Treat WASET as optional / best-effort** and prioritize sources with reliable speaker/talk data (example: eMedEvents).

### 2) eMedEvents extraction (working source)
We found a reliable pattern:
- The site is Next.js and embeds structured JSON in `__NEXT_DATA__`.
- Specialty pages expose event lists, but **always limited to 9** per tab in initial payload (live/online/free).
- Detail pages contain `detailData`, including a `speakers` array when available.

Current observed run:
- Specialties discovered on-site: **14**
- Total available conferences shown by site: **980**
- Conferences fully loaded in our run: **170**
- Conferences that had speakers: **38**
- Total speaker entries captured: **102**

### 3) What gets stored in MySQL (conference pipeline)
- `conference_event`: stable “event shell”
- `conference_talk`: one row per talk/session when we have a title + speaker (or at least a title)
- `conference_claim`: extracted statements from talk text (or later: from slides/video transcripts), linked to normalized entities via `entity_type` + `entity_id`

This structure is intentionally simple so we can:
- add more sources later (YouTube talks, society sites, journals, etc.)
- unify everything into the final graph export

