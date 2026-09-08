# Skill Index

**Status:** Architecture Gate前 / 詳細Skill展開停止中  
**as-of:** 2026-09-08

現在は総体設計 v0.4 の確定を優先し、個別サブシステムSkillの詳細化を停止している。

## Authority

サブシステム一覧・名称・責務は以下をAuthorityとする。

- `docs/01-architecture/SUBSYSTEM_CATALOG.md`
- `docs/01-architecture/OVERALL_DESIGN.md`
- `docs/01-architecture/SUBSYSTEM_RELATION_MAP.md`
- `docs/01-architecture/DATA_AUTHORITY_MAP.md`

## 現在のSkillディレクトリ

```text
skills/
  subsystems/   # SS01-SS39 のv0.3時点Skeleton。Architecture Freezeまでは参考扱い
  common/       # CS01-CS06 Skeleton
```

v0.4 Gap Reviewで追加した `SS40`, `SS41`, `SS42` のSkill Skeletonは、Architecture v1.0 Freeze後に一括生成する。

## 重要

- `skills/subsystems/` の既存Skeletonより `SUBSYSTEM_CATALOG.md` を優先する。
- 現在の正式論理モデルは **SS01-SS42 + CS01-CS06**。
- `SS24` の正式責務はv0.4で「残高・資金照合／例外・Fail管理」へ変更済み。
- 信用取引/現物取引はTransaction、株式/債券/投信はProductであり、サブシステム名にしない。
- Architecture Gate通過後、このIndexを正式な48サブシステムSkill Indexへ再生成する。

## 別軸

- [取引種別](01-architecture/TRANSACTION_CATALOG.md)
- [商品](01-architecture/PRODUCT_CATALOG.md)
