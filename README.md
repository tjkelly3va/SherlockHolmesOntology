# Sherlock Holmes Narrative Ontology

![Sherlock Holmes](SherlockImage.png)

An RDF/OWL ontology extracted from Arthur Conan Doyle's four Sherlock Holmes novels, capturing the characters, locations, objects, organizations, events, and relationships that constitute the narrative world of the canon's long-form works.

## Scope

The ontology covers the four novels in publication order:

| Novel | Year | Setting |
|-------|------|---------|
| *A Study in Scarlet* | 1887 | London; Utah Territory (flashback, 1847–1860) |
| *The Sign of the Four* | 1890 | London; Agra, India (flashback, 1857) |
| *The Hound of the Baskervilles* | 1902 | London; Dartmoor, Devonshire |
| *The Valley of Fear* | 1915 | Sussex; Vermissa Valley, Pennsylvania (flashback, 1875) |

Each novel was read in its entirety and processed as an extraction pass, with entities accumulated into a single growing ontology file.

## Ontology at a Glance

| Metric | Count |
|--------|-------|
| OWL classes | 113 |
| Object properties | 52 |
| Data properties | 30 |
| Named individuals | 321 |
| Persons (fictional + real) | ~123 |
| Locations | ~90 |
| Source documents | 4 |
| File size | ~208 KB, ~4,700 lines |

The ontology is serialized in Turtle (.ttl) format and uses W3C standards: OWL 2 for class and property definitions, Dublin Core for provenance metadata, SKOS for alternate labels, and Schema.org for bibliographic typing.

## Structure

The file is organized in clearly delimited sections:

1. **Namespace declarations** — standard prefixes plus two project-specific namespaces (`:` for individuals, `ex:` for ontological terms)
2. **Ontology declaration** — title, creator, date, and initial source reference
3. **Core subproperty definitions** — `ex:prefLabel`, `ex:description`, and `ex:memberOfOntology`, defined as subproperties of SKOS and Dublin Core terms to support inference
4. **Class definitions** — sorted alphabetically, with explicit `rdfs:subClassOf` hierarchy
5. **Property definitions** — object and data properties, with domain/range declarations and inverse relationships
6. **Individual instances** — grouped by class, each with human-readable labels, descriptions, relationships, ontology membership, and source provenance
7. **Source provenance** — bibliographic records for each novel as a `schema:Book` individual
8. **Per-novel additions** — new classes, properties, enrichments, and individuals introduced by each successive novel

### Key Modeling Decisions

**Two-axis person classification.** Persons are classified along two independent axes: ontological status (`FictionalPerson` vs. `RealPerson`, mutually exclusive) and narrative role (`Detective`, `Criminal`, `Victim`, `Client`, etc., freely combinable). This avoids the modeling trap of making roles depend on fictionality.

**Cumulative enrichment.** When a character appears in multiple novels, new facts are added to the existing entity without modifying or removing anything established by earlier sources. Multiple `dc:source` values track which novels contributed information. Sherlock Holmes, for example, carries source attributions from all four novels.

**Dual geographic containment.** Every location asserts both a transitive general property (`ex:locatedIn`) and a specific-level property (`ex:inCity`, `ex:inRegion`, `ex:inCountry`). This ensures queryability without requiring a reasoner to resolve transitive chains.

**ISO 3166-1 compliance.** Sovereign states are typed as `ex:Country` with alpha-2 codes. Constituent parts of the United Kingdom (England, Scotland, Wales) are typed as `ex:Region`, not `ex:Country`.

**Alias handling.** Characters with multiple identities (notably John Douglas / Jack McMurdo / Birdy Edwards in *The Valley of Fear*) use `skos:altLabel` for display variants, `ex:alias` for in-story assumed names, and `ex:aliasOf`/`ex:hasAlias` object properties for formal identity linking between URI references.

## How the Ontology Was Built

This ontology was built collaboratively by a human knowledge engineer and Claude (Anthropic), working through an iterative development process that refined both the extraction methodology and the ontology itself over the course of the four novels.

### The Process

Each novel followed a consistent pipeline:

1. **Document assessment.** The source text was measured by word count to determine whether single-pass or multi-pass extraction was needed. Novels under 40,000 words used a single pass; *The Valley of Fear* (57,600 words) required a two-phase approach with an intermediate extraction manifest.

