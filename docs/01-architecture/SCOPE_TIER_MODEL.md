# Scope Tier Model v1.0

**Status:** Frozen for Architecture v1.0  
**as-of:** 2026-09-08

## 1. 目的

「THE STAR級の日本リテール総合証券基幹として持つべき責務」「取扱業務によって必要となる責務」「証券会社として必要でも取引基幹の外側に置く責務」を分離する。

本Repositoryでは**42業務SSすべてを総体モデルに含める**。Tier Bは“不要”という意味ではなく、その証券会社が該当業務を扱う場合に有効化される論理Domainを意味する。

---

## 2. Tier定義

### Tier A — Core

THE STAR級の日本リテール総合証券バックオフィスをBenchmarkとした場合、基本構成として扱う責務。

### Tier B — Conditional Core

取扱商品・取引・顧客層・Channel・決済モデルにより必要となるが、必要な会社ではCoreの一部として扱う責務。

### Tier C — Adjacent

証券会社として必要/重要な場合があるが、本Repositoryでは「証券取引基幹の業務Book/Transaction Processing」の外側に置き、CoreとのI/Fを定義する領域。

> Tierは重要度を示さない。例えば自己資本規制比率管理は法令・監督上極めて重要だが、取引基幹とは別のEnterprise Risk/Regulatory Capital Systemとして構成できるためTier Cとする。

---

## 3. Tier A — Core（Freeze）

| ID | サブシステム | Coreとする理由 |
|---|---|---|
| SS01 | 顧客属性管理 | 口座・税・Compliance等の顧客Authority |
| SS02 | 口座管理 | 顧客勘定の基本Authority |
| SS03 | 契約・サービス管理 | 特定口座/NISA/信用等Service適用の前提 |
| SS04 | 営業組織・担当者・チャネル管理 | リテール営業/仲介/Channel帰属 |
| SS05 | 銘柄管理 | 全商品処理のReference |
| SS06 | 市場・営業日管理 | 注文/受渡/税/BatchのCalendar Authority |
| SS07 | 時価・為替レート・基準価額管理 | 余力/評価/担保等のReference |
| SS08 | 制度・料率・業務パラメータ管理 | 制度改正/Rate/Rule Version Authority |
| SS09 | 注文・約定管理 | 取引Core |
| SS10 | 余力管理 | リテール注文可否/拘束Core |
| SS14 | 手数料・諸経費管理 | 顧客取引料金Core |
| SS16 | 顧客勘定管理 | 債権債務/未決済Book |
| SS17 | 資金残高・入出金管理 | 顧客Cash Book |
| SS18 | 証券預り・残高管理 | 顧客Securities Book |
| SS19 | 入出庫・移管管理 | 他社移管/JASDEC振替 |
| SS20 | 評価・損益管理 | 顧客/業務評価・損益 |
| SS21 | 清算管理 | 約定後Clearing |
| SS22 | 受渡・決済管理 | Settlement Authority |
| SS23 | 保振加入者情報・振替口座管理 | 日本固有JASDEC制度対応 |
| SS24 | 残高・資金照合／例外・Fail管理 | Book完整性/Settlement Fail管理 |
| SS25 | 権利管理 | Corporate Action |
| SS26 | 譲渡益税・特定口座 | 日本リテール証券の主要税務 |
| SS27 | 配当・利金税 | 配当/利金源泉税 |
| SS28 | NISA・非課税口座 | **THE STAR級日本リテール総合証券のBenchmarkではCore扱い** |
| SS31 | 会計 | 証券業務仕訳/財務会計連携 |
| SS32 | 対客帳票・電子交付 | 取引報告/残高/税務帳票 |
| SS33 | 法定帳簿・当局/税務報告 | 法定保存/提出 |
| SS34 | コンプライアンス・売買審査 | 注文前後Compliance |
| SS35 | AML・経済制裁 | Onboarding/取引Monitoring |
| SS36 | 顧客資産分別管理 | 顧客資産保護 |
| SS37 | 情報系・営業日報・経営情報 | THE STAR公開Coverageに営業日報/情報系が含まれるためCore |
| SS38 | 事務ワークフロー・承認 | 訂正/例外/事務運用の統制 |
| SS41 | 取引先・決済条件（SSI）管理 | 決済先/口座/Counterparty Reference |
| SS42 | 交付書面・目論見書・同意管理 | 取引前/契約時の法定/業務Evidence |
| CS01 | 外部接続 | 市場/清算/保振/銀行/税務等接続 |
| CS02 | 業務日付・バッチ統制 | EOD/EOM/EOY統制 |
| CS03 | 認証・権限管理 | User/Role/SoD |
| CS04 | 監査証跡・操作履歴 | Audit Evidence |
| CS05 | 運用監視・Recovery・再処理 | 安定運用/Recovery |
| CS06 | データ連携・配信 | 内部STP/Event/Data Delivery |

