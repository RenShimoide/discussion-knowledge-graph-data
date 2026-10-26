# Discussion Knowledge Graphs

This repository accompanies the paper **“From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution”**, accepted at **NLP4KGC 2026**.

It provides machine-readable outputs from the construction of Discussion Knowledge Graphs for **25 discussion chains** from the Old but Gold (ObG) dataset, as well as **10 interactive discussion examples**.

The proposed representation contains two layers:

- **Comment Discussion Graph (CDG):** represents comment-level discussion structure and expressed attitudes.
- **Topic Assumption Graph (TAG):** represents Topic Triples and relations between topics.

## Interactive demonstrations

Open **[Discussion Knowledge Graph Explorer](https://renshimoide.github.io/discussion-knowledge-graph-data/)** to explore 10 examples. The linked HTML pages provide interactive views of the CDG and TAG.

**Note:** The interactive HTML demonstrations will be taken offline after the NLP4KGC 2026 presentation session. The graph data in this repository will remain publicly available.

## Data

The [`data/`](data/) directory includes per-discussion and aggregated JSON outputs for the graph-construction process.

- **[`data/README.md`](data/README.md)** — file descriptions, data fields, record counts, and joining instructions.
- **[`data/aggregate_manifest.json`](data/aggregate_manifest.json)** — released discussion IDs and aggregate record counts.

### Construction overview

1. Identify Argumentative Semantic Relations (ASRs) in comment pairs.
2. Extract and normalize Topic Triples.
3. Assess causal relations between topics.
4. Link comments to topics and topic relations.
5. Classify expressed attitudes as **support**, **rebuttal**, or **qualification**.

The stored files include intermediate judgments and final outputs associated with these steps.

## Source and supplementary materials

The work builds on the **Old but Gold (ObG)** dataset. Source discussion and comment identifiers are retained to support provenance and cross-file joins. The released files consist of research annotations and generated outputs; they are not a complete copy of the original ObG dataset. Access the source dataset separately under its applicable terms.

Prompts for the research are provided separately as supplementary materials.
