# Corporate Action と譲渡益税

権利の業務Authorityは S17。S19 は税務上の取得価額・譲渡所得への影響を受け持つ。

| Event | 主な税務影響 | S19処理 |
|---|---|---|
| 株式分割 | 1単位当たり取得価額を調整 | 数量増加、総取得価額維持を基本にRule適用 |
| 株式併合 | 1単位当たり取得価額を調整 | 数量減少、端数処理を別管理 |
| 株主割当 | 取得費調整が必要となる場合 | 払込/無償等条件でRule分岐 |
| 無償交付等 | 一定ケースで取得価額0 | 法令条件によりbasis設定 |
| 合併/株式交換 | 税務上譲渡とみなさないケース/取得価額引継ぎ等 | event subtypeでRule分岐 |
| 資本払戻し | みなし配当 + 譲渡収入 + 残存株取得価額調整 | 純資産減少割合をINPUTとして計算 |
| TOB | 通常の譲渡/口座外扱い等を確認 | 取扱形態ごとに分類 |
| 上場廃止/価値喪失 | 特定管理株式等の特例可能性 | 別Ruleで管理 |

## 株式分割/併合

国税庁 No.1464 は株式等の分割・併合等で1単位当たり取得価額を調整することを示す。

Source:
https://www.nta.go.jp/taxes/shiraberu/taxanswer/shotoku/1464.htm

## 資本払戻し

国税庁の法令解釈では概念上、

- 譲渡所得等の収入金額とみなされる額 = 交付金銭等 - みなし配当額
- 控除すべき取得価額 = 旧株従前取得価額合計 × 純資産減少割合
- 払戻し後1株当たり取得価額も純資産減少割合により調整

Source:
https://www.nta.go.jp/law/joho-zeikaishaku/shotoku/joto-sanrin/070405/08.htm

## 設計上の重要点

Corporate Action入力は「イベント名」だけでは不足。少なくとも次を保持する。

- tax_event_type
- effective_date
- record_date
- ratio
- cash_component
- stock_component
- deemed_dividend_amount
- net_asset_reduction_ratio
- old_security_id / new_security_id
- old_qty / new_qty
- fractional_share_cash
- official_notice_reference
