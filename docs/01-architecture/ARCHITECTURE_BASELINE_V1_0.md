# Architecture Baseline v1.0

**Status:** FROZEN  
**Freeze Date:** 2026-09-08  
**Target:** 日本の総合証券会社向け基幹/バックオフィス（NRI THE STARクラスをBenchmarkとする独自参照モデル）

## 1. Freeze結論

Phase 0のArchitecture Reviewを完了し、以下をv1.0 BaselineとしてFreezeする。

```text
Business Subsystems : SS01-SS42 = 42
Common Subsystems   : CS01-CS06 = 6
Transactions        : TR01-TR11 = 11
Products            : PR01-PR11 = 11
```

業務要件の基本座標:

`Subsystem × Transaction × Product`

例:

`SS26 譲渡益税 × TR01 現物売却 × PR01 国内株式`

---

## 2. Freezeした業務サブシステム

### Customer / Account
- SS01 顧客属性管理
- SS02 口座管理
- SS03 契約・サービス管理
- SS04 営業組織・担当者・チャネル管理

### Reference / Master
- SS05 銘柄管理
- SS06 市場・営業日管理
- SS07 時価・為替レート・基準価額管理
- SS08 制度・料率・業務パラメータ管理
- SS41 取引先・決済条件（SSI）管理

### Trading / Risk
- SS09 注文・約定管理
- SS10 余力管理
- SS11 募集・売出・配分管理
- SS12 建玉管理
- SS13 担保・保証金管理
- SS14 手数料・諸経費管理
- SS15 与信・取引リスク管理

### Customer Books / Assets
- SS16 顧客勘定管理
- SS17 資金残高・入出金管理
- SS18 証券預り・残高管理
- SS19 入出庫・移管管理
- SS20 評価・損益管理

### Clearing / Settlement / Matching
- SS21 清算管理
- SS22 受渡・決済管理
- SS23 保振加入者情報・振替口座管理
- SS24 残高・資金照合／例外・Fail管理
- SS40 約定照合・決済照合管理

### Corporate Action / Tax
- SS25 権利管理
- SS26 譲渡益税・特定口座
- SS27 配当・利金税
- SS28 NISA・非課税口座

### Foreign Securities
- SS29 外国証券業務管理（外証）
- SS30 外貨・為替管理

### Accounting / Output / Control
- SS31 会計
- SS32 対客帳票・電子交付
- SS33 法定帳簿・当局/税務報告
- SS34 コンプライアンス・売買審査
- SS35 AML・経済制裁
- SS36 顧客資産分別管理
- SS37 情報系・営業日報・経営情報
- SS38 事務ワークフロー・承認
- SS42 交付書面・目論見書・同意管理

### Treasury
- SS39 資金繰り・決済資金管理

---

## 3. Freezeした共通系

- CS01 外部接続
- CS02 業務日付・バッチ統制
- CS03 認証・権限管理
- CS04 監査証跡・操作履歴
- CS05 運用監視・Recovery・再処理
- CS06 データ連携・配信

---

## 4. Reviewで閉鎖した主要Gap

### GAP-01 商品/取引/システム混在

解決: 3軸へ完全分離。

### GAP-02 MatchingとReconciliation混在

解決:
- SS40 = 約定/決済Matching
- SS24 = 残高/資金Reconciliation、Exception、Fail

### GAP-03 Counterparty/SSI Authority欠落

解決: SS41新設。

### GAP-04 取引前文書/目論見書/Consent Authority欠落

解決: SS42新設。

### GAP-05 Conditional業務Coverage

信用、先物、Option、貸借、外国証券、募集、機関投資家MatchingについてE2E Validationを実施し、新SS追加不要と判定。

---

## 5. Freezeした重要Boundary

