# S19 INPUT / OUTPUT

## INPUT

| Input ID | 名称 | Source | 主な項目 |
|---|---|---|---|
| TAX-IN-001 | 顧客税属性 | S01/S02/S03 | customer_id, residence, account_type, specified_account_flag, withholding_flag, dividend_acceptance_flag |
| TAX-IN-002 | 銘柄税属性 | S04 | security_id, tax_asset_class, listed/general classification, country, product_type |
| TAX-IN-003 | 買付/取得 | S06/S10/S16/S17 | acquisition_date, qty, acquisition_amount, fees, acquisition_reason |
| TAX-IN-004 | 売却/譲渡 | S06/S08/S10 | trade_id, trade_date, settlement_date, qty, proceeds, fees, disposal_type |
| TAX-IN-005 | 移管 | S16 | from/to account, qty, transferred_cost, transfer_date, basis_method |
| TAX-IN-006 | Corporate Action | S17 | event_type, effective_date, ratio, cash, deemed_dividend, net_asset_reduction_ratio |
| TAX-IN-007 | 配当等 | S21 | payment_date, gross_dividend, income_tax, resident_tax, foreign_tax, eligible_for_specified_account |
| TAX-IN-008 | 税率/制度Version | S05 | effective_from/to, base_income_tax_rate, surtax_components, resident_tax_rate, rounding |
| TAX-IN-009 | 訂正/取消 | Source System | original_event_id, correction_type, corrected_values |

## Core Tax Ledger

最低限の論理データ:

| Field | 説明 |
|---|---|
| customer_id | 顧客 |
| account_id | 課税口座 |
| tax_year | 年分 |
| security_id | 銘柄 |
| tax_asset_class | 上場株式等/一般株式等等 |
| opening_qty | 直前数量 |
| opening_cost | 直前取得価額 |
| acquired_qty | 取得数量 |
| acquired_cost | 取得金額 |
| disposal_qty | 譲渡数量 |
| allocated_cost | 譲渡分取得費 |
| proceeds | 譲渡対価 |
| expenses | 譲渡費用 |
| realized_pl | 譲渡損益 |
| ytd_realized_pl | 年初来損益 |
| taxable_increment | 今回源泉対象増分 |
| income_tax_withheld | 所得税等 |
| resident_tax_withheld | 住民税 |
| refund_income_tax | 還付所得税等 |
| refund_resident_tax | 還付住民税 |
| source_event_id | 原イベント |

## OUTPUT

| Output ID | 名称 | Target | 主な内容 |
|---|---|---|---|
| TAX-OUT-001 | 取得価額更新 | S14 | qty, unit_cost, total_cost, reason |
| TAX-OUT-002 | 譲渡損益 | S30/S32 | proceeds, cost, expenses, realized_pl |
| TAX-OUT-003 | 税徴収 | S13/S28 | tax amount, tax component, value date |
| TAX-OUT-004 | 税還付 | S13/S28 | refund amount, reason, value date |
| TAX-OUT-005 | 年間取引報告データ | S30/S31 | 年次譲渡・税・配当等 |
| TAX-OUT-006 | 税務署提出データ | S31/S33 | 法定調書データ |
| TAX-OUT-007 | 例外 | S29 | missing basis, invalid transfer, rule version error |

## Business Key

`customer_id + account_id + tax_year + source_event_id`

訂正/取消のため、原イベントとの lineage を失わないこと。
