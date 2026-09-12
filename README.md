# From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution

This repository contains dataset extensions, prompt specifications, and pipeline configurations for the paper **"From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution"** accepted at **NLP4KGC 2026**.

---

## Dataset Notice

* **Original Dataset**:  
  This repository does **NOT** redistribute the full Old but Gold (ObG) dataset. To reconstruct the full text and features, please obtain the original dataset from the official ObG distribution site.

* **Included Scope**:  
  This repository provides Knowledge Graph annotations and extracted structures for **5 discussion chains** analyzed in the paper.

---

## Visualization Tool

* **Release Schedule**:  
  The interactive visualization tool for exploring the constructed Discussion Knowledge Graphs will be made available by the **first day of the NLP4KGC 2026 workshop (October 25, 2026)**.

---

## Data Pipeline & File Structure

`data/03_discussion_topic_index.json` contains the existing discussion-level Topic IDs, provenance, and both the extracted surface and final normalized subject–predicate–object representations. The stored pipeline output is preserved verbatim, including seven empty normalized object values.

The graph construction follows a sequential multi-stage extraction pipeline. Following Step 4, Steps **5a/6a** (topic targets) and **5b/6b** (relation targets) branch into two parallel tracks.

> **Note for Reviewers**: Although `07_discussion_processing_summary.json` is generated last during execution (Step 7), inspecting it first provides a helpful high-level summary of overall metrics and completion statuses across all discussions.

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
