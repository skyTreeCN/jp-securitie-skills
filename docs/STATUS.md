# Project Status

**Version**: Architecture v0.2  
**Date**: 2026-09-08

## 本次修正
v0.1ではサブシステム、取引種別、商品を同一階層に混在させたため分類を破棄し、v0.2へ全面再編した。

## v0.2完了
- 3軸分類確立: サブシステム / 取引種別 / 商品
- 業務サブシステム: SS01-SS39（39）
- 共通系サブシステム: CS01-CS06（6）
- 取引種別カタログ: TR01-TR11
- 商品カタログ: PR01-PR11
- 全体ランドスケープ v0.2
- 旧`skills/Sxx-*`混在構造を削除し、`skills/subsystems/`と`skills/common/`へ再編
- 全45サブシステムSkill骨格作成
- SS26 譲渡益税のv0.1詳細資料を移行用として保持
- INPUT/OUTPUT/帳票標準、出典管理標準を継続利用

## 現在のAuthority
1. `docs/01-architecture/CLASSIFICATION_MODEL.md`
2. `docs/01-architecture/SUBSYSTEM_CATALOG.md`
3. `docs/01-architecture/TRANSACTION_CATALOG.md`
4. `docs/01-architecture/PRODUCT_CATALOG.md`
5. `docs/01-architecture/SYSTEM_LANDSCAPE.md`
6. `docs/SKILL_INDEX.md`

## 次工程
1. SS01-SS39/CS01-CS06の対内・対外関係をI/O単位まで定義
2. 国内株式×現物取引を基準シナリオに中核SkillをL2化
3. SS26 譲渡益税をv0.2 ID体系へ正式移行しL3再検証
4. 帳票一覧と各Layoutを体系化
5. 信用取引はTR02として横断Coverageを追加
6. 債券/投信/デリバティブは商品軸PRxxとして既存サブシステムへRule追加
