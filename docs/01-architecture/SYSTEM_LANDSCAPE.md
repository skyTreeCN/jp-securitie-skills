# 日本証券基幹システム 全体ランドスケープ v0.2

## 1. 分類

本図はサブシステムのみを描く。現物/信用等の取引種別、株式/債券/投信等の商品は別軸とする。

## 2. 全体像

```mermaid
flowchart LR
  CUST[顧客 / 営業店 / ネット / アプリ]
  MKT[取引所 / PTS]
  CLR[JSCC等 清算機関]
  JAS[JASDEC]
  BANK[銀行 / 日銀関連]
  ISS[発行体 / 信託銀行]
  TAX[国税庁 / 税務署]
  REG[金融庁 / SESC / 日証協]
  CUS[海外カストディ / SWIFT]
  INFO[情報ベンダー]

  subgraph CORE[業務サブシステム]
    A[SS01-SS04\n顧客・口座・契約・営業]
    B[SS05-SS08\n銘柄・市場・レート・制度パラメータ]
    C[SS09-SS15\n注文約定・余力・建玉・担保・手数料・与信]
    D[SS16-SS20\n顧客勘定・資金・証券残高・移管・評価]
    E[SS21-SS24\n清算・決済・保振口座・照合例外]
    F[SS25-SS28\n権利・譲渡益税・配当利金税・NISA]
    G[SS29-SS30\n外証・外貨為替]
    H[SS31-SS39\n会計・帳票・報告・コンプラ・AML・分別・情報・事務・資金繰り]
  end

  subgraph COMMON[共通系サブシステム]
    X[CS01 外部接続]
    Y[CS02 業務日付・バッチ]
    Z[CS03-CS06\n権限・監査・運用再処理・データ連携]
  end

  CUST --> A --> C --> D --> E
  B --> C
  B --> D
  E --> F
  D --> F
  G --> D
  G --> E
  F --> H
  D --> H
  E --> H

  Y --> C
  Y --> D
  Y --> E
  Y --> F
  Z --> CORE

  C <--> X <--> MKT
  E <--> X <--> CLR
  E <--> X <--> JAS
  D <--> X <--> BANK
  F <--> X <--> ISS
  G <--> X <--> CUS
  H <--> X <--> TAX
  H <--> X <--> REG
  B <--> X <--> INFO
```

## 3. 取引種別・商品との交差

```mermaid
flowchart TB
  T[取引種別\n現物 / 信用 / 募集売出 / 先物 / オプション 等]
  P[商品\n株式 / 債券 / 投信 / ETF / 外国証券 等]
  S[サブシステム\nSS01-SS39]

  T --> R[業務要件]
  P --> R
  S --> R
```

例: `信用取引 × 国内株式`は、SS09注文約定、SS10余力、SS12建玉、SS13担保保証金、SS16顧客勘定、SS18証券残高、SS21清算、SS22決済、SS25権利、SS26譲渡益税、SS31会計、SS32帳票等を横断する。

## 4. 主な対外関係

| 外部主体 | 主に関係するサブシステム |
|---|---|
| 取引所/PTS | SS09, SS34, CS01 |
| JSCC等清算機関 | SS21, SS22, SS24, SS39, CS01 |
| JASDEC | SS18, SS19, SS22, SS23, SS24, SS25, CS01 |
| 銀行 | SS17, SS22, SS30, SS39, CS01 |
| 発行体/信託銀行 | SS25, SS27, SS32, CS01 |
| 国税庁/税務署 | SS26, SS27, SS28, SS33, CS01 |
| 金融庁/SESC/日証協 | SS33, SS34, SS35, SS36, CS01 |
| 海外カストディ/SWIFT | SS29, SS30, SS22, SS24, CS01 |
| 情報ベンダー | SS05, SS06, SS07, SS25, CS01 |

## 5. 公開根拠

- NRI THE STAR: https://www.nri.com/jp/service/solution/the_star.html
- NRI I-STAR/CORE: https://www.nri.com/jp/service/solution/i_star_core.html
- NRI I-STAR/GV: https://www.nri.com/jp/service/solution/i_star_gv.html
- JASDEC: https://www.jasdec.com/rule/
