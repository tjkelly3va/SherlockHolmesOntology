# Sherlock Holmes Narrative Ontology

![Sherlock Holmes](SherlockImage.png)

An RDF/OWL ontology extracted from Arthur Conan Doyle's complete Sherlock Holmes canon — four novels and fifty-six short stories — capturing the characters, locations, objects, organizations, events, deductions, and relationships that constitute the narrative world of the world's most famous detective.

## Why This Exists

In the Sherlock Holmes canon, which characters appear in more than one story? Who are Moriarty's known associates? What are Holmes's deductions in "The Adventure of the Speckled Band"? You could ask an AI chatbot like ChatGPT or Claude, and it might give you the right answers — but it might also make things up. These tools are known to "hallucinate": inventing characters who don't exist, attributing deductions to the wrong story, or confidently describing relationships that Doyle never wrote. They sound authoritative, but they have no reliable way to check their answers against the original texts.

This project takes a different approach. Instead of relying on an AI's memory, it organizes everything from the Holmes stories — every character, location, object, event, organization, and deduction — into a structured knowledge base called an ontology. Think of it as a carefully indexed reference work where every fact is linked back to the specific story it came from. It doesn't guess, and it doesn't fill in gaps. If Holmes made a deduction in "The Speckled Band," it's recorded here with a direct link to that story. If a character appears in three stories, the ontology says exactly three — no more, no less. 

The real power comes from combining these two technologies. An AI chatbot gives you the ability to ask questions in plain English; the ontology gives it a foundation of verified facts to draw on. Together, they can answer complex questions about the Holmes canon conversationally, point you to the specific stories that support each answer, and — just as importantly — hold back when the evidence isn't there rather than making something up.

## Scope