2. **Close reading.** Every chapter was read sequentially in its entirety. This was a deliberate methodological choice — early experiments with keyword search and pattern-matching approaches missed roughly 44% of extractable entities compared to close reading. Incidental mentions in dialogue, background details on cigarette stubs, and characters introduced only by description are invisible to grep.

3. **Entity extraction.** Characters, locations, objects, organizations, publications, and events were identified and categorized. Each entity was assessed against the existing ontology: new entities were added, entities already present were enriched with newly discovered facts, and entities mentioned without new information were left unchanged.

4. **Turtle generation.** The extracted entities were serialized into well-formed OWL/Turtle, following strict conventions for URI naming, predicate ordering, annotation requirements, and file structure.

5. **Merge and verification.** New content was appended to the cumulative baseline, with automated checks for duplicate definitions, dangling semicolons, and schema consistency.

### Claude's Role

Claude participated in this project at multiple levels, not just as an extraction engine:

**Skill development.** The five custom skills that govern this project's methodology — `ontology-extraction`, `ontology-cumulative-merge`, `turtle-style-guide`, `large-file-generation`, and `token-checkpoint-protocol` — were developed collaboratively. Claude helped draft, test, and refine these skill documents through iterative trial and error. When an extraction pass produced errors (duplicate definitions, missed entities, inconsistent formatting), those failures were analyzed and encoded as explicit rules in the skill files, creating an improving feedback loop.

**Schema evolution.** The class hierarchy and property vocabulary grew organically as each novel introduced new narrative elements. *A Study in Scarlet* established the foundation. *The Sign of the Four* added classes for river craft, treasure, and colonial military structures. *The Hound of the Baskervilles* introduced geographic features, naturalists, and the county system. *The Valley of Fear* brought secret societies, mining operations, and formal alias-identity linking. Claude proposed new classes and properties as they were needed and checked them against existing patterns to avoid redundancy.

**Methodology lessons.** Several hard-won practices emerged from the collaborative process:

- Close reading was established as mandatory after pattern-matching failed badly on *A Study in Scarlet*.
- The two-phase extraction approach (manifest then merge) was developed for *The Valley of Fear* when single-pass extraction proved infeasible for a 57,600-word novel within context window constraints.
- The enrichment protocol (preserving all existing triples, adding only new ones, tracking multi-source provenance) was formalized after early merges accidentally overwrote baseline content.
- The "Schema Authority Rule" — that the baseline ontology supersedes the reference schema once it contains real content — was introduced to prevent pattern conflicts between the evolving ontology and its bootstrap template.

**Quality verification.** After each merge, Claude ran automated checks: no duplicate URIs, no dangling semicolons, all definitions properly terminated, spot-checks on randomly selected entities from both baseline and new content. These checks caught real errors — for instance, `:california` and `:unitedStates` were initially categorized as new entities during the *Valley of Fear* extraction but were found to already exist from *A Study in Scarlet* during merge verification.

### Tools and Standards

- **Format:** Turtle (.ttl) — W3C RDF 1.1 Turtle serialization
- **Schema language:** OWL 2
- **Vocabularies:** Dublin Core (provenance), SKOS (labeling), Schema.org (bibliographic metadata)
- **Generation environment:** Python 3 scripts within Claude's sandboxed Linux environment
- **Validation:** Automated syntax checks plus manual spot-checking after each merge

## Usage

The ontology can be loaded into any RDF triplestore or queried with SPARQL. Example queries:

```sparql
# All characters who appear in multiple novels
SELECT ?person ?name (COUNT(?source) AS ?novels)
WHERE {
    ?person a ex:FictionalPerson ;
            ex:prefLabel ?name ;
            dc:source ?source .
}
GROUP BY ?person ?name
HAVING (COUNT(?source) > 1)
ORDER BY DESC(?novels)
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
# Moriarty's known associates and possessions
SELECT ?rel ?target ?targetName
WHERE {
    :professorMoriarty ?rel ?target .
    ?target ex:prefLabel ?targetName .
    FILTER (?rel != ex:memberOfOntology)
}
```

## License

The ontology structure and extraction methodology are original work. The source texts are in the public domain.

## Acknowledgments

Built collaboratively by Tom Kelly and Claude (Anthropic, Claude Opus 4.6), February 2026.
Corrections and enhancement suggestions are invited.  Please email your comments to tjk45268@gmail.com
