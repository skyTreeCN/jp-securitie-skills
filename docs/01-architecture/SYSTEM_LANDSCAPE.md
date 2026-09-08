# 日本証券基幹システム 全体ランドスケープ v0.3

**Status:** Draft for Architecture Review  
**as-of:** 2026-09-08

> 本図はサブシステムのみを描く。現物/信用等の取引種別、株式/債券/投信等の商品は別軸とする。

## 1. One-page System Landscape

```mermaid
flowchart LR

  subgraph EXT[外部 / Channel]
    CUST[顧客・営業店・Web/App・IFA]
    MKT[取引所 / PTS]
    CCP[JSCC等 清算機関]
    JAS[JASDEC]
    BANK[銀行 / 決済銀行]
    ISS[発行体 / 信託銀行]
    TAX[国税庁 / 税務署]
    REG[金融庁 / SESC / 日証協]
    VEND[情報Vendor]
    GC[海外市場 / Custody / SWIFT]
    OTH[他証券会社 / Fund関連]
  end

  subgraph CORE[証券基幹 Business Subsystems]
    A[SS01-04\n顧客・口座・契約・営業]
    B[SS05-08\n銘柄・市場・Price/Rate・制度Parameter]
    C[SS09-15\n注文約定・余力・募集・建玉・担保・Fee・Risk]
    D[SS16-20\n顧客勘定・Cash・証券残高・移管・評価損益]
    E[SS21-24\n清算・受渡決済・保振口座・照合/Fail]
    F[SS25-28\n権利・譲渡益税・配当利金税・NISA]
    G[SS29-30\n外証・外貨為替]
    H[SS31-39\n会計・帳票・法定報告・Compliance・AML・分別・MIS・Workflow・資金繰り]
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
  A --> C
  B --> C
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
  M[Master/顧客口座]
  O[注文・約定]
  R[余力/Risk]
  L[顧客勘定・Cash・証券残高]
  CL[清算]
  ST[決済]
  CT[権利・税]
  AC[会計]
  RP[帳票・法定報告]

  C --> M --> O
  O <--> R
  O --> L
  O --> CL --> ST --> L
  L --> CT
  ST --> CT
  O --> AC
  L --> AC
  CT --> AC
  O --> RP
  L --> RP
  CT --> RP
```

---

## 3. 3-axis Requirement Model

```mermaid
flowchart TB
  SS[Subsystem\nSS01-SS39 / CS01-CS06]
  TR[Transaction\n現物・信用・募集・先物・Option・移管・FX等]
  PR[Product\n株式・債券・投信・ETF・Derivatives等]

  SS --> REQ[Business Requirement]
  TR --> REQ
  PR --> REQ
```

例:

`SS09 注文・約定 × TR02 信用取引 × PR01 国内株式`

---

## 4. 総体設計Document Set

| Document | Authority |
|---|---|
| `OVERALL_DESIGN.md` | 全体システム構造/境界のAuthority |
| `CLASSIFICATION_MODEL.md` | Subsystem/Transaction/Product分類のAuthority |
| `SUBSYSTEM_CATALOG.md` | サブシステム一覧/責務のAuthority |
| `SUBSYSTEM_RELATION_MAP.md` | 内部関係のAuthority |
| `EXTERNAL_RELATION_MAP.md` | 外部主体/I/F関係のAuthority |
| `DATA_AUTHORITY_MAP.md` | Business Object正本のAuthority |
| `END_TO_END_FLOW.md` | 代表E2EフローのAuthority |
| `TRANSACTION_CATALOG.md` | 取引種別のAuthority |
| `PRODUCT_CATALOG.md` | 商品分類のAuthority |
| `COVERAGE_MATRIX.md` | 3軸Coverage/GAP検証のAuthority |

---

## 5. Architecture Gate

個別サブシステムSkillの詳細化開始条件:

1. 39業務SS + 6共通SSの過不足Review完了
2. 内部関係Review完了
3. 外部関係Review完了
4. Data Authority Review完了
5. E2E Flow Review完了
6. Coverage Matrixで重大GAPなし

Architecture Gate通過前は、既存の詳細SkillはReference Sampleとして保持するが、新規の詳細化を進めない。