The ontology covers all sixty works in the Sherlock Holmes canon. Source texts were obtained from the public domain archive at [sherlock-holm.es](https://sherlock-holm.es/), and each source document in the ontology includes an `rdfs:seeAlso` link to its plain-text source, supporting ingestion by separate RAG (Retrieval-Augmented Generation) pipelines.

### Novels

| Novel | Published | Setting |
|-------|-----------|---------|
| [*A Study in Scarlet*](https://sherlock-holm.es/stories/plain-text/stud.txt) | 1887 | London; Utah Territory (flashback, 1847–1860) |
| [*The Sign of the Four*](https://sherlock-holm.es/stories/plain-text/sign.txt) | 1890 | London; Agra, India (flashback, 1857) |
| [*The Hound of the Baskervilles*](https://sherlock-holm.es/stories/plain-text/houn.txt) | 1902 | London; Dartmoor, Devonshire |
| [*The Valley of Fear*](https://sherlock-holm.es/stories/plain-text/vall.txt) | 1915 | Sussex; Vermissa Valley, Pennsylvania (flashback, 1875) |

### Short Stories (56)

All fifty-six short stories from the five canonical collections have been processed:

| Collection | Stories | Years |
|------------|---------|-------|
| *The Adventures of Sherlock Holmes* | 12 stories | 1891–1892 |
| *The Memoirs of Sherlock Holmes* | 11 stories | 1892–1893 |
| *The Return of Sherlock Holmes* | 13 stories | 1903–1904 |
| *His Last Bow* | 8 stories | 1908–1917 |
| *The Case Book of Sherlock Holmes* | 12 stories | 1921–1927 |

Complete story listing with publication years and source links is encoded in the ontology's SOURCE PROVENANCE section, where each work is typed as `schema:Book` or `schema:ShortStory` with Dublin Core metadata and an `rdfs:seeAlso` URL to the public domain source text.

## Ontology at a Glance

| Metric | Count |
|--------|-------|
| OWL classes | 484 |
| Object properties | 348 |
| Data properties | 96 |
| Named individuals (non-deduction) | ~3,490 |
| Persons (fictional + real) | ~1,000 |
| Locations | ~930 |
| Objects | ~650 |
| Events | ~450 |
| Organizations | ~150 |
| Deduction instances | 785 |
| Literary reviews | 60 (one per source document) |
| Source documents | 60 |
| File size | ~3.8 MB, ~56,800 lines |

The ontology is serialized in Turtle (.ttl) format and uses W3C standards: OWL 2 for class and property definitions, Dublin Core for provenance metadata, SKOS for alternate labels, and Schema.org for bibliographic typing.

## Seven Orthogonal Pillars

The ontology is organized around seven independently queryable dimensions. The first five represent the narrative world — every individual descends from exactly one top-level class with no shared parent, ensuring clean categorization. The sixth and seventh pillars (deductions and reviews) layer analytical and critical perspectives on top of the narrative content.

| Pillar | Scope | Examples |
|--------|-------|----------|
| **Persons** | Agents: fictional characters and historical figures | Holmes, Watson, Irene Adler, Professor Moriarty |
| **Locations** | Spatial entities at every scale, from rooms to countries | 221B Baker Street, Dartmoor, Vermissa Valley, Agra |
| **Objects** | Physical things: documents, weapons, animals, vehicles | the Agra treasure, the Blue Carbuncle, the Hound |
| **Events** | Occurrences with participants and locations | murders, investigations, journeys, trials |
| **Organizations** | Groups, firms, institutions, criminal enterprises | Scotland Yard, the Scowrers, the Baker Street Irregulars |
| **Deductions** | Sherlock Holmes's (and others') inferential reasoning | 785 structured deduction instances with observation, reasoning chain, and conclusion |
| **Reviews** | Literary critical analysis of each source document | themes, narrative structure, Doyle's craft — one per work |

The deductions pillar captures each act of reasoning as a first-class entity: who made the deduction, what was observed, the logical chain connecting observation to conclusion, and the narrative outcome. This enables queries like "find all deductions where Holmes reasons from physical evidence" or "compare deductive methods across the canon."

The reviews pillar provides a scholarly layer alongside the factual extraction, analyzing each of the sixty works for themes, narrative structure, character development, and Doyle's evolving craft across four decades of writing.

## Source Provenance and RAG Support

Each source document is modeled as a named individual with full bibliographic metadata and a URL to the public domain text:

```turtle
:studyInScarletDoc a schema:Book ;
    ex:prefLabel "A Study in Scarlet" ;
    dc:title "A Study in Scarlet" ;
    dc:creator "Arthur Conan Doyle" ;
    ex:publicationYear "1887"^^xsd:gYear ;
    ex:plotSummary """...""" ;
    rdfs:seeAlso "https://sherlock-holm.es/stories/plain-text/stud.txt" ;
    ex:memberOfOntology ex:SherlockOntology .
```

The `rdfs:seeAlso` links enable automated ingestion of the original texts by external RAG systems, connecting the structured ontology to the unstructured source material. Every entity in the ontology carries `dc:source` attribution back to the specific work(s) that introduced it, and entities enriched across multiple stories accumulate multiple `dc:source` values, creating a complete provenance chain from any fact in the ontology back to its textual origin.

## Structure

The merged ontology file is organized in clearly delimited sections:

1. **Namespace declarations** — standard prefixes plus two project-specific namespaces (`:` for individuals, `ex:` for ontological terms)
2. **Ontology declaration** — title, creator, date, and cumulative source reference list
3. **Core subproperty definitions** — `ex:prefLabel`, `ex:description`, and `ex:memberOfOntology`, defined as subproperties of SKOS and Dublin Core terms
4. **Source provenance** — bibliographic records for all sixty works, with plot summaries and source URLs
5. **Class definitions** — 484 classes sorted alphabetically, with explicit `rdfs:subClassOf` hierarchy
6. **Object property definitions** — 348 properties with domain, range, and inverse declarations
7. **Data property definitions** — 96 properties with domain and range
8. **Recurring characters** — full definitions for characters appearing across many stories (Holmes, Watson, Lestrade, Mrs. Hudson, Mary Morstan, Moriarty, Wiggins, the Baker Street Irregulars)
9. **Deductions index** — forward links from each source document to its extracted deduction instances
10. **Persons** — full definitions for all non-recurring characters
11. **Locations** — full definitions for all places
12. **Objects** — full definitions for documents, weapons, animals, vehicles, and other physical entities
13. **Events** — full definitions for crimes, investigations, journeys, and other occurrences
14. **Organizations** — full definitions for institutions, firms, criminal enterprises, and groups
15. **Deductions** — 785 individual deduction instances capturing Holmes's (and others') reasoning
16. **Reviews** — literary critical analysis for each of the sixty source documents

## How the Ontology Was Built

This ontology was built collaboratively by Tom Kelly (human knowledge engineer) and Claude (Anthropic, Claude Opus 4.6), working through an iterative development process that refined both the extraction methodology and the ontology itself over the course of all sixty works.

### The Processing Pipeline

Each work followed a consistent pipeline governed by a consolidated extraction prompt (over 1,400 lines), which encoded every rule, convention, and hard-won lesson from the project's development:

1. **Context budget assessment.** The source text was measured by word count and the baseline ontology by line count to determine processing mode. Short stories used a two-phase approach (reading in one conversation turn, extraction and generation in subsequent turns); longer novels required three-phase processing with intermediate extraction manifests. This assessment was mandatory — the most common failure mode in early development was attempting to read a text and extract from it in the same conversation turn, exhausting Claude's output token budget before the extraction scripts could be written.

2. **Close reading.** Every chapter and paragraph was read sequentially in its entirety. This was a deliberate methodological choice established early in the project — experiments with keyword search and pattern-matching approaches missed roughly 44% of extractable entities compared to close reading. Incidental mentions in dialogue, background details, and characters introduced only by description are invisible to automated search.

3. **Two-pass entity extraction.** Pass 1 was a sequential read-through extracting every entity as encountered. Pass 2 was a category-driven audit, systematically checking each entity type (persons, locations, objects, events, organizations) against the text to catch entities missed in the first pass. A reconciliation report documented the differences between passes, and benchmark gates ensured extraction counts met minimum thresholds before proceeding.

4. **Entity classification.** Every extracted entity was classified against the existing baseline as NEW (not previously seen), ENRICHED (already exists but this work adds new facts), or UNCHANGED (appears but contributes nothing new). This classification happened after extraction was complete — never during reading — to avoid the distortion of filtering entities based on what already existed.

5. **Turtle generation.** A Python script read the extraction manifest and the existing ontology files, classified entities authoritatively against the baseline, and wrote all updates atomically — new schema terms, source documents, entity stubs, and full definitions — in a single execution. The script enforced alphabetical ordering, proper punctuation, and all formatting conventions.

6. **Verification.** Automated checks confirmed no duplicate URIs, no dangling semicolons, all definitions properly terminated, entity counts matching expectations, and spot-checks on randomly selected entities from both baseline and new content.

### Split-File Architecture

As the ontology grew beyond what could be efficiently processed in a single file, a split-file architecture was adopted. The ontology is maintained as eight working files:

- **Baseline index** (`sherlock_baseline.ttl`) — schema definitions, recurring characters, entity stubs, source provenance, and the deductions index
- **Seven pillar files** — full definitions for persons, locations, objects, events, organizations, deductions, and literary critical reviews per source document

This architecture reduces the generation script's read/write burden from the full ontology (56,000+ lines) to approximately 4,500 lines per story. A periodic merge process combines all eight files into a single `sherlock_holmes_ontology.ttl` for querying, validation, and distribution. The merge process itself is governed by a dedicated merge prompt that handles namespace normalization, enrichment addendum consolidation, cross-pillar duplicate detection, and comprehensive verification.

### Context Window Management

Processing sixty works through an LLM with finite context windows required deliberate memory management strategies, several of which were developed through trial and error:

**Token budget monitoring.** Every processing turn tracked estimated token consumption against a 70% safety threshold. At 70%, the current chunk was saved and a checkpoint issued. At 85%, processing stopped unconditionally. This simple rule — established after several costly truncation incidents — prevented more data loss than any other single practice.

**Hard phase boundaries.** Reading and extraction were never permitted in the same conversation turn, even for short stories. A 7,000-word story requires 3–5 view calls; combined with output framing overhead, this consumes enough of the output budget to truncate the extraction manifest. The cost of an extra checkpoint (one user reply) was always less than the cost of lost work.

**Filesystem as ground truth.** The extraction pipeline wrote all intermediate artifacts (reading evidence, pass 1 and pass 2 entity lists, reconciliation reports, extraction manifests) to disk as JSON files. When context resets occurred between conversation turns, recovery started from the filesystem — never from summaries or conversation history. This principle was formalized after an early incident where reconstructing entity data from a conversation summary introduced errors that propagated through the merge.

**Manifest-driven generation.** The extraction manifest served as the sole bridge between Phase 1 (reading and extraction, which required the source text) and Phase 2 (generation, which required only the manifest and the ontology files). This separation meant the full novel text did not need to be in context during generation — the manifest contained everything the generation script needed.

**Three-call ceiling for generation.** Phase 2 was constrained to exactly three tool calls: read the manifest, execute the generation script, and verify. Every "quick inspection" of the baseline before writing the generation script cost approximately 500 tokens of output framing. After observing that eight pre-generation inspection calls consumed 4,000 tokens — enough to write the entire generation script — the three-call ceiling was made absolute.

**Atomic script generation.** Structured output files (Turtle, JSON manifests) were always generated by a single Python script writing the complete file. Incremental generation across multiple tool calls — persons in one script, locations in the next — wasted tokens on inter-call narration and risked truncation. For very large entity sets (100+ entities), a two-step accumulator pattern was used: write entity data to an intermediate file first, then assemble the complete output in a second script, with zero narration between calls.

**Custom skills as persistent memory.** Five custom skill files served as Claude's institutional memory across conversation boundaries: `ontology-extraction` (close reading and entity extraction rules), `ontology-cumulative-merge` (enrichment protocol and merge discipline), `turtle-style-guide` (formatting, naming, and annotation conventions), `large-file-generation` (tool selection and Python-first file creation), and `token-checkpoint-protocol` (context window monitoring and checkpoint management). Because each conversation turn starts with a fresh context, these skills ensured that methodology lessons learned from processing *A Study in Scarlet* were still applied fifty-eight stories later when processing *The Adventure of Shoscombe Old Place*. Without them, every conversation would have started from scratch.

### Quality Testing and Verification

Quality assurance was built into every stage of the pipeline, not applied as a post-processing step:

**Extraction benchmarks.** Expected entity counts per 50,000 words (40–80 persons, 30–60 locations, 20–50 objects, 5–15 organizations, 15–40 events) served as minimum thresholds. If counts fell below 70% of the expected range, processing stopped and the extraction was reviewed before proceeding. An explicit anti-curation rule — "if you catch yourself using 'major' or 'notable' to filter, you are curating, not extracting" — prevented Claude from skipping minor characters or background locations.

**Mandatory review gate.** After extraction and before generation, entity counts and the complete entity name list were presented for human review. Generation could not begin without explicit approval. This gate caught misclassifications, missed entities, and naming inconsistencies that automated checks could not detect.

**Post-generation verification.** Every generation run verified: each new entity had both a baseline stub and a pillar file definition; each enriched entity's stub had the new `dc:source` added; new schema terms were inserted in correct alphabetical position; no duplicate URIs existed; definitions were properly terminated; and spot-checks confirmed 3–5 entities matched expectations.

**Periodic merge validation.** The merge process performed its own verification suite: entity count reconciliation across all pillar files, schema completeness checks, namespace normalization verification, duplicate subject detection, double-semicolon scanning (indicating punctuation errors), cross-pillar duplicate detection (indicating extraction routing errors), and standalone subject URI line checks to catch post-processing corruption.

**Standards compliance.** The ontology follows W3C OWL 2 specifications, with every class, property, and individual carrying four mandatory annotations (`ex:prefLabel`, `ex:description`, `ex:memberOfOntology`, `dc:source`). Every object property declares domain and range. Every property definition includes inverse relationships where applicable. Symmetric properties (`ex:isMarriedTo`, `ex:siblingOf`) are declared as `owl:SymmetricProperty`. Disjoint classes (`FictionalPerson` and `RealPerson`) are formally declared.

## Key Modeling Decisions

**Two-axis person classification.** Persons are classified along two independent axes: ontological status (`FictionalPerson` vs. `RealPerson`, mutually exclusive) and narrative role (`Detective`, `Criminal`, `Victim`, `Client`, etc., freely combinable). This avoids the modeling trap of making roles depend on fictionality. A character like Sherlock Holmes is typed as both `ex:FictionalPerson` and `ex:Detective` — the role class is a subclass of `ex:Person` directly, never nested under `FictionalPerson`.

**Five orthogonal entity pillars.** Every individual descends from exactly one of five top-level classes — `ex:Person`, `ex:Location`, `ex:Object`, `ex:Event`, or `ex:Organization` — ensuring clean categorization and enabling pillar-specific queries without class overlap.

**Cumulative enrichment.** When a character, location, or object appears in multiple works, new facts are added to the existing entity without modifying or removing anything established by earlier sources. Multiple `dc:source` values track which works contributed information. Sherlock Holmes carries source attributions from all sixty works.

**Dual geographic containment.** Every location asserts both a transitive general property (`ex:locatedIn`) and a specific-level property (`ex:inCity`, `ex:inRegion`, `ex:inCountry`, `ex:inCounty`, `ex:inState`). Full chains are asserted explicitly — `221B Baker Street` links to Baker Street, London, England, and the United Kingdom — ensuring queryability without requiring a reasoner to resolve transitive chains.

**ISO 3166-1 compliance.** Sovereign states are typed as `ex:Country` with ISO alpha-2 codes. Constituent parts of the United Kingdom (England, Scotland, Wales) are typed as `ex:Region`, not `ex:Country`.

**Alias and identity handling.** Characters with multiple identities use `skos:altLabel` for display variants, `ex:alias` for in-story assumed names, and `ex:aliasOf`/`ex:hasAlias` object properties for formal identity linking between URI references. This is particularly important for characters like John Douglas / Jack McMurdo / Birdy Edwards in *The Valley of Fear*.

**Deduction modeling.** Each deductive inference made by Holmes (and occasionally by other characters) is captured as a first-class entity with structured properties including the deducer, the observation, the reasoning chain, and the conclusion. The deductions index in the ontology provides forward links from each source document to its extracted deductions, enabling queries like "show all deductions in *The Speckled Band*" or "find all deductions where Holmes reasons from physical evidence."

**Literary reviews.** Each source document carries a critical literary review analyzing themes, narrative structure, and Doyle's craft, providing a scholarly layer alongside the factual extraction.

### Claude's Role in Development

Claude participated in this project at multiple levels, not just as an extraction engine:

**Skill development.** Five custom skill files govern the project's methodology. These skills were developed collaboratively through iterative trial and error. When an extraction pass produced errors — duplicate definitions, missed entities, inconsistent formatting, truncated output — those failures were analyzed and encoded as explicit rules in the skill files, creating an improving feedback loop. The skills functioned as Claude's institutional memory, ensuring that lessons learned early in the project persisted across the hundreds of conversation turns required to process the full canon.

**Prompt engineering.** The consolidated extraction prompt (over 1,400 lines) evolved across the project as new failure modes were discovered and addressed. Claude helped identify, diagnose, and codify solutions for problems like: regex-based block parsing failing on multi-line descriptions (`re.DOTALL` corruption), enrichment addenda being routed to wrong pillar files, alphabetical insertion algorithms splitting multi-line definitions, triple-quoted strings being misidentified as block boundaries, and punctuation normalization producing double semicolons. Each fix was expressed as both a rule and a code pattern in the prompt, with an explicit failure modes checklist growing to over thirty entries.

**Schema evolution.** The class hierarchy and property vocabulary grew organically as each work introduced new narrative elements. *A Study in Scarlet* established the foundation with core person, location, and event classes. *The Sign of the Four* added classes for river craft, treasure, and colonial military structures. *The Hound of the Baskervilles* introduced geographic features, naturalists, and the county system. *The Valley of Fear* brought secret societies, mining operations, and formal alias-identity linking. As the short stories were processed, the schema continued to expand with increasingly specialized classes — from `ex:Apiarist` (beekeeper) to `ex:Zoologist`, from `ex:TelegraphOffice` to `ex:OpiumDen`. Claude proposed new classes and properties as they were needed, checking them against existing patterns to avoid redundancy.

**Methodology lessons.** Several hard-won practices emerged from the collaborative process:

- Close reading was established as mandatory after pattern-matching failed badly on *A Study in Scarlet*, missing 44% of extractable entities.
- The two-phase processing approach (reading in one turn, extraction in the next) was developed after repeated output truncation incidents.
- The three-phase approach (manifest then merge) was created for *The Valley of Fear* when single-pass extraction proved infeasible for a 57,600-word novel within context window constraints.
- The enrichment protocol (preserving all existing triples, adding only new ones, tracking multi-source provenance) was formalized after early merges accidentally overwrote baseline content.
- The "Schema Authority Rule" — that the baseline ontology supersedes the reference schema once it contains real content — was introduced to prevent conflicts between the evolving ontology and its bootstrap template.
- The split-file architecture was adopted when the monolithic ontology file grew too large for efficient single-file processing within token constraints.
- The anti-curation signal ("if you catch yourself using 'major' or 'notable' to filter, you are curating, not extracting") was added after observing that Claude would sometimes skip minor characters or background locations that nonetheless appear in the text.

## Usage

The ontology can be loaded into any RDF triplestore or queried with SPARQL. Example queries:

```sparql
# All characters who appear in multiple stories
SELECT ?person ?name (COUNT(?source) AS ?stories)
WHERE {
    ?person a ex:FictionalPerson ;
            ex:prefLabel ?name ;
            dc:source ?source .
}
GROUP BY ?person ?name
HAVING (COUNT(?source) > 1)
ORDER BY DESC(?stories)
```

```sparql
# All locations in the Vermissa Valley
SELECT ?place ?name ?type
WHERE {
    ?place ex:locatedIn* :vermissaValley ;
           ex:prefLabel ?name ;
           a ?type .
    FILTER (?type != owl:NamedIndividual)
}
```

```sparql
# Holmes's deductions in a specific story
SELECT ?deduction ?observation ?conclusion
WHERE {
    :speckledBandDoc ex:hasDeductionFrom ?deduction .
    ?deduction ex:deducedBy :sherlockHolmes ;
               ex:observation ?observation ;
               ex:conclusion ?conclusion .
}
```

```sparql
# All source texts with their public domain URLs
SELECT ?title ?year ?url
WHERE {
    ?doc dc:title ?title ;
         ex:publicationYear ?year ;
         rdfs:seeAlso ?url .
}
ORDER BY ?year
```

```sparql
# Moriarty's known associates and relationships
SELECT ?rel ?target ?targetName
WHERE {
    :professorMoriarty ?rel ?target .
    ?target ex:prefLabel ?targetName .
    FILTER (?rel != ex:memberOfOntology)
}
```

## Tools and Standards

- **Format:** Turtle (.ttl) — W3C RDF 1.1 Turtle serialization
- **Schema language:** OWL 2
- **Vocabularies:** Dublin Core (provenance), SKOS (labeling), Schema.org (bibliographic metadata)
- **Generation environment:** Python 3 scripts within Claude's sandboxed Linux environment
- **Validation:** Automated syntax checks, entity count verification, namespace validation, and manual spot-checking after each extraction and merge cycle

## License

The ontology structure, extraction methodology, and all associated tooling are original work by Tom Kelly and Claude (Anthropic, Claude Opus 4.6).

**Permitted uses:** The ontology may be used freely for personal, educational, and research purposes. Commercial use is permitted for demonstration, prototyping, and integration purposes, provided that attribution is given to Tom Kelly and Claude Opus 4.6 (Anthropic) as the creators.

**Attribution requirement:** Any commercial use of the ontology must include the following attribution: "Sherlock Holmes Narrative Ontology by Tom Kelly and Claude Opus 4.6 (Anthropic), 2026."

**Not permitted:** Sale of the ontology, in whole or in part, as a standalone product is not permitted. Redistribution for sale, whether modified or unmodified, is prohibited.

The source texts from which the ontology was extracted are in the public domain.

## Acknowledgments

Built collaboratively by Tom Kelly and Claude (Anthropic, Claude Opus 4.6), February–March 2026.

Special thanks to Bob DuCharme (https://www.bobdc.com) for his contributions.

Corrections, enhancement suggestions, and queries are invited. Please email comments to tjk45268@gmail.com.
