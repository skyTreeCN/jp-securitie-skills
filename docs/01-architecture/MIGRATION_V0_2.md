# v0.1 → v0.2 分類修正

v0.1ではサブシステム、取引種別、商品を同じ`Sxx`階層に置いたため分類誤りがあった。

## 廃止する分類

- `margin-trading`をサブシステム扱い
- `domestic-bond`をサブシステム扱い
- `investment-trust`をサブシステム扱い
- `listed-derivatives`をサブシステム扱い

## v0.2

- サブシステム: `SSxx`
- 共通系サブシステム: `CSxx`
- 取引種別: `TRxx`
- 商品: `PRxx`

旧`skills/Sxx-*`は移行完了後に削除する。Authorityは`SUBSYSTEM_CATALOG.md` v0.2とする。