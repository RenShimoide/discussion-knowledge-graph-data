# Discussion Knowledge Graph Data

This directory contains the machine-readable outputs for **25 discussion chains** from the **Old but Gold (ObG)** dataset. Records are available both as aggregate collections and as files organized by discussion.

## Directory structure

```text
data/
├── README.md
├── aggregate_manifest.json
├── 01_comment_pair_asr_annotations.json
├── 02_comment_pair_topic_extraction_records.json
├── 03_discussion_topic_index.json
├── 04_topic_causal_relation_judgments.json
├── 05a_comment_topic_link_judgments.json
├── 05b_comment_causal_relation_link_judgments.json
├── 06a_comment_topic_attitudes.json
├── 06b_comment_causal_relation_attitudes.json
├── 07_discussion_processing_summary.json
└── discussion_<id>/
    └── [the corresponding nine per-discussion JSON files]
```

Files in the `data/` root aggregate records from the 25 discussion directories. `aggregate_manifest.json` lists the included discussion IDs and aggregate counts.

## File descriptions

| File | Records | Description |
| --- | ---: | --- |
| `01_comment_pair_asr_annotations.json` | 604 | Comment-pair ASR annotations, model-stage results and metadata |
| `02_comment_pair_topic_extraction_records.json` | 306 | Topic Triple extraction and normalization for comment pairs |
| `03_discussion_topic_index.json` | 281 | Discussion-level Topic Triple index and source-comment references |
| `04_topic_causal_relation_judgments.json` | 3,330 | Directed candidate Topic–Topic causal relation judgments |
| `05a_comment_topic_link_judgments.json` | 1,241 | Comment-to-Topic link judgments |
| `05b_comment_causal_relation_link_judgments.json` | 1,304 | Comment-to-causal-relation link judgments |
| `06a_comment_topic_attitudes.json` | 417 | Expressed attitudes toward Topic Triples |
| `06b_comment_causal_relation_attitudes.json` | 270 | Expressed attitudes toward topic causal relations |
| `07_discussion_processing_summary.json` | 25 | Per-discussion processing summaries |

Counts refer to stored JSON records, rather than necessarily to unique comments or retained graph edges.

## ASR annotation records

`01_comment_pair_asr_annotations.json` stores **Argumentative Semantic Relation (ASR)** results for comment pairs. Each record identifies its comment pair using fields such as:

- `pair_id` — identifier for the source comment pair.
- `metadata.discussion_id` — discussion identifier.
- `metadata.parent_id` and `metadata.child_id` — IDs of the parent and child comments.
- `scheme` — ASR scheme label.
- `stage1_v3`, `stage2_v3`, `stage3_v3` — staged annotation and validation outputs, where present.

The structured comment-pair references use **comment IDs rather than dedicated `parent_text` and `child_text` fields**. The ASR files also preserve model-generated explanations and serialized stage outputs under fields such as `raw_outputs`; these may include descriptions or short references to comment content. They are not a standalone reproduction of the source discussion dataset.

## Joining records

- `source_discussion_id` identifies the originating discussion in most downstream files; ASR metadata uses `metadata.discussion_id`.
- `pair_id` relates comment-pair processing records.
- `topic_id` identifies a Topic Triple in `03_discussion_topic_index.json`, where `topic_triple.surface` and `topic_triple.normalized` provide subject–predicate–object representations.
- `relation_id`, `from_topic_id`, and `to_topic_id` identify Topic–Topic candidate relations and their direction.
- `source_comment_id` with `topic_id` or `relation_id` connects comment–target link judgments (05a/05b) and attitude annotations (06a/06b).

When joining results, use the discussion ID alongside comment, topic, pair, and relation identifiers where appropriate.

### Topic causal judgments

In `04_topic_causal_relation_judgments.json`, the `final_causal_status` values distinguish:

- `Explicit` — explicit causal relation.
- `Implicit` — implicit causal relation.
- `Unsupported` — candidate not retained as a causal relation.

The file contains judgments for candidates and should not be interpreted as a list containing only retained relations.

## Exploring the release

Start with `aggregate_manifest.json` for collection-wide information. Use the numbered JSON files for detailed processing outputs, or inspect a `discussion_<id>/` directory for one discussion.

The repository root also contains [`index.html`](../index.html), the entry point to **10 interactive Discussion Knowledge Graph examples**. These visualizations are a subset of the examples presented separately from the collection of 25 data discussions.

For the paper and source-data context, see the [repository README](../README.md).
