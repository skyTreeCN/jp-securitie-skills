# Skills taxonomy v0.2

このリポジトリでは、Skillはサブシステム単位で管理する。

- 業務サブシステム: `SSxx`
- 共通系サブシステム: `CSxx`
- 取引種別: `docs/01-architecture/TRANSACTION_CATALOG.md`
- 商品: `docs/01-architecture/PRODUCT_CATALOG.md`

基本式は `サブシステム × 取引種別 × 商品`。

旧`skills/Sxx-*`配下には、v0.1で商品・取引・サブシステムが混在しているため、v0.2構造への移行対象とする。