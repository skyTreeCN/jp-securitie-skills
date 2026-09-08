# Subsystem Boundary Review v0.4

**Status:** Second Review Completed / Freeze Candidate  
**as-of:** 2026-09-08

## 1. Review目的

v0.4の `SS01-SS42 + CS01-CS06` について、追加・統合・再分割が必要かを第二段階でReviewする。

判定基準:

1. 独立したBusiness Authorityを持つか
2. 独立したState/Lifecycleを持つか
3. 複数商品・複数取引に共通する機能か
4. 外部制度上、独立したI/F・締め・Evidenceを持つか
5. 既存SSに入れると異質な責務を抱えすぎないか
6. 逆に分割すると単なる実装都合の細分化にならないか

---

## 2. Review結論

**現時点ではSS43以降の追加は行わない。**

v0.4で追加したSS40-42により、第一輪で見つかった大きなAuthority空洞は解消した。第二輪では、以下の境界を再確認し、**42業務SS + 6共通SSをArchitecture v1.0候補**とする。

ただし、外部主体の追加・E2E Coverage補強は行う。

---

## 3. 分割しないと判断した領域

### BR-01 SS34 Compliance・売買審査

候補分割:
- 取引前Compliance
- 事後売買審査/Surveillance
- Best Execution監視

**判定: 分割しない。**

理由:
- いずれも取引可否/取引監視RuleとCompliance Alertを中心とする同一統制Domain。
- Rule結果/AlertのAuthorityをSS34へ集約した方が一貫する。
- 物理実装では別Engine/Serviceでも、論理AuthorityはSS34でよい。

境界:
- AML/制裁/KYC RiskはSS35
- 注文Routing実行はSS09
- 顧客向けBest Execution Policy等の文書Version/交付証跡はSS42

---

### BR-02 SS35 AML・経済制裁

候補統合: SS34へ統合

**判定: 統合しない。**

理由:
- Customer Risk、Sanctions Screening、Transaction Monitoring、疑わしい取引対応は売買審査と異なるLifecycle/Evidenceを持つ。
- 顧客Onboarding/継続管理/送金・外貨等にも跨るため、SS34だけに入れると責務が過大になる。

---

### BR-03 SS32 対客帳票 vs SS33 法定帳簿/報告

候補統合: Output Managementへ統合

**判定: 統合しない。**

理由:
- SS32は顧客への発行・交付・再発行・電子交付Statusが中心。
- SS33は法定帳簿の保存、当局/税務/業界提出、提出Version/受付結果が中心。
- Recipient、Retention、訂正/再提出Lifecycle、Evidenceが異なる。

---

### BR-04 SS32 対客帳票 vs SS42 交付書面・目論見書・同意

**判定: 分離維持。**

理由:
- SS42は取引前/契約時の「交付が取引前提条件になる文書」とConsentがAuthority。
- SS32は取引後/期間後の「取引事実・残高・税結果を通知する帳票」がAuthority。

例:
- 投信目論見書 → SS42
- 取引報告書 → SS32
- 特定口座年間取引報告書 → SS32
- 電子交付に対する顧客同意 → SS42

---

### BR-05 SS24 Reconciliation/Fail vs SS38 Workflow

候補統合: Exception Operationsへ統合

**判定: 統合しない。**

理由:
- SS24はBreak/FailというBusiness CaseのAuthority。
- SS38は承認/担当割当/Maker-Checker等のWork Item LifecycleのAuthority。
- 1件のSS24 Breakが複数Workflow Taskを持つ場合がある。
- SS38の承認結果でSS24/SS22等の原Authorityを更新する。

---

### BR-06 SS07 時価・為替・基準価額管理

候補分割:
- Market Price
- NAV
- FX Rate
- Interest Rate

**判定: 現段階では分割しない。**

理由:
- いずれも外部/内部から取得するReference Calculation Inputという共通責務。
- 商品別Source/更新頻度はSkill内部のRule/Interface差分として扱える。
- SS30のFX Trade/Currency Balanceとは明確に分離する。

---

### BR-07 SS08 制度・料率・業務パラメータ管理

候補分割: Tax Parameter / Fee Parameter / Margin Parameter等

**判定: 分割しない。**

理由:
- SS08はBusiness Rule Parameter Repositoryという共通Authority。
- 計算結果のAuthorityは各業務SSに置く。
- Parameterごとのeffective date/versionが主責務であり、計算機能を持たない。

---

### BR-08 SS12 建玉管理

候補分割:
- 信用建玉
- 先物建玉
- Option建玉
- 貸借Position

**判定: 分割しない。**

理由:
- 信用/先物/Option/貸借は取引種別・商品差分。
- 共通するPosition lifecycle（Open/Increase/Decrease/Close/Maturity）をSS12でAuthority化する。
- 各TR/PR固有RuleはSkillの3軸交点に置く。

---

### BR-09 SS13 担保・保証金管理

候補分割:
- 信用保証金
- Derivative Margin
- 貸借担保

**判定: 分割しない。**

理由:
- Collateral asset、valuation、haircut、required amount、shortfall、margin call、releaseという共通Lifecycleを持つ。
- 制度差分はTR/PR Ruleとして扱う。

---

### BR-10 SS29 外国証券業務管理（外証）

候補廃止: 外国株/外国債をProduct差分だけにする

**判定: 維持。**

