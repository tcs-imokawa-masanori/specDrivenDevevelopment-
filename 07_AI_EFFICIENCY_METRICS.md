# AI開発効率メトリクス定義書
# AI Development Efficiency Metrics Definition

**プロジェクト名**: EDION マスタ管理モダナイゼーション PoC
**作成日**: 2025-12-25
**バージョン**: 1.0

---

## 1. 概要 (Overview)

本ドキュメントは、AIを活用した開発における効率性を測定・評価するための指標定義を整理したものです。
「何時間AIで可能か」「待ち時間はどの程度か」を含め、LOC・画面/機能/US/UC・信頼性・最終チェックまで一気通貫で計測可能な指標体系を定義しています。

---

## 2. AIに対しての開発効率（何時間AIで可能、待ち時間）

### 2.1 計測対象の工数（時間）

| 区分 | 定義 | 単位 | 記録方法 |
|------|------|------|----------|
| **AI作業時間（AI-Active）** | AIに投入する作業（プロンプト作成、要件入力、生成結果の指示、差分確認、再生成指示）に人が実際に手を動かしている時間 | h | タイムログ |
| **AI待ち時間（AI-Wait）** | AIが生成/解析/ビルド/テストを実行しており、人が待っている時間（ジョブ待ち・応答待ち・CI待ち含む） | h | タイムログ |
| **人作業時間（Human-Active）** | AIを使わず人が実装/調査/修正/レビューした時間（通常作業） | h | タイムログ |

### 2.2 主要指標（KPI）

| 指標名 | 計算式 | 説明 |
|--------|--------|------|
| **AI適用率（AI Coverage）** | AI-Active / (AI-Active + Human-Active) | AI活用の割合 |
| **待ち時間比率（Wait Ratio）** | AI-Wait / (AI-Active + AI-Wait + Human-Active) | 総時間に占める待ち時間 |
| **総リードタイム（Lead Time）** | AI-Active + AI-Wait + Human-Active | タスク完了までの総時間 |
| **短縮率（Time Saving）** | 1 − (Lead Time_AIあり / Lead Time_Baseline) | AIなしと比較した削減率 |

### 2.3 記録方法（推奨）

- 1タスクごとに「AI-Active / AI-Wait / Human-Active」をタイムログ化
- 記録先：
  - Jira/Wrikeコメント
  - 作業ログシート（Excel/スプレッドシート）
  - GitコミットコメントでもOK

---

## 3. LOCs の作業策定（規模と生産性）

### 3.1 LOC定義

| 区分 | 定義 | 備考 |
|------|------|------|
| **新規追加LOC（Added LOC）** | 新たに追加された行数 | |
| **修正LOC（Modified LOC）** | 既存コードを修正した行数 | |
| **削除LOC（Deleted LOC）** | 削除された行数 | |
| **合計変更LOC（Churn LOC）** | Added + Modified + Deleted | Git差分から算出 |

> ※可能なら「AI生成コード」と「人手修正コード」を区別して計測

### 3.2 生産性指標

| 指標名 | 計算式 | 説明 |
|--------|--------|------|
| **実装生産性（LOC/h）** | Churn LOC / (AI-Active + Human-Active) | 時間あたりの変更行数 |
| **AI生成比率（AI-Generated Ratio）** | AIが初回生成したLOC / Churn LOC | AI生成コードの割合 |

> ※Git差分と生成ログで推定（難しければ「主観判定」でもOK：AI主導/人主導）

---

## 4. 画面、機能、User Stories / Use Case / Grid 等の動作数

### 4.1 カウント定義

| 区分 | 定義 | 例 |
|------|------|-----|
| **画面数（Screens）** | UI単位 | 一覧・詳細・編集・検索など |
| **機能数（Functions）** | API/バッチ/画面機能などの機能単位 | CRUD操作、バッチ処理 |
| **User Story数（US）** | Backlog上のストーリー単位 | Jira/Backlogチケット |
| **Use Case数（UC）** | 業務ユースケース単位 | 業務フロー単位 |
| **Grid動作数（Grid Ops）** | 一覧の動作 | 検索、ソート、フィルタ、ページング、CSV出力、行編集、行削除など |

