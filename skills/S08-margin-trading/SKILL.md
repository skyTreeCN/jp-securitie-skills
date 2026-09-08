---
name: margin-trading
description: 信用新規/返済、建玉、保証金、期日、品貸料等を管理
maturity: L0
as_of: 2026-09-08
---

# S08 信用取引

## 1. Positioning
信用新規/返済、建玉、保証金、期日、品貸料等を管理。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[信用取引] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
S06,S07,S13,S14,S15,S17,S18,S19,S25

## 8. External Interfaces
取引所/清算機関等

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
