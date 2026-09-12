# Data Schema and File Guide

This directory contains UTF-8 JSON outputs for **five Old but Gold (ObG) discussion chains processed with the same pipeline described in the NLP4KGC 2026 paper _From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution_**.

Two of the released discussions are presented in detail in the paper. The other released discussions provide additional machine-readable instances showing that the same construction pipeline has been used beyond those two case studies.

The release preserves source IDs and generated graph annotations, but does **not** redistribute the full ObG comment texts, quotations, source spans, or the original ObG stance / relation / quality annotations. Generated Topic Triple fields may retain words or short phrases derived from the source comments; they are pipeline outputs and are not intended to substitute for the original comments.

## Correspondence to the Proposed Representation

The paper separates the representation into two layers:

- **Comment Discussion Graph (CDG):** participants/comments, reply structure, and argumentative relations associated with the observable discussion structure.
- **Topic Assumption Graph (TAG):** discussion-level Topic Triples and relations reconstructed from the discussion.

The two layers are connected by links from comments to individual Topic Triples or Topic–Topic relations, together with the comment-specific attitudes **support**, **rebuttal**, and **qualification**.

The repository stores the generated outputs used to instantiate these structures. It does not claim that the released JSON alone is a complete end-to-end reproduction package for every intermediate LLM decision.

## ID Conventions

| Field | Meaning / Reference |
| --- | --- |
| `source_discussion_id` | ObG discussion key. Interpret generated IDs within this discussion. |
| `source_split` | Original ObG split (`Train` or `Test`) used by the source data. |
| `source_comment_id` | Original ObG `CommentID`. |
| `source_parent_comment_id` / `source_child_comment_id` | Original parent and child comment IDs used for pair-level ASR and Topic processing. |
| `source_comment_ids` | Original comment IDs used as provenance for a discussion-level Topic. Full comment text is not included here. |
| `pair_id` | Parent–child pair ID generated during preprocessing; not an original ObG field. |
| `pair_claim_id` | Topic identifier within a parent–child extraction record. |
| `topic_id` | Discussion-level Topic ID after consolidation / reassignment. See `03_discussion_topic_index.json`. |
| `relation_id` | Identifier assigned to a Topic–Topic causal-relation candidate. See `04_topic_causal_relation_judgments.json`. |
| `from_topic_id` / `to_topic_id` | The two Topic IDs supplied to the relation-classification task. Use `direction` for the inferred causal direction. |

Generated IDs must be interpreted together with `source_discussion_id`. A `pair_claim_id` must additionally be interpreted with its corresponding `pair_id`.

The `topic_id` field in `02_comment_pair_topic_extraction_records.json` maps a pair-level extraction to its downstream discussion-level Topic in `03_discussion_topic_index.json`.

For Phase II outputs, Topic-oriented files use `topic_id` as the target, while relation-oriented files use `relation_id`.

## Topic Triple Representation

Each record in `03_discussion_topic_index.json` contains the existing discussion-level Topic metadata plus:

```json
"topic_triple": {
  "surface": {
    "subject": "...",
    "predicate": "...",
    "object": "..."
  },
  "normalized": {
    "subject": "...",
    "predicate": "...",
    "object": "..."
  }
}
```

- `surface` is the Topic Triple before normalization, copied from the saved pipeline output.
- `normalized` is the final normalized Topic Triple used for the discussion-level Topic, also copied from the saved pipeline output.
- The current release contains **46 discussion-level Topics**. No Topic records or provenance IDs were added when the Triple contents were exposed.
- Seven normalized `object` values are empty strings because that is the value recorded by the pipeline; they have not been manually filled or reconstructed.

The Topic Triple contents are generated outputs. Full ObG comments, dedicated quotation fields, and source-span fields are not included.

## Argumentative Semantic Relations (ASRs)

`01_comment_pair_asr_annotations.json` contains judgments for the four coarse-grained **Argumentative Semantic Relations (ASRs)** used as contextual cues in the paper:

- `afsj`: Argument from Supportive Justification (AfSJ)
- `afr`: Argument from Rebuttal (AfR)
- `afcc`: Argument from Causal Consequence (AfCC)
- `afac`: Argument from Analogy or Comparison (AfAC)

