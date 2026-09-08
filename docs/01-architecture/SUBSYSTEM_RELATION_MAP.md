# サブシステム内部関係マップ v0.4

**Status:** Draft for Architecture Freeze  
**as-of:** 2026-09-08

本書はサブシステム間の**主要な業務関係**を定義する。詳細I/F項目はArchitecture Freeze後、各サブシステムSkillで定義する。

## 1. 読み方

- `Upstream`: 当該サブシステムが主要INPUTを受ける相手
- `Downstream`: 当該サブシステムが主要OUTPUT/Eventを渡す相手
- `主要Object/Event`: 受渡す代表的な業務情報

---

## 2. 顧客・口座・営業

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS01 | 顧客属性管理 | Channel, CS01 | SS02,03,04,10,15,26,28,32,34,35,36,37,42 | Customer, KYC属性, 居住性, 税務属性, 適合性属性 |
| SS02 | 口座管理 | SS01, SS03, Channel | SS09,10,16,17,18,23,26,28,32,33,36,42 | Account, AccountStatus, 口座階層, 口座開閉 |
| SS03 | 契約・サービス管理 | SS01,02, Channel | SS09,10,12,13,26,28,32,34,42 | 特定口座契約, NISA契約, 信用契約, 電子交付契約等 |
| SS04 | 営業組織・担当者・チャネル管理 | 社内組織/Channel | SS01,09,14,32,34,37,42 | Branch, SalesRep, Channel, IFA/仲介帰属 |

---

## 3. 基準情報・マスター

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS05 | 銘柄管理 | CS01/情報Vendor/取引所等 | SS09-15,18-20,25-30,32-37,40-42 | Instrument, 商品属性, 市場属性, 権利/税属性, 取扱可否 |
| SS06 | 市場・営業日管理 | CS01/市場Calendar | SS09,11,12,21,22,25,26,28,32,33,39,40,41,CS02 | BusinessCalendar, MarketSession, SettlementCalendar, Cutoff |
| SS07 | 時価・為替・基準価額管理 | CS01/情報Vendor/銀行等 | SS10,13,15,20,25,29,30,31,36,37,39 | Price, NAV, FXRate, InterestRate, ValuationRate |
| SS08 | 制度・料率・業務Parameter管理 | 制度改定/業務管理者 | SS09-42 | TaxRate, FeeRate, MarginRate, Haircut, SettlementCycle, RuleVersion |
| SS41 | 取引先・決済条件（SSI）管理 | CS01/JASDEC/銀行/Custodian/業務管理者 | SS09,21,22,23,24,29,30,39,40 | Counterparty, ParticipantCode, SettlementBank, CustodyAccount, CashAccount, SSI, SettlementPlace |

---

## 4. 取引・Risk

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS09 | 注文・約定管理 | Channel, SS01-08,SS10,15,34,41,42,CS01 | SS10,12,14,16,18,20,21,26,31,32,37,40 | Order, RoutingDecision, Execution, Correction, Cancel, TradeEvent |
| SS10 | 余力管理 | SS01-03,05-08,12,13,15-18,26,28 | SS09,11,17,32 | BuyingPower, WithdrawalPower, SellableQty, Reserve/Release |
| SS11 | 募集・売出・配分管理 | SS01-08,10,34,42,Channel | SS16-18,26,28,31,32,37,40 | Application, Allocation, Subscription, PaymentDue |
| SS12 | 建玉管理 | SS09,05-08 | SS10,13,14,15,16,20,21,25,31,32 | OpenPosition, Close/Repay, Maturity, PositionBalance |
| SS13 | 担保・保証金管理 | SS12,17,18,07,08 | SS10,15,16,31,32,36 | Collateral, Margin, Haircut, MarginCall, Shortfall |
| SS14 | 手数料・諸経費管理 | SS04,05,08,09,12 | SS16,17,20,26,31,32,37 | Commission, Fee, Interest/Expense, ChargeEvent |
| SS15 | 与信・取引Risk管理 | SS01-03,07,08,10,12,13,16-20 | SS09,10,34,37 | CreditLimit, RiskExposure, LimitCheck, RiskBlock |

---

