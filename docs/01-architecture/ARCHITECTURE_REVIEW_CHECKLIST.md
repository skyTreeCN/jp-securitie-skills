# Architecture Review Checklist v0.4

**Purpose:** 总体设计确认后再进入各子系统详细化。  
**as-of:** 2026-09-08

## A. 分类

- [x] 子系统 / 交易种别 / 商品没有混在同一层
- [x] 子系统名称代表“业务功能 / Authority”，不是商品名或交易名
- [x] 现物/信用 = Transaction，股票/债券/投信 = Product
- [x] 外证作为日本证券公司跨境业务的例外性横断子系统，边界已明确

## B. 子系统完整性

- [x] v0.3 第一轮Gap Review完成
- [x] SS40 約定照合・決済照合追加
- [x] SS41 取引先・決済条件（SSI）追加
- [x] SS42 交付書面・目論見書・同意追加
- [x] SS24从Matching中分离，限定为残高/资金Reconciliation・Exception・Fail
- [ ] SS01-SS42第二轮Boundary Review完成
- [ ] CS01-CS06第二轮Boundary Review完成
- [ ] 不存在只是因为商品不同而重复建立的系统
- [ ] 不存在独立State/Balance/Legal Evidence但没有Authority的业务功能

## C. 责任边界

重点Review:

- [x] SS09 注文約定 vs SS21 清算
- [x] SS09 Trade vs SS40 Matching
- [x] SS40 約定/決済照合 vs SS24 Reconciliation/Fail
- [x] SS41 SSI/Settlement Reference vs SS22 Transaction Settlement
- [x] SS16 顧客勘定 vs SS17 Cash
- [x] SS12 建玉 vs SS18 証券残高
- [x] SS20 評価簿価 vs SS26 税務取得価額
- [x] SS25 権利 vs SS27 配当利金税
- [x] SS31 会計 vs 原业务Authority
- [x] SS32 对客帐票 vs SS42 取引前交付书面
- [ ] SS32 对客帐票 vs SS33 法定/当局报告最终确认
- [ ] SS24 Exception vs SS38 Workflow最终确认
- [ ] SS34 Compliance vs SS35 AML最终确认
- [ ] CS01 外部接续 vs CS06 内部Data联携最终确认

## D. Data Authority

- [x] 主要Business Object已指定Primary Authority
- [x] Counterparty/SSI Authority = SS41
- [x] Trade/Settlement Match Authority = SS40
- [x] Document Version/Delivery Evidence/Consent Authority = SS42
- [x] Matching状态与Trade事实分离
- [x] Reference SSI与个别Settlement Instruction分离
- [ ] 第二轮检查每个主要Business Object只有一个Primary Authority
- [ ] 外部正本在社内的管理Authority全部明确
- [ ] 派生值保留Source/RuleVersion/BusinessDate
- [ ] 取消/订正/遡及修正保留Lineage

## E. 内部关系

- [x] SS01-SS42/CS01-CS06有总体关系定义
- [x] 订单→约定→勘定→清算→决济→残高链路连续
- [x] 机构交易约定→Matching→SSI→Settlement链路连续
- [x] 权利→税→Cash/证券→会计→帐票链路连续
- [x] 取引前书面→订单可否链路已补足
- [x] 外证/外汇能接回共同勘定/决济/税/会计体系
- [ ] 第二轮检查不存在循环Authority/双向更新冲突

## F. 外部关系

- [x] Market / JPX / PTS
- [x] Clearing / JSCC
- [x] JASDEC 振替制度
- [x] JASDEC 決済照合
- [x] 银行 / 决济银行 / 日银相关
- [x] 发行体 / 信托银行 / 株主名簿管理人
- [x] 国税厅 / 税务署
- [x] FSA / SESC / JSDA
- [x] 信息Vendor
- [x] 他证券公司 / Counterparty
- [x] 投信相关机构
- [x] 日本证券金融等贷借基础设施
- [x] 海外市场 / Custodian / SWIFT
- [ ] J-IRISS明确加入外部关系并映射SS01/SS34
- [ ] e-Tax等税务电子提交Endpoint在SS33/CS01中明确

## G. E2E

- [x] E2E-01 口座开设・契约・书面同意
- [x] E2E-02 国内株现物买
- [x] E2E-03 国内株现物卖
- [x] E2E-04 信用新规→返济
- [x] E2E-05 IPO/PO
- [x] E2E-06 投信
- [x] E2E-07 债券
- [x] E2E-08 Corporate Action
- [x] E2E-09 移管
- [x] E2E-10 外国株
- [x] E2E-11 特定口座年次
- [x] E2E-12 日次締め
- [x] E2E-13 Settlement Fail
- [x] E2E-14 机构投资家约定→决济照合
- [ ] 先物/Option代表E2E补充验证
- [ ] 贷借/Repo代表E2E补充验证

## H. Scope

- [x] Tier A Core / Tier B Conditional / Tier C Adjacent模型建立
- [ ] Tier A最终确认
- [ ] Tier B各条件业务Coverage确认
- [ ] Tier C与Core I/F边界确认
- [ ] 全社Risk/自己资本规制是否维持Adjacent最终确认

## I. Architecture Gate

以下全部满足后才开始Phase 1:

- [ ] 第二轮Boundary Review完成
- [ ] 重大Gap = 0
- [ ] Authority冲突 = 0
- [ ] 未定义关键外部主体 = 0
- [ ] E2E断点 = 0
- [ ] 分类混乱 = 0
- [ ] Scope Tier冻结
- [ ] SS/CS名称和ID冻结
- [ ] 发布Architecture v1.0
