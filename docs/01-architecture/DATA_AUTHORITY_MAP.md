# Data Authority Map v0.3

**Status:** Draft for Architecture Review  
**as-of:** 2026-09-08

本書は、同じ業務情報を複数サブシステムが「正本」として持つことを防ぐため、主要Business ObjectごとのAuthorityを定義する。

---

## 1. 原則

1. **1 Business Object = 1 Primary Authority** を原則とする。
2. 他サブシステムは参照/Cache/派生値を持てるが、Authorityを奪わない。
3. 派生値は元データ・RuleVersion・計算時点を追跡可能にする。
4. 訂正/取消は原Eventを上書きして消すのではなくLineageを保持する。
5. 外部Systemが正本の場合でも、社内ではその外部情報を取り込んだAuthority SSを一つ決める。

---

## 2. Authority一覧

| Business Object / Data | Primary Authority | 主なConsumer | 備考 |
|---|---|---|---|
| Customer | SS01 顧客属性 | SS02,03,04,15,26,28,32,34,35,37 | 氏名住所/KYC/居住性/税務属性等 |
| Account | SS02 口座 | SS09,10,16-19,23,26,28,32,33 | 口座状態/階層/開閉 |
| Service Contract | SS03 契約・Service | SS09,10,12,13,26,28,32,34 | 特定/NISA/信用/電子交付等 |
| Sales/Channel Assignment | SS04 営業組織・Channel | SS01,09,14,32,34,37 | 部店/担当/IFA/Channel |
| Instrument Master | SS05 銘柄 | ほぼ全業務SS | 銘柄属性/商品分類/市場/税属性 |
| Business Calendar | SS06 市場・営業日 | SS09,21,22,25,26,28,32,33,CS02 | 業務日/市場日/受渡日 |
| Market Price / NAV | SS07 時価等 | SS10,13,15,20,25,29-31,36-39 | 外部原SourceはVendor等 |
| Business Parameter / Rate | SS08 制度・料率 | SS09-39 | effective_from/to必須 |
| Order | SS09 注文・約定 | SS10,15,34,37 | Order statusを含む |
| Execution / Trade | SS09 注文・約定 | SS12,14,16,18,20,21,26,31,32 | 約定訂正/取消Lineage |
| Buying Power | SS10 余力 | SS09,11,32 | 派生値。根拠残高/拘束を追跡 |
| Offering/Application/Allocation | SS11 募集・配分 | SS16-18,28,31,32 | IPO/PO/募集関連 |
| Open Position | SS12 建玉 | SS10,13-16,20,21,25,31,32 | 信用/Derivative等 |
| Collateral / Margin | SS13 担保・保証金 | SS10,15,16,31,32,36 | 代用評価含む |
| Fee / Commission | SS14 手数料 | SS16,17,20,26,31,32,37 | RuleはSS08、計算結果はSS14 |
| Credit Limit / Exposure | SS15 与信Risk | SS09,10,34,37 | Risk値のAuthority |
| Customer Receivable/Payable | SS16 顧客勘定 | SS10,15,17,18,20,31,32,36,37 | 未決済含む勘定残 |
| Cash Balance | SS17 資金残高 | SS10,13,16,20,31,32,36,39 | 顧客Cash残高 |
| Deposit/Withdrawal | SS17 資金残高 | SS16,31,32,39 | 銀行結果と照合 |
| Security Balance | SS18 証券残高 | SS10,13,15,16,20,25,26,28,32,36,37 | 預り/拘束/予定を区分 |
| Security Transfer | SS19 入出庫・移管 | SS18,24,26,28,31,32 | 他社移管/振替 |
| Book Cost / Valuation PL | SS20 評価・損益 | SS10,15,31,32,36,37 | 税取得価額とは目的が異なる場合あり |
| Clearing Obligation | SS21 清算 | SS16,22,24,31,39 | Netting後債権債務 |
| Settlement Instruction / Status | SS22 受渡・決済 | SS16-19,24,31,32,36,39 | DVP/FOP/Fail含む |
| Depository Account Structure | SS23 保振口座 | SS18,19,22,25,33 | 外部正本はJASDEC、社内管理AuthorityはSS23 |
| Reconciliation Break / Fail | SS24 照合・例外 | SS22,31,38,CS05 | Break lifecycle |
| Corporate Action Event | SS25 権利 | SS12-14,16-20,26-32,36 | Event条件/基準日/権利数量 |
| Tax Cost Basis | SS26 譲渡益税 | SS10,16-20,31-33,37 | 税務上取得価額 |
| Capital Gain/Loss | SS26 譲渡益税 | SS31-33,37 | 特定口座年初来を含む |
| Tax Withholding/Refund | SS26/SS27 | SS16,17,31-33 | 譲渡税はSS26、配当利金税はSS27 |
| Dividend/Interest Tax | SS27 配当・利金税 | SS16,17,26,31-33 | 外国税情報含む |
| NISA Account/Allowance | SS28 NISA | SS09,10,18,26,32,33,37 | 非課税枠・保有限度額 |
| Foreign Securities Business Record | SS29 外証 | SS16-27,31,32,37 | 海外固有情報 |
| Currency Balance / FX Trade | SS30 外貨・為替 | SS10,16,17,20,22,27,29,31,32,39 | 通貨別 |
| Accounting Journal | SS31 会計 | GL,SS33,37 | 元業務EventのAuthorityにはならない |
| Customer Report Instance | SS32 対客帳票 | Customer/Channel | 発行Version/交付状態 |
| Statutory Book / Submission | SS33 法定報告 | Authority/Tax | 提出Version/受付状態 |
| Compliance Rule Result / Alert | SS34 Compliance | SS09,11,38,33 | 取引可否/事後Alert |
| AML Risk / Alert | SS35 AML | SS02,34,38,33 | 制裁/Monitoring |
| Segregation Requirement | SS36 分別管理 | SS33,37,38 | 顧客資産分別必要額等 |
| Management Metric | SS37 情報系 | Internal Users | Derived Data。元業務Authorityではない |
| Work Item / Approval | SS38 事務Workflow | 原Authority SS | 業務補正は原Authorityへ返す |
| Funding Forecast / Liquidity | SS39 資金繰り | SS22,30,31,37 | 証券会社自身の資金需要 |
| External Message / File Receipt | CS01 外部接続 | Authority SS | Transport原本/送受信証跡 |
| Business Date / Job Status | CS02 Batch | 全SS | 業務日とJob制御 |
| User/Role Entitlement | CS03 IAM | 全SS | 業務ユーザー権限 |
| Audit Record | CS04 Audit | 全SS/監査 | 改ざん耐性を要求 |
| Recovery/Retry State | CS05 Operations | 全SS | 再処理/Recovery状態 |
| Internal Delivery State | CS06 Data Integration | 全SS | 配信状態。業務内容のAuthorityではない |

