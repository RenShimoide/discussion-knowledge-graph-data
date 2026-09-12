# From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution

This repository accompanies the paper **“From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution”**, accepted at **NLP4KGC 2026**.

It provides machine-readable annotations and graph-construction outputs for **five Old but Gold (ObG) discussion chains processed with the same pipeline described in the paper**. Two of these chains are presented in detail in the paper; the remaining released instances provide additional inspectable examples of the proposed representation beyond those two cases.

The main contribution of the paper is a two-layer knowledge representation consisting of a **Comment Discussion Graph (CDG)** and a **Topic Assumption Graph (TAG)**. The files in this repository expose the generated structures used to instantiate that representation, including Argumentative Semantic Relations (ASRs), Topic Triples, Topic–Topic causal relations, Comment–Topic / Comment–Relation links, and comment-specific attitudes.

## Dataset Notice

- **Original ObG data are not redistributed.** This repository does not include the full original comment texts, quotations, or the original ObG stance / relation / quality annotations. To reconstruct the source discussions, obtain the ObG dataset separately and follow its distribution terms.
- **Generated Topic Triples are included.** `data/03_discussion_topic_index.json` contains both the extracted surface Topic Triple and the final normalized Topic Triple for each released discussion-level Topic. These are generated pipeline outputs, not copies of the complete source comments.
- **Stable source IDs are retained.** The released JSON uses the original ObG discussion and comment IDs where needed for provenance and joining; random `public_*` identifiers are not used.
- **Prompts are distributed separately.** The prompt set used for the submission is provided as supplementary material rather than in this repository.

## Relationship to the Paper

The paper describes a two-phase construction procedure:

- **Phase I — ASR identification and Topic structure reconstruction:** ASRs are identified for parent–child comment pairs, Topic Triples are extracted and normalized, and Topic–Topic relations are classified as **explicit causal**, **implicit causal**, or unrelated.
- **Phase II — Comment-to-Topic linking:** comments are linked to Topic Triples or Topic–Topic relations, and the expressed attitude toward each identified target is classified as **support**, **rebuttal**, or **qualification**.

The JSON files below expose these stages at a finer level of granularity. In `04_topic_causal_relation_judgments.json`, the implementation labels corresponding to the paper are:

- `Explicit` → explicit causal
- `Implicit` → implicit causal
- `Unsupported` → unrelated / not retained as a Topic–Topic causal relation

Only the Explicit and Implicit cases correspond to the causal relations retained in the Topic structure described in the paper.

## Data Pipeline and File Structure

`data/03_discussion_topic_index.json` is the discussion-level Topic index. It contains each existing `topic_id`, its provenance through `source_comment_ids`, and both the **surface** and **normalized** subject–predicate–object forms of the Topic Triple. The saved pipeline output is preserved, including seven normalized Triples whose `object` field is an empty string.

The repository files correspond to the construction flow as follows. Steps 2–3 together expose the Topic extraction / normalization part of Phase I, while Steps 5–6 expose the two target types handled in Phase II.

```text
[1] 01_comment_pair_asr_annotations.json
       │
[2] 02_comment_pair_topic_extraction_records.json
       │
[3] 03_discussion_topic_index.json
       │
[4] 04_topic_causal_relation_judgments.json
       ├───────────────────────────────────────────┐
       ▼                                           ▼
[5a] 05a_comment_topic_link_judgments.json   [5b] 05b_comment_causal_relation_link_judgments.json
       │                                           │
[6a] 06a_comment_topic_attitudes.json        [6b] 06b_comment_causal_relation_attitudes.json
       └───────────────────────────────────────────┘
                       │
                       ▼
         [7] 07_discussion_processing_summary.json
```

`07_discussion_processing_summary.json` is a summary artifact generated after the other files; it is not an additional inference stage. For a quick overview of the five released discussions, it can be inspected first.

See [`data/README_Data.md`](data/README_Data.md) for field definitions, record counts, joining instructions, and reproducibility scope.

## Visualization Tool

An interactive visualization tool for exploring the constructed Discussion Knowledge Graphs is planned to be linked from this repository by the **first day of the NLP4KGC 2026 workshop (October 25, 2026)**.
