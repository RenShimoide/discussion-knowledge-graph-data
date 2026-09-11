# ObG談話ツール：完了5談話・元ID参照版（形式3）

ObG本文・引用を含めず、元IDと生成ラベル・構造を収録したUTF-8 JSON配列です。ランダムなpublic_* IDは使用しません。

## ID規則

|項目|意味・参照先|
|---|---|
|source_discussion_id|ObGの談話キー。全ファイル共通|
|source_split|ObG原本のTrainまたはTest。前処理後splitとは異なる|
|source_comment_id|ObGのCommentID|
|source_parent_comment_id / source_child_comment_id|ASR・Topic抽出の元親子コメント|
|source_comment_ids|Topicの根拠となった元コメントID一覧。引用本文は含めない|
|pair_id|ツール前処理で生成した親子ペアID。ObGの元項目ではない|
|pair_claim_id|親子ペア内の抽出Topic ID|
|topic_id|重複除去・再採番後のTopic ID。topic_inventoryを参照|
|relation_id|因果候補判定のID。topic_relationsを参照|
|from_topic_id / to_topic_id|関係判定に入力した始点・終点Topic ID|

生成IDは必ずsource_discussion_idと組み合わせて参照します。pair_claim_idはさらにpair_idとの組み合わせが必要です。topic_extractionsのtopic_idでペア別抽出を後段Topicへ対応付けます。この対応はTriple・極性・条件・根拠等の一致で検証済みです。

コメントリンク・態度の対象は、Topic向けファイルならtopic_id、関係向けならrelation_idです。曖昧なtarget_idや重複するtarget_typeは置きません。from_topic_id/to_topic_idは入力順を表し、推定された因果の向きはdirectionを参照します。

## ObGとの結合

source_splitで指定したDataset/Train.jsonまたはTest.jsonからdiscussions[source_discussion_id].messagesを読み、CommentIDでsource_comment_id等を結合します。親子の直接の返信先は子のresponding_to.comment_idです。manifest.jsonに原本のSHA-256を記録しています。privateのID対応表は不要です。

## ファイルと件数

|ファイル|件数|
|---|---:|
|discourse_summary.json|5|
|asr_annotations.json|120|
|topic_extractions.json|47|
|topic_inventory.json|46|
|topic_relations.json|518|
|comment_topic_labels.json|167|
|comment_relation_labels.json|141|
|topic_attitudes.json|62|
|relation_attitudes.json|27|

35コメント、46 C2 Topic、518因果候補のうち採択22、リンク対象25 Topic。各選定対象のStage 1〜3完了を確認済みです。全46 Topicに態度を付与したという意味ではありません。ASRは5談話の成功Stage 3を収録します。

AfSJ=Supportive Justification、AfR=Rebuttal、AfCC=Causal Consequence、AfAC=Analogy or Comparison。

## 再現性の範囲

原文とラベルの照合、親子関係、Topicへの参照を検証できます。Triple・仮説の文言、引用、理由、推論履歴、ObGの既存stance等は含めないため、命題内容や推論全体の再現を保証しません。モデル・prompt versionを処理の来歴として残しています。元データは別途取得してください。
