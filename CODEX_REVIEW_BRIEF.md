# Codex Desktop App レビュー Brief — LSFP セミナーLP

**対象 commit**: `lsfp-ai/lsfp-seminar-lp` main HEAD（commit `fa9fbab` 時点 / 1 file / 1091行）
**公開URL**: https://lsfp-ai.github.io/lsfp-seminar-lp/
**ファイル**: `index.html`（27,745 bytes / 1ファイル単一HTML / CSS埋込）
**配信媒体**: LINE公式アカウントのリッチメニュー（6マス中4マス：#step1/#step2/#step3/#cta）からスマホ閲覧
**配信対象**: 50–65歳の老後資金設計に関心がある一般個人

---

## 0. 禁止事項

- **このリポジトリには build/test runtime が存在しない**。CI実行・npm install・vitest 等は走らせない。
- **HTMLの章立て・主要キャッチコピー（"じぶん条件"/"使えるお金"/"専門家不在の分野"）は加藤さん固有の商標的概念**。リライト提案は禁止。表記揺れ・誤字・法令違反・marketing用語違反のみ指摘。
- 数値の置き換え提案は禁止。**計算ロジックが疑わしい箇所は「数値検算が必要」と指摘するに留める**。
- 「クール」「ミニマル」「Pro風」等の感性的リライト指示は禁止。具体的なファクト不整合・規則違反のみ報告。

---

## 1. 目的

LINE公式リッチメニュー経由でスマホから閲覧される販促LPの **送付可否判定**。
- GREEN = 「このまま LINE 友だちに公開してよい」
- YELLOW = critical/major 修正必須、minor は記録のみ
- RED = 重大事故リスクあり、公開停止すべき

---

## 2. 制約・前提

- 単一HTMLファイル、JavaScript なし、外部CDN はGoogle Fonts のみ。
- ホスティングは GitHub Pages（main / root 配信、HTTPS強制）。
- スマホ縦表示が主用途。`@media (max-width: 600px)` でレスポンシブ実装済。
- 予約導線は line 1057 の Google Calendar URL（`https://calendar.app.google/MBWMxKdFUuyaCqs49`）のみ。

---

## 3. レビュー対象（8層レビュー計画）

### Layer 1: HEAD差分確認
- `git log --oneline` で commit が `fa9fbab` 一本のみであることを確認
- `git diff --stat` で `index.html` 1ファイルのみであることを確認

### Layer 2: スコープ整合性
- LP の章立て（Hero → Intro → Step1 → Step2 → Step3 → CTA → Footer）が、リッチメニュー4マス（#step1/#step2/#step3/#cta）と整合しているか
- 各アンカーIDが実HTMLに存在するか（`id="step1"` 等）

### Layer 3: 計算核（comparison テーブルの数値検算）
**line 891-913 の比較表**を以下で検算：
- 年金額：1,368,000（60歳繰上げ24%カット = 1,800,000 × 0.76 = 1,368,000 ✓）
- 可処分所得：年金額 - 税・社保 の妥当性（仙台市・令和7年度料率と本文記載）
- 新NISA収益 468,000 が本文 line 917「NISA残高約792万円」と整合するか（792万 × 6% = 475,200 で乖離あり / 468,000 / 6% = 780万円）
- 「使えるお金」= 可処分所得 + 新NISA収益 の足し算検算
- 結論：A 1,671,600 > B 1,622,600（差 49,000円）が「Aが有利」と本文で言える妥当な差か

### Layer 4: 境界値・前提の明示
- 「仙台市・令和7年度料率」と書かれているが、加藤さんは岐阜が地元。**仙台市である理由がLP本文にない**（試算条件として明示すべきか確認）
- 年利6% という運用前提が table 内に明記されていない（line 917 で本文に書かれる）
- 「A の NISA積立元本」（年金1,368,000全額 or 可処分1,203,600 のどちら）が不明

