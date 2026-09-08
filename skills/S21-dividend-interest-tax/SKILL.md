---
name: dividend-interest-tax
description: 配当/利子等の源泉徴収、特定口座受入、外国税関連を管理
maturity: L0
as_of: 2026-09-08
---

# S21 配当・利金税

## 1. Positioning
配当/利子等の源泉徴収、特定口座受入、外国税関連を管理。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[配当・利金税] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
S01,S04,S13,S14,S17,S19,S20,S22,S28,S30,S31

## 8. External Interfaces
国税庁、発行体/信託銀行

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
