# ObG Discourse Tool: Five Completed Discussions with Original ID References

This dataset consists of UTF-8 JSON arrays containing original IDs, generated labels, and structural information, without including ObG comment text or quotations. Random `public_*` IDs are not used.

## ID Conventions

| Field                                                  | Meaning / Reference                                                                                          |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `source_discussion_id`                                 | ObG discussion key. Shared across all files.                                                                 |
| `source_split`                                         | The original ObG split, either Train or Test. This is different from any split created during preprocessing. |
| `source_comment_id`                                    | ObG `CommentID`.                                                                                             |
| `source_parent_comment_id` / `source_child_comment_id` | Original parent and child comment IDs used for ASR and Topic extraction.                                     |
| `source_comment_ids`                                   | List of original comment IDs that provide the evidence for a Topic. The comment text itself is not included. |
| `pair_id`                                              | Parent-child pair ID generated during tool preprocessing. This is not an original ObG field.                 |
| `pair_claim_id`                                        | Extracted Topic ID within a parent-child pair.                                                               |
| `topic_id`                                             | Topic ID after deduplication and reassignment. See `topic_inventory`.                                        |
| `relation_id`                                          | ID assigned to a causal-relation candidate. See `topic_relations`.                                           |
| `from_topic_id` / `to_topic_id`                        | Source and target Topic IDs supplied as input to relation classification.                                    |

Generated IDs must always be interpreted together with `source_discussion_id`. A `pair_claim_id` additionally requires its corresponding `pair_id`.

The `topic_id` field in `topic_extractions` maps each pair-level extraction to its downstream Topic. This mapping has been verified based on consistency in the Triple, polarity, conditions, evidence, and related attributes.

For comment links and attitude annotations, Topic-oriented files use `topic_id` as the target, while relation-oriented files use `relation_id`. Ambiguous `target_id` fields or redundant `target_type` fields are not used.

`from_topic_id` and `to_topic_id` represent the order in which Topics were provided as input. The inferred causal direction should be determined from the `direction` field.

## Joining with ObG

Using the split specified by `source_split`, load `discussions[source_discussion_id].messages` from either `Dataset/Train.json` or `Dataset/Test.json`, and join records by matching `source_comment_id` and related fields against `CommentID`.

For a direct parent-child reply relation, the parent comment ID is given by the child's `responding_to.comment_id`.

The SHA-256 hashes of the original source files are recorded in `manifest.json`. No private ID mapping table is required.

## Files and Record Counts

| File                           | Records |
| ------------------------------ | ------: |
| `discourse_summary.json`       |       5 |
| `asr_annotations.json`         |     120 |
| `topic_extractions.json`       |      47 |
| `topic_inventory.json`         |      46 |
| `topic_relations.json`         |     518 |
| `comment_topic_labels.json`    |     167 |
| `comment_relation_labels.json` |     141 |
| `topic_attitudes.json`         |      62 |
| `relation_attitudes.json`      |      27 |

The dataset contains 35 comments, 46 C2 Topics, and 518 causal-relation candidates, of which 22 were accepted. A total of 25 Topics were selected as linking targets.

Completion of Stages 1–3 has been confirmed for all selected targets. This does **not** mean that attitude labels were assigned to all 46 Topics.

The ASR annotations contain successful Stage 3 results for the five discussions.

The ASR labels are defined as follows:

* `AfSJ`: Supportive Justification
* `AfR`: Rebuttal
* `AfCC`: Causal Consequence
* `AfAC`: Analogy or Comparison

## Scope of Reproducibility

The released data allow verification of correspondence between the original comments and generated labels, parent-child relations, and references to Topics.

However, the dataset does not include Triple or hypothesis text, quotations, reasoning justifications, inference traces, or existing stance annotations from ObG. Therefore, it does not guarantee full reproducibility of proposition content or the complete inference process.

Model and prompt versions are retained as provenance information for the processing pipeline.

The original ObG dataset must be obtained separately.
