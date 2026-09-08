# Architecture Review Checklist v0.3

**Purpose:** 总体设计确认后再进入各子系统详细化。

## A. 分类

- [ ] 子系统/交易种别/商品没有混在同一层
- [ ] 子系统名称代表“业务功能/Authority”，不是商品名或交易名
- [ ] 外证作为例外性业务子系统的边界合理

## B. 子系统完整性

- [ ] SS01-SS39是否覆盖证券公司核心业务
- [ ] CS01-CS06是否覆盖必要共通基盘
- [ ] 是否缺少独立状态/残高/法定责任的业务功能
- [ ] 是否存在只是因为商品不同而重复建立的系统

## C. 责任边界

重点Review:

- [ ] SS09 注文約定 vs SS21 清算
- [ ] SS16 顧客勘定 vs SS17 Cash
- [ ] SS12 建玉 vs SS18 証券残高
- [ ] SS20 評価簿価 vs SS26 税務取得価額
- [ ] SS25 権利 vs SS27 配当利金税
- [ ] SS31 会計 vs 原业务Authority
- [ ] SS32 对客帐票 vs SS33 法定/当局报告
- [ ] SS24 例外管理 vs SS38 Workflow
- [ ] CS01 外部接续 vs CS06 内部Data联携

## D. Data Authority

- [ ] 每个主要Business Object只有一个Primary Authority
- [ ] 外部正本在社内的管理Authority明确
- [ ] 派生值保留Source/RuleVersion/BusinessDate
- [ ] 取消/订正/遡及修正保留Lineage

## E. 内部关系

- [ ] 每个SS都有主要Upstream/Downstream
- [ ] 订单→约定→勘定→清算→决济→残高链路连续
- [ ] 权利→税→Cash/证券→会计→帐票链路连续
- [ ] 外证/外汇能接回共同勘定/决济/税/会计体系

## F. 外部关系

- [ ] Market/PTS
- [ ] Clearing/JSCC
- [ ] JASDEC
- [ ] 银行/决济银行
- [ ] 发行体/信托银行
- [ ] 国税厅/税务署
- [ ] FSA/SESC/JSDA
- [ ] 信息Vendor
- [ ] 他证券公司
- [ ] 投信相关机构
- [ ] 海外市场/Custodian/SWIFT

## G. E2E

- [ ] 口座开设
- [ ] 国内株现物买
- [ ] 国内株现物卖
- [ ] 信用新规→返济
- [ ] IPO/PO
- [ ] 投信
- [ ] 债券
- [ ] Corporate Action
- [ ] 移管
- [ ] 外国株
- [ ] 特定口座年次
- [ ] 日次締め
- [ ] Settlement Fail

## H. Architecture Gate

以下全部满足后才开始Phase 1:

- [ ] 重大Gap = 0
- [ ] Authority冲突 = 0
- [ ] 未定义关键外部主体 = 0
- [ ] E2E断点 = 0
- [ ] 分类混乱 = 0