### 4.2 指標

| 指標名 | 計算式 | 説明 |
|--------|--------|------|
| **機能あたり工数** | Lead Time / Functions | 1機能の開発にかかる時間 |
| **USあたり工数** | Lead Time / US | 1ストーリーの開発にかかる時間 |
| **画面あたり工数** | Lead Time / Screens | 1画面の開発にかかる時間 |
| **Grid動作あたり工数** | Lead Time / Grid Ops | 1Grid動作の開発にかかる時間 |
| **AI適用US率** | AIを主要手段として実装したUS / 全US | AI活用したストーリーの割合 |

> **ポイント**: AI効果は「LOC」よりも「US・機能・画面あたり工数」で示すと経営層に伝わりやすい

---

## 5. 信頼性（Bug発生数、バグ修正率、バグ再発率）

### 5.1 バグ定義

| 項目 | 定義 |
|------|------|
| **バグ** | 要修正としてチケット化された欠陥 |
| **重要度分類** | Critical / High / Medium / Low |
| **発生フェーズ** | 実装中、UT、IT、ST、UAT、運用 など |

### 5.2 品質指標

| 指標名 | 計算式 | 説明 |
|--------|--------|------|
| **バグ発生数（Bug Count）** | 期間/リリース単位でカウント | 発生したバグの総数 |
| **バグ密度（Bug Density）** | Bug Count / (Churn LOC / 1000) | KLOCあたりのバグ数 |
| **機能あたりバグ密度** | Bug Count / Functions | 機能あたりのバグ数 |
| **バグ修正率（Fix Rate）** | 修正完了数 / 発生数（同一期間） | バグ解消の割合 |
| **平均修正時間（MTTR）** | バグ修正にかかった時間の平均 | 修正効率 |
| **バグ再発率（Regression Rate）** | 再オープン数 / 修正完了数 | 同一原因の再発を別カウントで管理も可 |

### 5.3 AI利用時の観点（分析項目）

| 観点 | 詳細 |
|------|------|
| **AI生成コードのバグ率** | AI生成 vs 人実装のバグ率比較 |
| **AIが原因のバグ分類** | 仕様解釈ミス、境界条件漏れ、例外処理不足、セキュリティ考慮不足 など |

---

## 6. 最終チェック（人）／Error発生率

### 6.1 最終チェックの定義

| 項目 | 定義 |
|------|------|
| **最終チェック** | リリース前に人が行うチェック（レビュー/テスト/仕様確認）の合計 |
| **チェック観点** | 仕様逸脱、UI崩れ、例外、性能、セキュリティ、ログ、監査 など |

### 6.2 指標

| 指標名 | 計算式 | 説明 |
|--------|--------|------|
| **最終チェック工数（Final Human Check Time）** | 実測時間 (h) | 人による最終確認の工数 |
| **最終チェック指摘数（Findings）** | カウント (件) | 発見された問題の数 |
| **Error発生率（Final Error Rate）** | 指摘数 / 対象機能数（または US数、画面数） | 単位あたりの問題発生率 |
| **最終チェック通過率（Pass Rate）** | 一発OK件数 / チェック対象件数 | 修正なしで通過した割合 |
| **重大欠陥率（Critical Finding Rate）** | 重大指摘数 / 全指摘数 | 指摘の深刻度 |

---

## 7. サマリー表示フォーマット（1枚に落とす時の見せ方）

### 7.1 経営層向けダッシュボード例