---

## 3. 間違えやすいAuthority境界

### 3.1 SS20 評価簿価 vs SS26 税務取得価額

同じ「取得価額」に見えても、目的とRuleが異なる可能性がある。

- SS20: 評価/損益/管理目的の簿価
- SS26: 税法上の取得価額/譲渡損益

同じ銘柄・数量でも完全に同じData Objectとみなさない。

### 3.2 SS16 顧客勘定 vs SS17 Cash

- SS16: 未決済債権債務を含むCustomer Ledger
- SS17: 実際のCash Balance/Movement

約定直後にSS16が変わっても、受渡前はSS17の実残高が同じ場合がある。

### 3.3 SS21 Clearing vs SS22 Settlement

- SS21: 何を支払う/受け取る義務があるか
- SS22: その義務をいつ・どの口座で実際に受渡すか、完了したか

### 3.4 SS25 権利 vs SS27 税

- SS25: Grossの権利内容/Entitlement
- SS27: 税区分/源泉税/Net支払

---

## 4. 派生Dataの必須Lineage

派生値は最低限以下を追跡可能とする。

```text
Derived Record
  ├─ source_object_id(s)
  ├─ source_event_id(s)
  ├─ calculation_rule_id
  ├─ rule_version
  ├─ business_date
  ├─ calculated_at
  ├─ correction_of
  └─ generated_by_system
```

特に余力、評価、税、会計、帳票、法定報告はLineageを必須とする。

---

## 5. Architecture Review観点

- [ ] 同一Objectを2つ以上のSSが正本化していないか
- [ ] Derived DataとAuthority Dataが混同されていないか
- [ ] 外部正本を取り込む社内Authorityが明確か
- [ ] 訂正/取消時のAuthorityと反映順序が明確か
- [ ] 年跨ぎ/締め後訂正のVersion管理が可能か