### Layer 5: UI（スマホ縦表示）
- `@media (max-width: 600px)` 配下のフォントサイズ・余白
- comparison-grid（1.4fr 1fr 1fr）がスマホ幅で破綻しないか
- cta-button / cta-button-large のタップ領域（最小44pxを満たすか）
- sticky header + sticky CTA 衝突がないか

### Layer 6: UX
- ハッシュアンカー（#step1/#step2/#step3/#cta）スクロール時、sticky header に隠れる問題（`scroll-margin-top` 設定なし）の有無
- Google Calendar リンク（line 1057）が `target="_blank" rel="noopener noreferrer"` ✓
- CTAボタン2箇所（hero と cta セクション）の文言整合性

### Layer 7: 実務リスク・marketing 規則違反 ★最重要★

以下はLSFP内部ルール（MEMORY 由来）：

- **【規則A】外向け販促物では「IFA」と書かない、「金融アドバイザー」を使う**（feedback_marketing_term_advisor）
  - **検出ポイント**: line 1077 `r-Laboratory株式会社 所属 IFA`
- **【規則B】r-Laboratory の代表は「会長：上地明徳」**（reference_r_laboratory）。本LPには社名のみ言及で役員名なし → 影響なし、念のため確認
- **【規則C】加藤博の表記**: 外部文書では「加藤 博（かとう ひろし）」「Kato Hiroshi」が正規（user_name）→ line 1065-1066 ✓
- **【規則D】文字校正**: line 985 `老娯資金最大化プランナー` の「娯」は「後」の誤字疑い、もしくは加藤さん固有概念の意図的表記か要確認
- **【規則E】資格表記の正確性**: line 1068-1072 の資格名称・登録名が正式名称か（特に「家族信託コーディネーター」「一種外務員」）

その他：
- 「無料・45分・ZOOM」と「初回のみ無料/2回目以降有料」の整合性（金商法・景表法上「無料」と書いて条件付きであることが本文に明記されているか）
- 「年利6%想定」が本文 line 917 にあるが、断り書き（特定運用商品を勧めるものではない／実績保証ではない旨）の有無
- Footer 著作権「© 2026」だが現在2026年5月なので問題なし

### Layer 8: 公開URL・記録
- 公開URL `https://lsfp-ai.github.io/lsfp-seminar-lp/` HTTP 200 ✓（deploy時確認済）
- 4アンカーID実在 ✓（deploy時 grep確認済）
- OGP/Twitter Card メタタグの有無（LINE/SNS共有時のサムネイル表示用）→ 残タスク#5 に記載済、本レビューでは「未設定」と記録するのみ
- canonical URL の有無

---

## 4. 検証手順

1. 上記 Layer 1–8 を順に独立確認。
2. 計算核（Layer 3）は **手計算で検算**。電卓レベルでOK。
3. 規則違反（Layer 7）は **grep で物理的に該当行を特定**してから報告。
4. スマホUI（Layer 5）はChrome DevTools の Device Mode で 375px / 414px / 600px の3断面確認。

---

## 5. 報告フォーマット

```
## Codex Review Report — lsfp-seminar-lp

### 総合判定
GREEN / YELLOW / RED  ※どれか1つ

### Critical（公開前に必ず修正）
- [layer X] 該当行: lineYYY / 内容: ... / 推奨対応: ...

### Major（公開前に修正推奨）
- ...

### Minor（記録のみ、後日対応可）
- ...

### Nit（任意改善）
- ...

### 対象外と判定したLayer
- Layer X: 理由 ...

### 検算結果サマリ（Layer 3 計算核）
| 項目 | LP掲載値 | Codex検算値 | 判定 |
|---|---|---|---|
| ... | ... | ... | ✓/✗ |

### 規則違反一覧（Layer 7）
| 規則 | 該当行 | 違反内容 | 推奨表記 |
|---|---|---|---|
| 規則A (IFA→金融アドバイザー) | line 1077 | ... | ... |
```

---

## 6. 完了条件

- 上記フォーマットで report 提出
- Critical/Major があれば修正diff提示（コードブロックでOK、PR作成は不要 → 加藤さん→Claude Code 経由で反映）
- GREEN判定なら「LINE公式公開可」を明記
