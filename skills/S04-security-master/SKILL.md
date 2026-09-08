---
name: security-master
description: 銘柄属性、上場区分、商品分類、税区分、権利属性等を管理
maturity: L0
as_of: 2026-09-08
---

# S04 銘柄・商品マスター

## 1. Positioning
銘柄属性、上場区分、商品分類、税区分、権利属性等を管理。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[銘柄・商品マスター] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
S06-S31の大半

## 8. External Interfaces
JPX、情報ベンダー、発行体等

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
