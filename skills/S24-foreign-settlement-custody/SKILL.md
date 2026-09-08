---
name: foreign-settlement-custody
description: 外国証券/外貨の決済、SWIFT、カストディ残高照合を管理
maturity: L0
as_of: 2026-09-08
---

# S24 外国決済・カストディ

## 1. Positioning
外国証券/外貨の決済、SWIFT、カストディ残高照合を管理。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[外国決済・カストディ] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
S13,S14,S22,S23,S29,S33

## 8. External Interfaces
SWIFT、グローバルカストディ

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
