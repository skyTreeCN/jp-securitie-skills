# 日本証券基幹システム 全体ランドスケープ v0.4

**Status:** Draft for Architecture Freeze  
**as-of:** 2026-09-08

> 本図は論理サブシステムのみを描く。現物/信用等の取引種別、株式/債券/投信等の商品は別軸とする。

## 1. One-page System Landscape

```mermaid
flowchart LR

  subgraph EXT[外部 / Channel]
    CUST[顧客・営業店・Web/App・IFA]
    MKT[取引所 / PTS]
    CCP[JSCC等 清算機関]
    JAS[JASDEC / 決済照合]
    BANK[銀行 / 決済銀行]
    ISS[発行体 / 信託銀行]
    TAX[国税庁 / 税務署]
    REG[金融庁 / SESC / 日証協]
    VEND[情報Vendor / 文書Source]
    GC[海外市場 / Custody / SWIFT]
    OTH[他証券会社 / Fund関連]
  end

  subgraph CORE[証券基幹 Business Subsystems]
    A[SS01-04\n顧客・口座・契約・営業]
    B[SS05-08 + SS41\n銘柄・市場・Price/Rate・制度・取引先/SSI]
    C[SS09-15\n注文約定・余力・募集・建玉・担保・Fee・Risk]
    D[SS16-20\n顧客勘定・Cash・証券残高・移管・評価損益]
    E[SS21-24 + SS40\n清算・受渡決済・保振・Matching・Reconciliation/Fail]
    F[SS25-28\n権利・譲渡益税・配当利金税・NISA]
    G[SS29-30\n外証・外貨為替]
    H[SS31-39 + SS42\n会計・帳票・法定報告・Compliance・AML・分別・MIS・Workflow・資金繰り・交付書面]
  end

  subgraph COMMON[Common Services]
    X1[CS01 外部接続]
    X2[CS02 業務日付・Batch]
    X3[CS03 認証・権限]
    X4[CS04 監査証跡]
    X5[CS05 運用監視・Recovery]
    X6[CS06 Data連携・配信]
  end

  CUST --> A
  A --> H
  H --> C
  A --> C
  B --> C
  B --> E
  C --> D
  C --> E
  D <--> E
  D --> F
  E --> F
  G --> C
  G --> D
  G --> E
  F --> H
  D --> H
  E --> H

  X2 --> CORE
  X3 --> CORE
  X4 --> CORE
  X5 --> CORE
  X6 <--> CORE

  CORE <--> X1
  X1 <--> MKT
  X1 <--> CCP
  X1 <--> JAS
  X1 <--> BANK
  X1 <--> ISS
  X1 <--> TAX
  X1 <--> REG
  X1 <--> VEND
  X1 <--> GC
  X1 <--> OTH
```

---

## 2. Main Business Backbone

```mermaid
flowchart LR
  C[顧客/Channel]
  M[顧客・口座・契約]
  D[交付書面/同意]
  O[注文・約定]
  R[余力/Risk]
  L[顧客勘定・Cash・証券残高]
  MT[約定/決済照合]
  CL[清算]
  ST[決済]
  RC[Reconciliation/Fail]
  CT[権利・税]
  AC[会計]
  RP[帳票・法定報告]

  C --> M --> D --> O
  O <--> R
  O --> L
  O --> MT --> CL --> ST --> L
  ST --> RC
  L --> RC
  L --> CT
  ST --> CT
  O --> AC
  L --> AC
  CT --> AC
  O --> RP
  L --> RP
  CT --> RP
```

約定/決済照合を利用しない取引では `O -> CL` の直接経路も成立する。

---

## 3. 3-axis Requirement Model

```mermaid
flowchart TB
  SS[Subsystem\nSS01-SS42 / CS01-CS06]
  TR[Transaction\n現物・信用・募集・先物・Option・移管・FX等]
  PR[Product\n株式・債券・投信・ETF・Derivatives等]

  SS --> REQ[Business Requirement]
  TR --> REQ
  PR --> REQ
```

例: `SS09 注文・約定 × TR02 信用取引 × PR01 国内株式`

---

## 4. Architecture Gap Reviewで追加した3責務

| ID | 追加理由 |
|---|---|
| SS40 約定照合・決済照合 | JASDEC決済照合等のMatching lifecycleを、残高Reconciliation/Failから分離 |
| SS41 取引先・決済条件(SSI) | Counterparty、決済口座、Custodian、Settlement Bank、SSIのAuthority不足を補完 |
| SS42 交付書面・目論見書・同意 | 取引前文書・電子交付同意・交付証跡を、取引後帳票SS32から分離 |

詳細: `ARCHITECTURE_GAP_REVIEW_V0_3.md`

---

## 5. 総体設計Document Set

| Document | Authority |
|---|---|
| `OVERALL_DESIGN.md` | 全体システム構造/境界のAuthority |
| `CLASSIFICATION_MODEL.md` | Subsystem/Transaction/Product分類のAuthority |
| `SUBSYSTEM_CATALOG.md` | サブシステム一覧/責務のAuthority |
| `SUBSYSTEM_RELATION_MAP.md` | 内部関係のAuthority |
| `EXTERNAL_RELATION_MAP.md` | 外部主体/I/F関係のAuthority |
| `DATA_AUTHORITY_MAP.md` | Business Object正本のAuthority |
| `END_TO_END_FLOW.md` | 代表E2EフローのAuthority |
| `SCOPE_TIER_MODEL.md` | Core/Conditional/AdjacentのAuthority |
| `ARCHITECTURE_GAP_REVIEW_V0_3.md` | v0.3 Gap Review記録 |
| `TRANSACTION_CATALOG.md` | 取引種別のAuthority |
| `PRODUCT_CATALOG.md` | 商品分類のAuthority |
| `COVERAGE_MATRIX.md` | 3軸Coverage/GAP検証のAuthority |

---

## 6. Architecture Gate

個別サブシステムSkillの詳細化開始条件:

1. 42業務SS + 6共通SSの過不足Review完了
2. 内部関係Review完了
3. 外部関係Review完了
4. Data Authority Review完了
5. E2E Flow Review完了
6. Scope Tier確定
7. Coverage Matrixで重大GAPなし
8. ID/名称をv1.0でFreeze

Architecture Gate通過前は、新規の詳細Skill作成を進めない。