These ASRs are evaluated on parent–child comment pairs and are used as signals for Topic reconstruction; they should not be interpreted as a complete taxonomy of Walton's Argumentation Schemes.

## Topic–Topic Causal Relation Labels

`04_topic_causal_relation_judgments.json` contains all evaluated Topic-pair candidates. Its implementation labels map to the terminology used in the paper as follows:

| JSON value (`final_causal_status`) | Paper terminology | Retained in Topic structure |
| --- | --- | --- |
| `Explicit` | explicit causal | Yes |
| `Implicit` | implicit causal | Yes |
| `Unsupported` | unrelated | No |

The current release contains **518** evaluated causal-relation candidates: **9 Explicit**, **13 Implicit**, and **496 Unsupported**. Thus, **22** Topic–Topic causal relations are retained under the representation described in the paper.

`from_topic_id` and `to_topic_id` describe the input ordering of the candidate pair. The inferred relation direction is stored separately in `direction`.

## Phase II Links and Attitudes

Phase II is represented by two parallel target types:

- `05a_comment_topic_link_judgments.json` identifies links from comments to individual Topic Triples.
- `05b_comment_causal_relation_link_judgments.json` identifies links from comments to Topic–Topic causal relations.
- `06a_comment_topic_attitudes.json` assigns **support**, **rebuttal**, or **qualification** to identified Comment–Topic targets.
- `06b_comment_causal_relation_attitudes.json` assigns the same three attitude types to identified Comment–Relation targets.

These links provide the cross-layer connections between the observable discussion structure and the reconstructed Topic structure described in the paper.

## Files and Record Counts

| File | Records |
| --- | ---: |
| `01_comment_pair_asr_annotations.json` | 120 |
| `02_comment_pair_topic_extraction_records.json` | 47 |
| `03_discussion_topic_index.json` | 46 |
| `04_topic_causal_relation_judgments.json` | 518 |
| `05a_comment_topic_link_judgments.json` | 167 |
| `05b_comment_causal_relation_link_judgments.json` | 141 |
| `06a_comment_topic_attitudes.json` | 62 |
| `06b_comment_causal_relation_attitudes.json` | 27 |
| `07_discussion_processing_summary.json` | 5 |

Across the five released discussions, the data reference **35 source comments** and contain **46 discussion-level Topics**. The 518 Topic-pair candidates include the 22 retained explicit/implicit causal relations described above.

The record counts, byte sizes, and SHA-256 values of the released JSON files are recorded in `manifest.json`.

## Joining with the Original ObG Dataset

To inspect the generated structures together with the original discussion text, obtain the ObG dataset separately. Using `source_split`, locate `discussions[source_discussion_id].messages` in the corresponding source split and join the released records using `source_comment_id`, `source_parent_comment_id`, `source_child_comment_id`, or `source_comment_ids` against ObG `CommentID` values.

For a direct parent–child reply relation, the parent ID corresponds to the child's `responding_to.comment_id` in the source data used by the pipeline.

The SHA-256 hashes of the ObG source files used during processing are recorded under `source_dataset` in `manifest.json`. The original ObG data themselves are not included in this repository.

## Reproducibility and Release Scope

The public JSON files provide inspectable final structured outputs and provenance for the five released discussions. In particular, they make it possible to inspect:

- pair-level ASR results;
- pair-level Topic extraction records;
- surface and normalized discussion-level Topic Triples;
- explicit / implicit causal Topic–Topic judgments;
- Comment–Topic and Comment–Relation links; and
- support / rebuttal / qualification attitudes.

Where present, records retain fields such as `stage`, `model`, `prompt_version`, `status`, and confidence values from the saved pipeline outputs.

The repository does **not** include the full ObG comments, dedicated quotation/source-span fields, reasoning justifications, chain-of-thought, or every intermediate LLM judgment. The prompt set used for the submission is distributed separately as supplementary material. Accordingly, this repository should be understood as the machine-readable data release supporting inspection of the instantiated representation, rather than as a standalone package for reproducing the complete inference process from raw text.