| A | B | Boundary |
|---|---|---|
| SS09 Trade | SS40 Match | 自社Trade事実 vs 相手とのMatching状態 |
| SS40 Match | SS24 Reconciliation | Instruction Matching vs 残高/資金Break/Fail |
| SS41 SSI | SS22 Settlement | Reference条件 vs 個別Settlement Instruction |
| SS16 顧客勘定 | SS17 Cash | 債権債務 vs 実Cash Balance/Movement |
| SS12 建玉 | SS18 証券残高 | Open Position vs Custody/Holding |
| SS20 評価簿価 | SS26 税取得価額 | 評価/管理目的 vs 税務目的 |
| SS25 権利 | SS26/27 税 | Gross Entitlement/Event vs Tax |
| SS32 対客帳票 | SS42 交付書面 | Post-trade/Periodic Report vs Pre-trade/Contract Document |
| SS32 対客帳票 | SS33 法定報告 | Customer Delivery vs Authority/Tax Submission |
| SS34 Compliance | SS35 AML | Trading/Conduct Control vs AML/Sanctions |
| SS24 Exception | SS38 Workflow | Business Break Case vs Work/Approval Task |
| SS17 Customer Cash | SS39 Firm Funding | Customer Asset vs Firm Liquidity |
| CS01 External | CS06 Internal | External Transport vs Internal Delivery |

---

## 6. Scope Freeze

### Tier A Core

THE STAR級日本リテール総合証券Back-officeの基本責務。34業務SS + 6共通SS。

### Tier B Conditional Core

- SS11 募集・売出・配分
- SS12 建玉
- SS13 担保・保証金
- SS15 与信・取引Risk
- SS29 外国証券
- SS30 外貨・為替
- SS39 資金繰り
- SS40 約定/決済照合

### Tier C Adjacent

- 全社GL/連結会計
- 自己資本規制/Enterprise Risk
- CRM/営業提案
- Web/スマホ/営業店Front
- ECM/DCM等Investment Banking
- Wrap/投資一任Engine
- 全社Document Archive
- 人事/給与

Tier Cは重要度が低いという意味ではなく、本Repositoryの証券取引基幹Bookの外側にAuthorityを置くという意味。

---

## 7. External Infrastructure Coverage

v1.0で明示的にCoverageする主要外部主体:

- JPX / PTS
- JSCC
- JASDEC振替制度
- JASDEC決済照合
- 銀行 / 決済銀行 / 日銀関連
- 日本証券金融等
- 発行体 / 株主名簿管理人 / 信託銀行
- 国税庁 / 税務署 / e-Tax
- 金融庁 / SESC / 日本証券業協会
- J-IRISS
- 情報Vendor
- 他証券会社 / Counterparty
- 投信委託会社 / Fund関連
- 海外市場 / Global Custodian / SWIFT

---

## 8. Architecture Authority Documents

1. `CLASSIFICATION_MODEL.md`
2. `SUBSYSTEM_CATALOG.md`
3. `SYSTEM_LANDSCAPE.md`
4. `OVERALL_DESIGN.md`
5. `SUBSYSTEM_RELATION_MAP.md`
6. `DATA_AUTHORITY_MAP.md`
7. `EXTERNAL_RELATION_MAP.md`
8. `END_TO_END_FLOW.md`
9. `SCOPE_TIER_MODEL.md`
10. `TRANSACTION_CATALOG.md`
11. `PRODUCT_CATALOG.md`
12. `COVERAGE_MATRIX.md`
13. `BOUNDARY_REVIEW_V0_4.md`
14. `CONDITIONAL_E2E_VALIDATION.md`
15. `ARCHITECTURE_REVIEW_CHECKLIST.md`

---

## 9. Change Control

v1.0以後、次はArchitecture Changeとして扱う。

- SS/CS追加・削除
- SS/CS名称変更
- Primary Authority変更
- Tier A/B/C変更
- TR/PR追加・削除
- 重要Boundary変更

詳細は `../00-governance/CHANGE_POLICY.md`。

---

## 10. 次Phase

Architecture v1.0 FreezeによりPhase 0を閉じる。

**Phase 1:** 国内株式現物を基準シナリオとして、Tier A中核サブシステムをL2/L3 Skillへ詳細化する。

各Skillでは必ず以下を整理する。

- 具体Business Know-how
- Business Rule
- State/Lifecycle
- Calculation
- INPUT/OUTPUT（項目レベル）
- Internal/External Interface
- Batch/Cutoff
- Error/Correction
- Accounting/Tax/Corporate Action影響
- 帳票Layout
- Test Viewpoint
- 一次出典
