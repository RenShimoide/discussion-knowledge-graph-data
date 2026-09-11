# ObG Discourse Tool: Discussions Data with Original ID References

This dataset consists of UTF-8 JSON arrays containing original IDs, generated labels, and structural information, without including ObG comment text or quotations[cite: 5]. Random `public_*` IDs are not used[cite: 5].

## ID Conventions

| Field                                                  | Meaning / Reference                                                                                          |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `source_discussion_id`                                 | ObG discussion key. Shared across all files[cite: 5].                                                                 |
| `source_split`                                         | The original ObG split, either Train or Test. This is different from any split created during preprocessing[cite: 5]. |
| `source_comment_id`                                    | ObG `CommentID`[cite: 5].                                                                                             |
| `source_parent_comment_id` / `source_child_comment_id` | Original parent and child comment IDs used for ASR and Topic extraction[cite: 5].                                     |
| `source_comment_ids`                                   | List of original comment IDs that provide the evidence for a Topic. The comment text itself is not included[cite: 5]. |
| `pair_id`                                              | Parent-child pair ID generated during tool preprocessing. This is not an original ObG field[cite: 5].                 |
| `pair_claim_id`                                        | Extracted Topic ID within a parent-child pair[cite: 5].                                                               |
| `topic_id`                                             | Topic ID after deduplication and reassignment. See `03_discussion_topic_index.json`[cite: 3, 4, 5].                                        |
| `relation_id`                                          | ID assigned to a causal-relation candidate. See `04_topic_causal_relation_judgments.json`[cite: 3, 4, 5].                                           |
| `from_topic_id` / `to_topic_id`                        | Source and target Topic IDs supplied as input to relation classification[cite: 5].                                    |

Generated IDs must always be interpreted together with `source_discussion_id`[cite: 5]. A `pair_claim_id` additionally requires its corresponding `pair_id`[cite: 5].

The `topic_id` field in `02_comment_pair_topic_extraction_records.json` maps each pair-level extraction to its downstream Topic[cite: 3, 4, 5]. This mapping has been verified based on consistency in the Triple, polarity, conditions, evidence, and related attributes[cite: 5].

For comment links and attitude annotations, Topic-oriented files use `topic_id` as the target, while relation-oriented files use `relation_id`[cite: 5]. Ambiguous `target_id` fields or redundant `target_type` fields are not used[cite: 5].

`from_topic_id` and `to_topic_id` represent the order in which Topics were provided as input[cite: 5]. The inferred causal direction should be determined from the `direction` field[cite: 5].

## Joining with ObG

Using the split specified by `source_split`, load `discussions[source_discussion_id].messages` from either `Dataset/Train.json` or `Dataset/Test.json`, and join records by matching `source_comment_id` and related fields against `CommentID`[cite: 5].

For a direct parent-child reply relation, the parent comment ID is given by the child's `responding_to.comment_id`[cite: 5].

The SHA-256 hashes of the original source files are recorded in `manifest.json`[cite: 5]. No private ID mapping table is required[cite: 5].

## Files and Record Counts

| File                                                  | Records |
| ----------------------------------------------------- | ------: |
| `07_discussion_processing_summary.json`               |       5 |
| `01_comment_pair_asr_annotations.json`                |     120 |
| `02_comment_pair_topic_extraction_records.json`       |      47 |
| `03_discussion_topic_index.json`                      |      46 |
| `04_topic_causal_relation_judgments.json`             |     518 |
| `05a_comment_topic_link_judgments.json`               |     167 |
| `05b_comment_causal_relation_link_judgments.json`      |     141 |
| `06a_comment_topic_attitudes.json`                    |      62 |
| `06b_comment_causal_relation_attitudes.json`          |      27 |

The dataset contains 35 comments, 46 C2 Topics, and 518 causal-relation candidates, of which 22 were accepted[cite: 5]. A total of 25 Topics were selected as linking targets[cite: 5].

Completion of Stages 1–3 has been confirmed for all selected targets[cite: 5]. This does **not** mean that attitude labels were assigned to all 46 Topics[cite: 5].

The ASR annotations contain successful Stage 3 results for the five discussions[cite: 5].

The ASR labels are defined as follows[cite: 5]:

* `AfSJ`: Supportive Justification[cite: 5]
* `AfR`: Rebuttal[cite: 5]
* `AfCC`: Causal Consequence[cite: 5]
* `AfAC`: Analogy or Comparison[cite: 5]

## Scope of Reproducibility

The released data allow verification of correspondence between the original comments and generated labels, parent-child relations, and references to Topics[cite: 5].

However, the dataset does not include Triple or hypothesis text, quotations, reasoning justifications, inference traces, or existing stance annotations from ObG[cite: 5]. Therefore, it does not guarantee full reproducibility of proposition content or the complete inference process[cite: 5].

Model and prompt versions are retained as provenance information for the processing pipeline[cite: 5].

The original ObG dataset must be obtained separately[cite: 5].