## 5. 顧客勘定・資産

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS16 | 顧客勘定管理 | SS09,11-14,17-19,21-22,25-27,30 | SS10,15,17,18,20,31,32,36,37 | Receivable, Payable, 未決済金額, 顧客勘定残 |
| SS17 | 資金残高・入出金管理 | Channel/銀行, SS09,11,13,14,16,22,25-27,30 | SS10,13,16,20,31,32,36,39 | CashBalance, Deposit, Withdrawal, Transfer, CashMovement |
| SS18 | 証券預り・残高管理 | SS09,11,12,19,22,25,29 | SS10,13,15,16,20,24,25,26,28,32,36,37 | SecurityBalance, StockRecord, AvailableQty, RestrictedQty, SettlementPending |
| SS19 | 入出庫・移管管理 | Channel/他社, SS02,05,18,23,41,CS01 | SS18,24,26,28,31,32 | TransferIn/Out, BookEntryTransfer, CostBasisTransfer |
| SS20 | 評価・損益管理 | SS07,09,12,14,16-19,25,29,30 | SS10,15,31,32,36,37 | BookCost, MarketValue, UnrealizedPL, RealizedPL |

---

## 6. 清算・決済・照合

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS21 | 清算管理 | SS09,11,12,05-08,40,41,CS01 | SS16,22,24,31,39 | ClearingPosition, NetObligation, Cash/SecurityObligation |
| SS22 | 受渡・決済管理 | SS21,16-18,23,29,30,39,40,41,CS01 | SS16-19,24,31,32,36,39 | SettlementInstruction, DVP/FOP, SettlementStatus, Fail |
| SS23 | 保振加入者情報・振替口座管理 | SS02,05,41,CS01/JASDEC | SS18,19,22,25,33,40 | Participant, AccountStructure, BookEntryAccount, JASDEC属性 |
| SS24 | 残高・資金照合／例外・Fail管理 | SS09,16-23,25,29-31,40,41,CS01 | SS22,31,38,CS05 | BalanceBreak, CashBreak, Exception, SettlementFail, ResolutionStatus |
| SS40 | 約定照合・決済照合管理 | SS09,11,21,22,23,41,CS01/JASDEC等 | SS21,22,24,31 | TradeMatch, SettlementMatch, MatchingStatus, SettlementInstructionCandidate |

---

## 7. 権利・税務

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS25 | 権利管理 | SS05-07,18,23,29,41,CS01/発行体等 | SS12-14,16-20,26-27,29-32,36 | CorporateAction, Entitlement, Election, Dividend, Split, Merger, Redemption |
| SS26 | 譲渡益税・特定口座 | SS01-03,05-09,14,18-20,25,27,28 | SS10,16,17,20,31-33,37 | CostBasis, DisposalPL, TaxWithheld, TaxRefund, AnnualTaxLedger |
| SS27 | 配当・利金税 | SS01-03,05,08,18,25,29,30 | SS16,17,26,31-33 | DividendTax, InterestTax, ForeignTax, NetPayment |
| SS28 | NISA・非課税口座 | SS01-03,05-09,18,19,25,26 | SS09,10,18,26,32,33,37 | NISAAccount, AnnualAllowance, LifetimeLimit, NonTaxableHolding |

---

## 8. 外国証券・外貨

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS29 | 外国証券業務管理 | SS01-09,18,25,30,41,CS01/海外市場・Custody | SS16-18,20-27,31,32,37,40 | ForeignTrade, CustodyPosition, ForeignCA, LocalTax/Settlement情報 |
| SS30 | 外貨・為替管理 | Channel, SS07,08,17,22,29,41,CS01/銀行等 | SS10,16,17,20,22,27,29,31,32,39 | FXTrade, CurrencyBalance, Conversion, SettlementFXRate |

---

## 9. 会計・帳票・統制