```
┌─────────────────────────────────────────────────────────────────┐
│                    AI開発効率サマリー                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  【生産性】                                                      │
│  ・Lead Time: Baseline 40h → AI活用 22h（▲45%）                 │
│  ・AI-Active 8h / AI-Wait 2h / Human-Active 12h                 │
│  ・AI適用率: 40%                                                 │
│                                                                 │
│  【規模】                                                        │
│  ・Churn LOC: 1,800（AI生成 1,200 / 人修正 600）                 │
│  ・実装生産性: 90 LOC/h                                          │
│                                                                 │
│  【量】                                                          │
│  ・Screens 4 / Functions 9 / US 6 / Grid Ops 12                 │
│  ・機能あたり工数: 2.4h / 画面あたり工数: 5.5h                    │
│                                                                 │
│  【品質】                                                        │
│  ・Bug 6（KLOCあたり 3.3）                                       │
│  ・Fix Rate 100% / Regression 0%                                │
│  ・Final Check Findings 5（重大0）→ Error Rate 0.56/Function    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 7.2 数値サマリーテーブル

| カテゴリ | 指標 | Baseline | AI活用 | 削減率 |
|----------|------|----------|--------|--------|
| **生産性** | Lead Time | 40h | 22h | ▲45% |
| | AI-Active | - | 8h | - |
| | AI-Wait | - | 2h | - |
| | Human-Active | 40h | 12h | ▲70% |
| **規模** | Churn LOC | 1,800 | 1,800 | - |
| | AI生成LOC | - | 1,200 | 67% |
| | 実装生産性 | 45 LOC/h | 90 LOC/h | +100% |
| **量** | Screens | 4 | 4 | - |
| | Functions | 9 | 9 | - |
| | US | 6 | 6 | - |
| | Grid Ops | 12 | 12 | - |
| **品質** | Bug Count | 8 | 6 | ▲25% |
| | Bug Density | 4.4/KLOC | 3.3/KLOC | ▲25% |
| | Fix Rate | 95% | 100% | +5% |
| | Regression Rate | 5% | 0% | ▲100% |

---

## 8. 計測ツール・記録方法

### 8.1 推奨ツール

| 用途 | ツール | 備考 |
|------|--------|------|
| 時間計測 | Toggl Track, Clockify, タイムシート | AI/Wait/Human を区別 |
| タスク管理 | Jira, Backlog, Wrike | US/UCを管理 |
| コード計測 | Git, GitHub Insights, SonarQube | LOC/コミット分析 |
| 品質管理 | Jira, TestRail | バグ追跡 |

### 8.2 記録テンプレート

```markdown
## タスク記録: [タスク名]

### 時間記録
- AI-Active: X.X h（プロンプト作成、結果確認）
- AI-Wait: X.X h（生成待ち、ビルド待ち）
- Human-Active: X.X h（手動実装、レビュー）
- Total Lead Time: X.X h

### 成果物
- LOC: Added XXX / Modified XXX / Deleted XXX
- Screens: X
- Functions: X
- Grid Ops: X

### 品質
- Bugs Found: X (Critical: X, High: X, Medium: X, Low: X)
- Final Check Findings: X
```

---

## 9. プロジェクト別適用ガイド

### 9.1 Edion PoC 向け設定

| 項目 | 設定値 |
|------|--------|
| 計測単位 | 画面/機能/US |
| Baseline | 従来手法の見積もり値 |
| 品質目標 | Bug Density < 5/KLOC |
| AI適用率目標 | 60%以上 |

### 9.2 その他プロジェクト向け

- **QES検索**: Grid Ops 中心に計測
- **フィルターB-F**: 機能単位で計測
- **Step Z**: US単位で計測

---

## 10. 付録：用語集

| 用語 | 英語 | 定義 |
|------|------|------|
| AI-Active | AI Active Time | AIを使った作業時間 |
| AI-Wait | AI Wait Time | AIの応答待ち時間 |
| Human-Active | Human Active Time | 人による作業時間 |
| Lead Time | Lead Time | タスク開始から完了までの時間 |
| Churn LOC | Churn Lines of Code | 変更行数（追加+修正+削除） |
| KLOC | Kilo Lines of Code | 1000行単位のコード量 |
| Bug Density | Bug Density | KLOCあたりのバグ数 |
| MTTR | Mean Time To Repair | 平均修復時間 |
| Fix Rate | Fix Rate | バグ修正完了率 |
| Regression Rate | Regression Rate | バグ再発率 |

---

*Document Version: 1.0*
*Created: 2025-12-25*
*Project: EDION Master Management Modernization PoC*
