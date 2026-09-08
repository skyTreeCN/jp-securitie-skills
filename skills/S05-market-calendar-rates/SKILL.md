---
name: market-calendar-rates
description: 営業日、市場、受渡日、価格、為替、税率、各種パラメータを管理
maturity: L0
as_of: 2026-09-08
---

# S05 市場・営業日・レート

## 1. Positioning
営業日、市場、受渡日、価格、為替、税率、各種パラメータを管理。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[市場・営業日・レート] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
全取引・決済・税・会計

## 8. External Interfaces
JPX、銀行、情報ベンダー、官庁

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
