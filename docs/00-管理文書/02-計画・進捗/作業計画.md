# 整理ロードマップ v0.3

**as-of:** 2026-09-08

## Phase 0 — 総体設計確定【現在】

個別サブシステム詳細化より先に、以下を確定する。

1. 3軸分類: サブシステム / 取引種別 / 商品
2. 39業務SS + 6共通SSの過不足
3. サブシステム責務境界
4. Internal Relation Map
5. External Relation Map
6. Data Authority Map
7. End-to-End Flow
8. Coverage Matrix
9. INPUT/OUTPUT/帳票標準
10. Architecture Review / Gap Close

### Phase 0 Authority Documents

- `OVERALL_DESIGN.md`
- `SYSTEM_LANDSCAPE.md`
- `CLASSIFICATION_MODEL.md`
- `SUBSYSTEM_CATALOG.md`
- `SUBSYSTEM_RELATION_MAP.md`
- `EXTERNAL_RELATION_MAP.md`
- `DATA_AUTHORITY_MAP.md`
- `END_TO_END_FLOW.md`
- `TRANSACTION_CATALOG.md`
- `PRODUCT_CATALOG.md`
- `COVERAGE_MATRIX.md`

### Architecture Gate

Phase 1へ進む条件:

- [ ] SS/CS過不足Review完了
- [ ] 責務重複/空白なし
- [ ] 主要内部I/FのSource/Target/Object明確
- [ ] 主要外部主体との関係明確
- [ ] 主要Data Authority明確
- [ ] E2E-01〜13で重大Gapなし
- [ ] Coverage Matrixで商品/取引による機能Gapなし

---

## Phase 1 — 国内株式・現物取引を基準シナリオとして中核SSを詳細化

Architecture Gate通過後に開始する。

優先対象:

- SS01 顧客属性
- SS02 口座
- SS03 契約・サービス
- SS05 銘柄
- SS06 市場・営業日
- SS08 制度・パラメータ
- SS09 注文・約定
- SS10 余力
- SS14 手数料・諸経費
- SS16 顧客勘定
- SS17 資金残高・入出金
- SS18 証券残高
- SS20 評価・損益
- SS21 清算
- SS22 受渡・決済
- SS23 保振加入者情報・振替口座
- SS24 照合・例外
- SS25 権利
- SS26 譲渡益税
- SS27 配当・利金税
- SS28 NISA
- SS31 会計
- SS32 対客帳票
- SS33 法定帳簿・報告
- SS34 Compliance
- CS01 外部接続
- CS02 業務日付・Batch

---

## Phase 2 — 信用取引横断Coverage

信用取引を独立サブシステムとはせず、TR02として各SSへRuleを追加する。

重点:

- SS09 注文・約定
- SS10 余力
- SS12 建玉
- SS13 担保・保証金
- SS14 手数料・諸経費
- SS15 与信・取引Risk
- SS16/17/18 勘定・Cash・証券残高
- SS21/22 清算・決済
- SS25 権利
- SS26 税
- SS31 会計
- SS32 帳票

---

## Phase 3 — 商品Coverage拡張

商品を新サブシステム化せず、PRxxごとに既存SSのRuleを追加する。

- 国内債券
- 投資信託
- ETF/ETN/REIT
- 先物/Option
- CB/新株予約権等

---

## Phase 4 — 外国証券・外貨

- SS29 外国証券業務
- SS30 外貨・為替
- 海外決済/Custody/SWIFT/外国税/Corporate Actionを既存SSとの関係で詳細化

---

## Phase 5 — 統制・経営・運用

- SS34 Compliance
- SS35 AML/経済制裁
- SS36 顧客資産分別
- SS37 情報系
- SS38 事務Workflow
- SS39 資金繰り
- CS03-CS06 共通基盤

---

## Definition of Done（各Skill）

- L2以上
- 主要Ruleに一次出典
- INPUT/OUTPUTが項目レベル
- 対内/対外関係
- Mermaid図
- 状態/計算/主要例外
- 帳票がある場合は論理Layout・項目・出力契機
- Batch/Cutoff
- 会計/税/権利影響
- テスト観点