| ID | サブシステム | Upstream | Downstream | 主要Object/Event |
|---|---|---|---|---|
| SS31 | 会計 | SS09,11-14,16-22,24-30,36,39,40 | GL/財務会計, SS37, SS33 | Journal, AccountPosting, TrialBalance連携 |
| SS32 | 対客帳票・電子交付 | SS01-31,37,CS02 | 顧客/Channel, SS33(必要時) | TradeConfirm, Statement, TaxReport, ElectronicDelivery |
| SS33 | 法定帳簿・当局/税務報告 | SS01-32,34-36,42,CS02 | CS01/国税庁・当局・業界 | StatutoryBook, TaxSubmission, RegulatoryReport |
| SS34 | Compliance・売買審査 | SS01-09,11,15,37,42 | SS09,11,38,33,42 | Suitability, InsiderRestriction, BestExecutionRuleCheck, TradeSurveillance, Alert |
| SS35 | AML・経済制裁 | SS01-04,09,17,19,29,30 | SS02,34,38,33 | CustomerRisk, SanctionMatch, TransactionAlert, STR関連 |
| SS36 | 顧客資産分別管理 | SS16-18,20-23,25,30,31,39 | SS33,37,38 | SegregationRequirement, CustomerAsset, TrustRequiredAmount, AuditEvidence |
| SS37 | 情報系・営業日報・経営情報 | SS01-36,39-42 | 社内利用者/BI | KPI, AUM, Revenue, Risk, Activity, ManagementReport |
| SS38 | 事務Workflow・承認 | SS01-37,40-42,CS05 | 原Authority SS, SS33 | WorkItem, Approval, MakerChecker, Hold/Release, ExceptionAction |
| SS39 | 資金繰り・決済資金管理 | SS16,17,21,22,30,31,36,41 | SS22,30,31,37 | FundingForecast, SettlementLiquidity, NostroFunding, CashNeed |
| SS42 | 交付書面・目論見書・同意管理 | SS01-08,34,Channel/文書Source | SS03,09,11,32,33,34,37 | DocumentDefinition, Version, DeliveryRequirement, DeliveryEvidence, Consent, Withdrawal |

---

## 10. 共通系

| ID | サブシステム | 主な接続先 | 主要責務 |
|---|---|---|---|
| CS01 | 外部接続 | 外部主体 ⇔ 各Authority SS | Protocol変換, Session, ACK/NACK, File/API/MQ, 再送 |
| CS02 | 業務日付・Batch統制 | SS06, 全業務SS | BusinessDate, JobChain, Cutoff, Close/Open, RestartPoint |
| CS03 | 認証・権限管理 | 全SS | Authentication, Role, Entitlement, SoD |
| CS04 | 監査証跡・操作履歴 | 全SS | Who/When/What/Before/After, Approval履歴, Evidence |
| CS05 | 運用監視・Recovery・再処理 | 全SS, SS24,38 | Alert, Retry, Replay, Recovery, BCP/DR |
| CS06 | Data連携・配信 | 全SS | 内部API/File/MQ, Event配信, STP, Distribution |

---

## 11. 重要な責務境界

### SS09 注文・約定 vs SS40 約定/決済照合

- SS09: 自社のOrder/Execution/Trade事実のAuthority
- SS40: 相手Party/決済当事者とのMatching状態のAuthority

### SS40 Matching vs SS24 Reconciliation/Exception

- SS40: 約定/決済Instructionの照合一致・不一致・未照合
- SS24: 残高/資金等のReconciliation、Exception、Settlement Fail、Break解消

### SS41 SSI vs SS22 Settlement

- SS41: どのCounterparty/口座/決済条件を使うかというReference Authority
- SS22: 具体的なTransactionについて生成されたSettlement Instruction/StatusのAuthority

### SS32 対客帳票 vs SS42 交付書面

- SS42: 取引前/契約時の書面Version・交付要否・交付証跡・同意
- SS32: 取引後/期間後のCustomer Report生成・交付

### SS16 顧客勘定 vs SS17 Cash

- SS16: 将来受払を含むCustomer Receivable/Payable
- SS17: 実際のCash Balance/Movement

### SS18 証券残高 vs SS12 建玉

- SS18: 保有/預り証券数量
- SS12: 未決済の信用/Derivative/貸借等Position

### SS25 権利 vs SS26/27 税

- SS25: 権利Event・権利数量・Gross Entitlement
- SS26/27: 税務上取得価額/譲渡損益/源泉税

### SS31 会計 vs 業務SS

- 業務事実のAuthorityは各業務SS
- SS31は業務Eventから会計仕訳を生成する

---

## 12. 次工程

本書と `OVERALL_DESIGN.md`, `EXTERNAL_RELATION_MAP.md`, `DATA_AUTHORITY_MAP.md`, `END_TO_END_FLOW.md`, `SCOPE_TIER_MODEL.md` をArchitecture Freeze対象とする。個別サブシステム詳細化はGate通過後に開始する。
