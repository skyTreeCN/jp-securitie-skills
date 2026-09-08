# 対内・対外インターフェースマップ

## 外部主体

| External Party | 主な業務 |
|---|---|
| 顧客/営業店/ネット/アプリ | 口座、注文、入出金、照会、帳票 |
| JPX/PTS等 | 注文、約定、市場情報 |
| JSCC等 | 清算、ネッティング、決済 |
| JASDEC | 証券振替、残高、権利、決済関連 |
| 銀行 | 入出金、資金決済 |
| 発行体/信託銀行 | 配当、権利、Corporate Action |
| 国税庁/税務署 | 特定口座/NISA等法定報告 |
| 金融庁/SESC/日証協 | 監督/業界報告 |
| 海外カストディ/SWIFT | 外国証券決済・残高・資金 |
| 情報ベンダー | 銘柄、価格、為替、Corporate Action |

## I/F 必須属性

各I/Fは最低限、次を明記する。

- Business Object / Event
- Sender / Receiver
- Trigger
- Frequency / Cutoff
- Online / Batch
- Protocol / File format
- Business Key
- Idempotency Key
- Field Definition
- Validation
- ACK/NACK
- Retry / Replay
- Duplicate Handling
- Sequence
- Reconciliation
- Error escalation
- Retention
- Security/PII
- Source Authority
