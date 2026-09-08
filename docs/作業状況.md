# Project Status

**Version:** Architecture v1.0  
**Date:** 2026-09-08  
**Status:** FROZEN

## 当前阶段

**Phase 0 — 总体设计：完成**

Architecture Gate通过。总体设计已Freeze，可以进入Phase 1子系统Skill详细化。

## Architecture v1.0 Baseline

- 业务子系统: **SS01-SS42（42）**
- 共通子系统: **CS01-CS06（6）**
- 合计逻辑子系统: **48**
- 取引種別: **TR01-TR11（11）**
- 商品: **PR01-PR11（11）**
- 基本Requirement模型: `Subsystem × Transaction × Product`

正式Freeze记录:

- `docs/01-architecture/ARCHITECTURE_BASELINE_V1_0.md`

## Phase 0完成内容

- 子系统 / 交易 / 商品3轴完全分离
- 第一轮Architecture Gap Review
- 第二轮Subsystem Boundary Review
- Tier B Conditional E2E Validation
- Scope Tier Freeze
- Internal Relation Map
- External Relation Map
- Data Authority Map
- E2E-01〜14代表业务流
- 信用/先物/Option/贷借/外证/募集/机构Matching追加验证
- JPX/PTS、JSCC、JASDEC、银行/日银、日本证券金融、税务、监管、J-IRISS、海外Custody等外部Coverage
- INPUT/OUTPUT/帐票标准
- 出典/变更管理标准

## Architecture Review主要修正

1. 原SS24把Matching与Reconciliation混在一起 → 分离SS40。
2. Counterparty / Settlement Account / SSI Authority缺失 → 新增SS41。
3. 取引前书面/目论见书/Consent Authority缺失 → 新增SS42。
4. SS24正式限定为残高/资金Reconciliation・Exception・Settlement Fail。
5. 现物/信用保持Transaction轴。
6. 股票/债券/投信保持Product轴。
7. 贷借/Repo保持TR08，跨既存SS实现。
8. 最良执行/SOR: SS09执行、SS34规则监控、SS42客户文书。
9. J-IRISS: 外部System；SS01/SS34 + CS01/CS02对应。
10. e-Tax: 外部Endpoint；SS33 + CS01对应。

## Scope Freeze

### Tier A Core
34业务SS + 6共通SS。

THE STAR级日本リテール总合证券Back-office的基本责任。

### Tier B Conditional Core
8业务SS:
- SS11 募集・売出・配分
- SS12 建玉
- SS13 担保・保証金
- SS15 与信・取引Risk
- SS29 外国証券
- SS30 外貨・為替
- SS39 資金繰り
- SS40 約定/決済照合

### Tier C Adjacent
- 全社GL/连结合计
- 自己资本规制/Enterprise Risk
- CRM/营业提案
- Web/App/营业店Front
- Investment Banking
- Wrap/投资一任
- 全社Document Archive
- 人事/給与

## 总体设计Authority

1. `ARCHITECTURE_BASELINE_V1_0.md`
2. `CLASSIFICATION_MODEL.md`
3. `SUBSYSTEM_CATALOG.md`
4. `SYSTEM_LANDSCAPE.md`
5. `OVERALL_DESIGN.md`
6. `SUBSYSTEM_RELATION_MAP.md`
7. `DATA_AUTHORITY_MAP.md`
8. `EXTERNAL_RELATION_MAP.md`
9. `END_TO_END_FLOW.md`
10. `SCOPE_TIER_MODEL.md`
11. `TRANSACTION_CATALOG.md`
12. `PRODUCT_CATALOG.md`
13. `COVERAGE_MATRIX.md`
14. `BOUNDARY_REVIEW_V0_4.md`
15. `CONDITIONAL_E2E_VALIDATION.md`
16. `ARCHITECTURE_REVIEW_CHECKLIST.md`

## 下一阶段 — Phase 1

从**国内股票现物交易**作为基准Scenario开始详细化子系统。

第一批优先Skill:

1. SS01 顾客属性
2. SS02 口座
3. SS03 契约・Service
4. SS05 铭柄
5. SS06 市场・营业日
6. SS08 制度・Parameter
7. SS42 交付书面・目论见书・同意
8. SS34 Compliance・买卖审查
9. SS09 注文・约定
10. SS10 余力
11. SS14 手续费
12. SS16 顾客勘定
13. SS17 资金残高・入出金
14. SS18 证券预り・残高
15. SS21 清算
16. SS22 受渡・决济
17. SS23 保振
18. SS24 Reconciliation/Fail
19. SS25 权利
20. SS26 让渡益税
21. SS27 配当・利金税
22. SS28 NISA
23. SS31 会计
24. SS32 对客帐票
25. SS33 法定帐簿/报告
26. CS01/CS02

每个Skill必须包含业务Know-how、Rule、State、计算、I/O、系统关系、外部关系、Batch/Cutoff、帐票Layout、异常/订正、测试观念和一次出典。
