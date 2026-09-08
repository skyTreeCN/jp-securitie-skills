# Conditional Business E2E Validation v0.4

**Status:** Completed for Architecture Freeze Candidate  
**as-of:** 2026-09-08

## 1. 目的

Tier B Conditional領域について、既存の42業務サブシステムでEnd-to-End処理が閉じるかを確認する。

対象:

1. 信用取引
2. 先物
3. オプション
4. 貸借取引
5. 外国証券
6. 募集/売出
7. 機関投資家の約定/決済照合

結論: **新規サブシステム追加を必要とする重大Gapは検出しなかった。**

---

## 2. 信用取引

```mermaid
flowchart LR
  DOC[SS42 書面] --> O[SS09 注文]
  O --> BP[SS10 余力]
  BP --> M[SS13 保証金]
  M --> R[SS15 与信Risk]
  R --> E[SS09 約定]
  E --> P[SS12 建玉]
  P --> L[SS16 顧客勘定]
  P --> C[SS21 清算]
  C --> S[SS22 決済]
  P --> CA[SS25 権利]
  P --> TAX[SS26 税]
  L --> ACC[SS31 会計]
  TAX --> REP[SS32 帳票]
```

必要Authority:
- Order/Execution = SS09
- Buying Power = SS10
- Open Position = SS12
- Margin/Collateral = SS13
- Fee/Interest = SS14
- Credit/Risk = SS15
- Customer Ledger = SS16
- Settlement = SS21/22
- Corporate Action = SS25
- Tax = SS26

**判定: Closed**

---

## 3. 先物

JSCCはOSE等の上場先物・オプションについて債務引受、Margin計算/管理、決済を行う。Customer MarginはOpen Positionを基礎に管理される。

```mermaid
flowchart LR
  A[SS03 Derivative契約] --> DOC[SS42 交付書面]
  DOC --> O[SS09 注文/約定]
  O --> P[SS12 建玉]
  P --> M[SS13 証拠金]
  P --> V[SS20 評価損益]
  P --> C[SS21 清算]
  M --> C
  C --> S[SS22 決済]
  S --> CASH[SS17 Cash]
  P --> ACC[SS31 会計]
  P --> REP[SS32 帳票]
```

固有Rule:
- Initial Margin / Variation Margin
- Intraday/Emergency Margin
- Cash Settlement / Physical Delivery
- Final Settlement/SQ
- Position report/cutoff

これらは新SSではなく、SS12/13/21/22の `TR06 × PR08` Ruleとする。

公式参考:
- JSCC Futures/Options Clearing: https://www.jpx.co.jp/jscc/en/cash/futures/assumption-obligation/futuresclearing.html
- JSCC Margin on Futures and Options: https://www.jpx.co.jp/jscc/en/cash/futures/marginsystem/margin.html

**判定: Closed**

---

## 4. オプション

```mermaid
flowchart LR
  O[SS09 注文/約定] --> P[SS12 建玉]
  P --> M[SS13 証拠金]
  P --> C[SS21 清算]
  P --> X{Exercise/Assignment?}
  X -- No --> S[SS22 決済]
  X -- Yes --> CA[SS25 権利/Exercise Event]
  CA --> S
  S --> L[SS16/17/18 勘定・Cash・証券]
  L --> A[SS31 会計]
  A --> R[SS32 帳票]
```

固有Rule:
- Premium
- Exercise/Assignment
- Expiration
- Cash/Physical Settlement
- Margin

SS25はCorporate Actionだけでなく、TR11として顧客意思を伴うExercise/Assignment Eventを扱うが、Option PositionのAuthorityはSS12に残す。

**判定: Closed**

---

## 5. 貸借取引

日本証券金融の貸借取引は制度信用取引の決済に必要な資金・株券を証券会社へ供給する外部業務であり、JSCCのDVP対象にも貸借取引等が含まれる。

```mermaid
flowchart LR
  E[信用約定/貸借需要] --> O[SS09 貸借申込]
  O --> CP[SS41 日本証券金融/決済条件]
  O --> POS[SS12 貸借Position]
  POS --> COL[SS13 担保]
  POS --> FEE[SS14 貸借料/品貸料等]
  POS --> C[SS21 清算]
  C --> S[SS22 決済]
  S --> BAL[SS17/18 資金・証券]
  POS --> CA[SS25 権利処理]
  S --> REC[SS24 Reconciliation]
```

重要Boundary:
- 貸借という取引 = TR08
- 貸借Position = SS12
- 担保 = SS13
- 貸借金利/品貸料等 = SS14
- Counterparty/SSI = SS41
- Clearing/Settlement = SS21/22

公式参考:
- 日本証券金融 貸借取引: https://www.jsf.co.jp/ja/business/r_taisyaku.html
- JSCC DVP: https://www.jpx.co.jp/jscc/seisan/genbutsu/acceptance_debt/dvp.html

**判定: Closed**

---

## 6. 外国証券

```mermaid
flowchart LR
  DOC[SS42 書面] --> O[SS09 注文]
  O --> F[SS29 外証]
  F --> CP[SS41 Broker/Custodian/SSI]
  F --> FX[SS30 外貨/FX]
  F --> M[SS40 Matching]
  M --> C[SS21 清算]
  C --> S[SS22 決済]
  S --> BAL[SS17/18 残高]
  F --> CA[SS25 Foreign CA]
  CA --> TAX[SS26/27 税]
  S --> REC[SS24 Reconciliation]
```

SS29はForeign-specific orchestration/recordを持つが、Order/Cash/Securities/Tax等のAuthorityは既存SSに残す。

**判定: Closed**

---

## 7. 募集・売出

```mermaid
flowchart LR
  D[SS42 目論見書/書面] --> A[SS11 申込]
  A --> BP[SS10 余力]
  A --> C[SS34 Compliance]
  C --> AL[SS11 抽選/配分]
  AL --> L[SS16/17 顧客勘定/払込]
  L --> B[SS18 証券残高]
  AL --> N[SS28 NISA 条件時]
  B --> R[SS32 帳票]
```

SS11がApplication/Allocation Authority、SS42がDocument Authority。

**判定: Closed**

---

## 8. 機関投資家 約定/決済照合

```mermaid
flowchart LR
  T[SS09 Trade] --> M1[SS40 約定照合]
  M1 --> SSI[SS41 SSI]
  SSI --> M2[SS40 決済照合]
  M2 --> C[SS21 Clearing]
  C --> S[SS22 Settlement]
  S --> R[SS24 Reconciliation/Fail]
```

JASDECの決済照合制度では約定照合、決済照合、SSI利用が独立した業務機能として存在するため、v0.4のSS40/41追加でBoundaryが閉じた。

公式参考:
- JASDEC 決済照合: https://faq.jasdec.com/faq/show/854
- JASDEC SSI: https://faq.jasdec.com/faq/show/862

**判定: Closed**

---

## 9. Conditional Coverage結論

| Domain | Result | New SS Needed |
|---|---|---|
| 信用 | Closed | No |
| 先物 | Closed | No |
| Option | Closed | No |
| 貸借 | Closed | No |
| 外国証券 | Closed | No |
| 募集/売出 | Closed | No |
| 機関投資家Matching | Closed | No |

**Architecture Gap = 0（本Review対象範囲）**

この結果、`SS01-SS42 + CS01-CS06` を引き続きArchitecture v1.0 Freeze Candidateとする。