**Tier A: 34業務SS + 6共通SS = 40論理SS**

---

## 4. Tier B — Conditional Core（Freeze）

| ID | 条件 | 主な適用 |
|---|---|---|
| SS11 募集・売出・配分 | IPO/PO/公募/募集を扱う | リテール募集業務 |
| SS12 建玉 | 信用、先物、Option、貸借等を扱う | Margin/Derivative/Securities Lending |
| SS13 担保・保証金 | 信用、Derivative、担保取引を扱う | Margin/Collateral |
| SS15 与信・取引Risk | 顧客与信/取引Limitが必要 | 信用、法人、店頭等 |
| SS29 外国証券業務管理 | 外国証券を扱う | 外株/外債等 |
| SS30 外貨・為替管理 | 外貨建商品/外貨決済を扱う | 外証/FX |
| SS39 資金繰り・決済資金管理 | 証券会社自身のFunding/Liquidityを基幹内で管理 | Settlement Liquidity |
| SS40 約定照合・決済照合管理 | JASDEC決済照合/機関投資家等のMatchingを扱う | Wholesale/Institutional等 |

**Tier B: 8業務SS**

Tier A + Tier B = **42業務SS**。

---

## 5. Tier C — Adjacent（Freeze）

| 領域 | Coreとの主な接続元 | Freeze判断 |
|---|---|---|
| 全社GL/連結会計 | SS31 | CoreのAccounting Journalを受領する全社System |
| 自己資本規制比率・Enterprise Market/Counterparty/Liquidity Risk | SS15,20,31,39 | **Adjacent維持**。法令上重要だが取引基幹Bookとは別Authority Domain |
| CRM/営業提案 | SS01,04,37 | Front/営業支援 |
| Web/スマホ/営業店Front | SS01-10,17-19,32,42 | Channel層 |
| Investment Banking（ECM/DCM/引受審査） | SS11,33,34 | Securities Back-office外のIB Domain |
| Wrap/投資一任運用Engine | SS03,09,18,20,32 | Portfolio/Discretionary Management Domain |
| 全社Document Archive | SS32,33,42,CS04 | Long-term enterprise archive |
| 人事/給与 | - | Scope外 |

### 自己資本規制/Enterprise RiskをAdjacentとする理由

金融庁監督指針では第一種金融商品取引業者に自己資本規制比率、市場Risk、取引先Risk、流動性Riskの管理を求めている。したがって業務としては必須級である。

一方、本RepositoryのAuthorityは**Customer / Order / Trade / Position / Cash / Securities / Clearing / Settlement / Tax / Accounting等の証券取引基幹Book**である。自己資本規制・Enterprise Riskはこれらの複数Bookを集約して会社全体健全性を算定する別Domainとして構築可能なため、v1.0ではTier C Adjacentに固定する。

Core側の責務:

- SS15: 顧客/取引Risk Exposure
- SS20: Valuation/PL
- SS31: Accounting/Capital-related input
- SS39: Liquidity/Funding input
- SS33: 必要なRegulatory Reporting連携

---

## 6. THE STAR Benchmarkとの整合

NRI公開情報ではTHE STARは証券会社の総合Back-office/勘定系として、口座開設、注文、決済、情報系、Compliance、営業日報、財務会計、報告等を総合的に支援する。

本Scope Tierは、これをTier Aの中心に置き、取扱業務に応じるForeign/Derivative/Institutional Matching等をTier Bで拡張する。

公開参照:
- https://www.nri.com/jp/service/solution/the_star.html
- https://www.nri.com/jp/service/solution/i_star_core.html
- https://www.fsa.go.jp/common/law/guide/kinyushohin/04a.html

---

## 7. Freeze原則

1. v1.0以後、Tier/SSの追加・削除・境界変更はChange Requestで行う。
2. Tier A/Bは論理責務であり、別App/別DBを強制しない。
3. 1 Packageが複数SSを実装してよい。
4. 1 SSを複数Serviceに物理分割してよい。
5. Tier Cの内部設計は本RepositoryのPrimary Scope外だが、Core I/Fは必ず定義する。
