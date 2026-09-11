# From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution

This repository contains dataset extensions, prompt specifications, and pipeline configurations for the paper **"From Comments to Discussion Knowledge Graphs: Modeling Context Dependent Opinions and Topic Evolution"** accepted at **NLP4KGC 2026**.

---

## 📢 Dataset Notice

* **Original Dataset**:  
  This repository does **NOT** redistribute the full Old but Gold (ObG) dataset. For the original dataset and licensing details, please visit the official distribution site of the ObG dataset.

* **Included Discussions**:  
  This repository includes extracted topic triples, Argumentation Scheme Relations (ASRs), and cross-layer annotations for **5 discussion chains** analyzed in our study.

---

## 🛠 Visualization Tool

* **Release Schedule**:  
  The interactive visualization tool for exploring the constructed Discussion Knowledge Graphs will be made available by the **first day of the NLP4KGC 2026 workshop (October 25, 2026)**.

---

## 🔄 Data Pipeline & File Structure

The graph construction follows a sequential multi-stage extraction pipeline. Following Step 4, Steps **5A/6A** (topic targets) and **5B/6B** (relation targets) branch into two parallel tracks.

> **Note for Reviewers**: Although `discourse_summary.json` is generated last during execution (Step 7), inspecting it first provides a helpful high-level summary of overall counts and completion statuses.

```text
[1] asr_annotations.json
       │
[2] topic_extractions.json
       │
[3] topic_inventory.json
       │
[4] topic_relations.json
       ├─────────────────────────────────┐
       ▼                                 ▼
[5A] comment_topic_labels.json     [5B] comment_relation_labels.json
       │                                 │
[6A] topic_attitudes.json          [6B] relation_attitudes.json
       └─────────────────────────────────┘
                       │
                       ▼
            [7] discourse_summary.json
