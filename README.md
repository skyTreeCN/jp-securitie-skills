# jp-securitie-skills

日本の証券会社向け基幹システム（NRI「THE STAR」クラスをベンチマーク）の**業務 Know-how / システム Know-how**を、AI Agent が要件定義・設計・テスト・障害分析で再利用できる Skill として体系化するリポジトリです。

> **重要**: 本リポジトリは公開一次情報と独自整理に基づく「日本証券基幹システム参照モデル」です。NRI THE STAR の非公開内部仕様・内部サブシステム名称を再現するものではありません。

## v0.2 分類原則

本リポジトリでは、以下を混在させない。

1. **サブシステム** = 業務機能・責務（例: 顧客属性、注文・約定、余力、譲渡益税、権利、会計）
2. **取引種別** = 何をするか（例: 現物取引、信用取引）
3. **商品** = 何を取引するか（例: 株式、債券、投資信託）

基本的な要件整理単位は:

`サブシステム × 取引種別 × 商品`

例: `注文・約定管理 × 信用取引 × 国内株式`

分類Authority:
- `docs/01-architecture/CLASSIFICATION_MODEL.md`
- `docs/01-architecture/SUBSYSTEM_CATALOG.md`
- `docs/01-architecture/TRANSACTION_CATALOG.md`
- `docs/01-architecture/PRODUCT_CATALOG.md`

## 目的

1. 日本の証券基幹システムに必要な業務サブシステムを漏れなく定義する。
2. 各サブシステムの業務ルール、状態、計算、例外、対内/対外インターフェースを Skill 化する。
3. INPUT / OUTPUT を項目レベルで定義し、帳票についてはレイアウト、項目、出力契機、提出/交付先、法的根拠まで追跡可能にする。
4. 取引種別と商品は別軸で管理し、各サブシステムへの影響をマトリクス化する。
5. 各 Know-how に一次情報の出典と適用時点を付与する。
6. 将来、ChatGPT / Codex / Claude 等が設計・開発・テストで直接利用できる知識基盤にする。

## ディレクトリ

```text
docs/
  00-governance/       # 出典、記述、変更管理ルール
  01-architecture/     # 分類、全体システム構成、関係、I/O標準
skills/
  subsystems/          # SSxx 業務サブシステムSkill
  common/              # CSxx 共通系サブシステムSkill
```

旧`skills/Sxx-*`はv0.1の分類混在があるため移行対象であり、v0.2のAuthorityではありません。

## 参照モデルの根拠

NRI公開情報では THE STAR は証券会社の総合バックオフィス/勘定系であり、口座開設から注文・決済、情報系、コンプライアンス、営業日報、財務会計までを対象としています。

I-STAR/COREの公開説明では、**約定管理、決済管理、証券残高管理、資金残高管理、顧客勘定、会計、対外報告**を機能として列挙し、その次に**現物・先物・オプション、外国証券、債券、貸借**等を取扱対象として別記しています。この区分を本プロジェクトの「サブシステム / 取引 / 商品」分離の重要な公開根拠とします。

- NRI THE STAR: https://www.nri.com/jp/service/solution/the_star.html
- NRI I-STAR/CORE: https://www.nri.com/jp/service/solution/i_star_core.html
- NRI I-STAR/GV: https://www.nri.com/jp/service/solution/i_star_gv.html
- JASDEC: https://www.jasdec.com/rule/

## 原則

- ルールは「結論」だけでなく、適用条件・例外・計算・システム影響まで記述する。
- 法令/制度に関する断定は一次情報を優先する。
- 出典不明の経験則は `INTERNAL-KNOWHOW` と明示し、法令根拠と混同しない。
- 制度改正を前提に `as-of` と改定履歴を持つ。
