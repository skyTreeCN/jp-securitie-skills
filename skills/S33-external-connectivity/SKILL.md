---
name: external-connectivity
description: 市場、保振、清算、銀行、税務、カストディ等との通信/変換/送受信
maturity: L0
as_of: 2026-09-08
---

# S33 外部接続

## 1. Positioning
市場、保振、清算、銀行、税務、カストディ等との通信/変換/送受信。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[外部接続] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
全サブシステム

## 8. External Interfaces
JPX、JSCC、JASDEC、銀行、国税庁、SWIFT等

## 9. Business Rules
- TODO: Rule ID + 適用条件 + 例外 + 出典

## 10. Reports
- TODO

## 11. Batch & Timing
- TODO

## 12. Test Viewpoints
- TODO

## 13. Sources
- TODO: 一次情報優先

## 14. Maturity
`L0 Skeleton`。今後L2/L3へ詳細化する。
