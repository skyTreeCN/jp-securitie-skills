# Project Status

**Version:** Architecture v0.3  
**Date:** 2026-09-08

## 当前阶段

**Phase 0 — 总体设计 / Architecture Review**

在总体设计完成前，暂停继续扩写单个子系统的详细Skill。现有SS26譲渡益税详细资料仅作为结构样板/既有资产保留。

## 已完成

- 3轴分类: サブシステム / 取引種別 / 商品
- 业务子系统: SS01-SS39（39）
- 共通子系统: CS01-CS06（6）
- 取引種別: TR01-TR11
- 商品: PR01-PR11
- `OVERALL_DESIGN.md` 总体设计Baseline
- `SYSTEM_LANDSCAPE.md` 全体系统图 v0.3
- `SUBSYSTEM_RELATION_MAP.md` 45个系统主要内部关系
- `EXTERNAL_RELATION_MAP.md` 外部主体关系
- `DATA_AUTHORITY_MAP.md` 主要业务对象Authority
- `END_TO_END_FLOW.md` E2E-01〜13代表业务流
- `COVERAGE_MATRIX.md` Subsystem × Transaction × Product Coverage
- INPUT/OUTPUT/帐票标准
- 出典/变更管理标准

## 总体设计Authority

1. `docs/01-architecture/OVERALL_DESIGN.md`
2. `docs/01-architecture/SYSTEM_LANDSCAPE.md`
3. `docs/01-architecture/CLASSIFICATION_MODEL.md`
4. `docs/01-architecture/SUBSYSTEM_CATALOG.md`
5. `docs/01-architecture/SUBSYSTEM_RELATION_MAP.md`
6. `docs/01-architecture/EXTERNAL_RELATION_MAP.md`
7. `docs/01-architecture/DATA_AUTHORITY_MAP.md`
8. `docs/01-architecture/END_TO_END_FLOW.md`
9. `docs/01-architecture/TRANSACTION_CATALOG.md`
10. `docs/01-architecture/PRODUCT_CATALOG.md`
11. `docs/01-architecture/COVERAGE_MATRIX.md`

## 下一步

Architecture Review重点不是马上写Skill，而是检查:

1. 39+6个子系统有没有遗漏
2. 有没有两个系统承担同一Authority
3. 有没有子系统责任过大/过小
4. 内部关系是否缺少重要Data/Event
5. 外部关系是否遗漏日本证券基础设施/监管/业务对手
6. E2E业务流能否完整通过系统图
7. 商品/交易组合是否暴露新的功能Gap

上述问题关闭后，才进入Phase 1，从国内股票现物交易开始逐个详细化子系统。
