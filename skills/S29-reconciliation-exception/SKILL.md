---
name: reconciliation-exception
description: 市場/保振/銀行/カストディ/社内間照合と不一致管理
maturity: L0
as_of: 2026-09-08
---

# S29 照合・例外管理

## 1. Positioning
市場/保振/銀行/カストディ/社内間照合と不一致管理。

## 2. Scope
- 詳細化予定。`docs/01-architecture/SUBSYSTEM_CATALOG.md` を基準とする。

## 3. Key Business Objects
- TODO: 業務オブジェクトを列挙

## 4. Business Lifecycle
```mermaid
flowchart LR
  I[INPUT] --> P[照合・例外管理] --> O[OUTPUT]
```

## 5. INPUT
- TODO: 項目レベルへ詳細化

## 6. OUTPUT
- TODO: データ/イベント/帳票を項目レベルへ詳細化

## 7. Internal Interfaces
S13-S16,S22-S24,S28,S33,S34

## 8. External Interfaces
JASDEC、JSCC、銀行、カストディ

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
