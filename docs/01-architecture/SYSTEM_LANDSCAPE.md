# 日本証券基幹システム 全体ランドスケープ

## 1. 全体像

```mermaid
flowchart LR
  CUST[顧客/営業店/ネット/アプリ]
  INTM[銀行/IFA/仲介]
  MKT[取引所/PTS]
  CLR[JSCC等 清算]
  JAS[JASDEC 保振]
  BANK[銀行]
  ISS[発行体/信託銀行]
  TAX[国税庁/税務署]
  REG[金融庁/SESC/日証協]
  CUS[海外カストディ/SWIFT]

  subgraph CORE[証券基幹 参照モデル]
    CA[S01-S03 顧客・口座・契約]
    MST[S04-S05 銘柄/市場/営業日]
    TRD[S06-S12 注文・商品取引]
    AST[S13-S18 金銭/残高/受渡/権利/手数料]
    TX[S19-S21 税/NISA/配当利金税]
    FOR[S22-S24 外証/外貨/外国決済]
    CTL[S25-S29 担保/分別/コンプラ/会計/照合]
    OUT[S30-S32 対客帳票/法定報告/情報系]
    HUB[S33 外部接続]
    BAT[S34 バッチ/締め]
    AGT[S35 仲介]
  end

  CUST --> CA
  CUST --> TRD
  INTM --> AGT --> CA
  AGT --> TRD
  CA --> TRD
  MST --> TRD
  TRD --> AST
  TRD --> CTL
  AST --> TX
  AST --> CTL
  TX --> OUT
  AST --> OUT
  CTL --> OUT
  FOR --> AST
  FOR --> TX
  BAT --> TRD
  BAT --> AST
  BAT --> TX
  BAT --> OUT

  TRD <--> HUB <--> MKT
  AST <--> HUB <--> CLR
  AST <--> HUB <--> JAS
  AST <--> HUB <--> BANK
  AST <--> HUB <--> ISS
  FOR <--> HUB <--> CUS
  OUT <--> HUB <--> TAX
  OUT <--> HUB <--> REG
```

## 2. 中核Business Flow

```mermaid
sequenceDiagram
  participant C as 顧客
  participant O as 注文約定
  participant P as 余力
  participant M as 市場
  participant S as 清算受渡
  participant B as 残高/金銭
  participant T as 税
  participant A as 会計
  participant R as 帳票

  C->>O: 注文
  O->>P: 余力/売却可能数量照会
  P-->>O: 可否・拘束額
  O->>M: 発注
  M-->>O: 約定
  O->>S: 約定情報
  O->>B: 約定反映/拘束更新
  S->>B: 受渡反映
  B->>T: 譲渡/取得/配当/権利イベント
  T->>B: 税徴収/還付
  B->>A: 金銭・証券イベント
  T->>A: 税仕訳イベント
  O->>R: 取引報告データ
  B->>R: 残高/金銭データ
  T->>R: 税/年間取引データ
```

## 3. 設計原則

- 業務イベントをサブシステム間の契約とする。
- 税、会計、帳票は注文画面の派生機能ではなく、独立した業務責務として扱う。
- 権利処理は残高・税・会計・帳票へ横断影響する。
- 外部接続は通信だけでなく、業務ACK/NACK、再送、重複、締切を持つ。
- 年次帳票は日次取引の単純合計ではなく、年度中の訂正/取消/移管/損益通算/税還付を反映する。