理由:
- 外国証券では海外Market/Custodian、Local Market Rule、Foreign Tax、Corporate Action、Multi-currency、Time Zone、Settlement convention等が横断的に追加される。
- 日本の証券会社実務では「外証」という独立した業務運用責務が大きい。
- ただしForeign Stock/Bond自体はPR軸であり、SS29が注文/残高/税の正本を奪わない。

---

### BR-11 SS23 保振加入者情報・振替口座管理

候補Generic化: Depository/CSD Account Management

**判定: 日本証券参照モデルでは現名称維持。**

理由:
- 本Repositoryは日本証券基幹に特化しており、JASDEC制度の加入者情報/振替口座/株主通知等は重要な日本固有責務。
- 外国CustodyはSS29/SS41に置く。

---

### BR-12 SS39 資金繰り・決済資金管理

候補統合: SS17またはSS22へ統合

**判定: 分離維持。**

理由:
- SS17は顧客資金残高、SS22はTransaction Settlement。
- SS39は証券会社自身の決済流動性・Funding Forecast・Nostro FundingがAuthority。
- 顧客資産と会社資金を混同しないため分離が必要。

---

## 4. 追加サブシステムにしない候補

### 4.1 J-IRISS

**判定: 外部System。新SSにしない。**

- JSDAのJ-IRISSは、上場会社役員情報と証券会社顧客情報を照合し、内部者登録カードの精度向上に用いる外部インフラ。
- 社内責務:
  - SS01: Customer/Insider属性
  - SS34: Insider restriction / trade check
  - CS01: J-IRISSとの送受信
  - CS02: 年次等の定期照合Scheduling

公式:
- https://www.jsda.or.jp/anshin/j-iriss/
- https://www.jsda.or.jp/houdou/2025/20250415_pabukome_jk.pdf

### 4.2 e-Tax

**判定: 外部提出Endpoint。新SSにしない。**

- SS26/27/28: 税計算/税務Data Authority
- SS33: 法定提出Data/提出Version Authority
- CS01: e-Tax Transport/受付結果

### 4.3 日本証券金融（貸借取引）

**判定: 外部業務インフラ。新SSにしない。**

- TR08貸借取引としてSS09/12/13/14/18/21/22/25等を横断する。
- Counterparty/Settlement ReferenceはSS41。

### 4.4 最良執行/SOR

**判定: 新SSにしない。**

- SS09: Routing/Execution Venue Selection/Execution Result
- SS34: Best Execution Rule/Policy Compliance
- SS42: 顧客向けPolicy文書Version/Delivery Evidence

### 4.5 自己資本規制比率・全社Risk

**判定: 現段階はTier C Adjacentを維持。**

理由:
- 証券会社経営上は必須級だが、取引基幹のCustomer/Trade/Settlement Bookとは別のEnterprise Risk/Regulatory Capital Domainとして構成可能。
- Core側からSS15/20/31/39等のDataを供給する。
- v1.0 Freeze前にScopeの最終確認項目として残す。

---

## 5. 第二輪で確認した主要Boundary

| Boundary | Authority A | Authority B | 判定 |
|---|---|---|---|
| Order/Trade vs Clearing | SS09 | SS21 | OK |
| Trade vs Match | SS09 | SS40 | OK |
| Match vs Reconciliation | SS40 | SS24 | OK |
| SSI Reference vs Settlement Instruction | SS41 | SS22 | OK |
| Customer Ledger vs Cash | SS16 | SS17 | OK |
| Open Position vs Security Balance | SS12 | SS18 | OK |
| Valuation Cost vs Tax Cost Basis | SS20 | SS26 | OK |
| Corporate Action Gross vs Tax | SS25 | SS26/27 | OK |
| Post-trade Report vs Pre-trade Disclosure | SS32 | SS42 | OK |
| Regulatory Submission vs Customer Report | SS33 | SS32 | OK |
| Compliance vs AML | SS34 | SS35 | OK |
| Exception Case vs Work Item | SS24 | SS38 | OK |
| Customer Cash vs Firm Liquidity | SS17 | SS39 | OK |
| External Transport vs Internal Delivery | CS01 | CS06 | OK |

---

## 6. 第二轮Review后结构

```text
Business Subsystems : SS01 - SS42 = 42
Common Subsystems   : CS01 - CS06 = 6
------------------------------------
Total Logical SS    : 48
```

**追加SS候補: 0**

現時点の判定:

> 48論理サブシステムをArchitecture v1.0 Freeze Candidateとする。

---

## 7. Freeze前に残す検証

第二輪Boundary Reviewは閉じるが、次のCoverage Verificationを行ってからv1.0へFreezeする。

1. J-IRISSをExternal Relationへ正式追加
2. e-TaxをExternal Relationへ明示
3. 先物/Option E2E
4. 貸借/Repo E2E
5. Tier A Core Flowが全て閉じているか確認
6. Tier B Conditionalの主要業務別Coverage確認
7. 全48SSの名称/IDを最終確認

---

## 8. Reviewに用いる主な公開根拠

- NRI I-STAR/CORE: https://www.nri.com/jp/service/solution/i_star_core.html
- NRI I-STAR/GV: https://www.nri.com/jp/service/solution/i_star_gv.html
- JASDEC 決済照合/SSI: https://faq.jasdec.com/
- JSDA J-IRISS: https://www.jsda.or.jp/anshin/j-iriss/
- JSDA 自主規制: https://www.jsda.or.jp/shijyo/seido/jishukisei/
- FSA 総合的監督指針: https://www.fsa.go.jp/common/law/guide/kinyushohin/
- 日本証券金融: https://www.jsf.co.jp/
