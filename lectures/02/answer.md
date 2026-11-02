# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：
- 自分で学習を実行したか（`executed_training_myself`）：はい（実行できた範囲：　セクション0~8）
- 学習前：`format_rate` =　0.095 / `content_rate` =　0.0
- 学習後：`format_rate` =　1.0 / `content_rate` =　0.905
- 退行した質問：なし
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：根拠なしの質問に断定した。FAQに記載がない事柄について、正しくneeds_review: true と判定できず、FAQに書いてある別項目の情報を引用してneeds_review: false としてしまうケースが残った。
- 知識を問う質問の結果（学習前の分類 → 学習後の分類）と気づいたこと：学習前はformat_error が多発していたが、学習後は全ての質問に対して形式を維持できるようになった。ただし、FAQにない知識質問に対して正しくneeds_review: trueと返せるようになったものがある一方で、FAQにないことに対して誤ってneeds_review: falseとしてしまう挙動も見られる。
- 学習で変わったこと・変わらなかったこと：出力の形式は遵守することができたが、ハルシネーションは依然として未解決。

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ：COFIGのnum_train_epochsを10に変更した。
- 結果（比較表や数字）：
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 10,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 21.7
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.8571428571428571,
    "both_rate": 0.8571428571428571,
    "seconds": 45.4
  },
  "regressed": [],
  "executed_training_myself": true
}
- ファインチューニングが向く場面・向かない場面についての考察：特定の出力形式を守らせたいときや特定の分野の用語や知識を学ばせたい時にはファインチューニングは適している。一方で、頻繁に更新される最新情報や高品質なデータを用意できない場合はファインチューニングは向かないと考える。
