# jp-securitie-skills

日本の証券会社向け基幹システム（NRI「THE STAR」クラスをベンチマーク）の**業務 Know-how / システム Know-how**を、AI Agent が要件定義・設計・テスト・障害分析で再利用できる Skill として体系化するリポジトリです。

> **重要**: 本リポジトリは公開一次情報と独自整理に基づく「日本証券基幹システム参照モデル」です。NRI THE STAR の非公開内部仕様・内部サブシステム名称を再現するものではありません。

## 目的

1. 日本の証券基幹システムに必要な業務領域・サブシステムを漏れなく定義する。
2. 各サブシステムの業務ルール、状態、計算、例外、対内/対外インターフェースを Skill 化する。
3. INPUT / OUTPUT を項目レベルで定義し、帳票についてはレイアウト、項目、出力契機、提出/交付先、法的根拠まで追跡可能にする。
4. 各 Know-how に一次情報の出典と適用時点を付与する。
5. 将来、ChatGPT / Codex / Claude 等が設計・開発・テストで直接利用できる知識基盤にする。

## ディレクトリ

```text
docs/
  00-governance/       # 出典、記述、変更管理ルール
  01-architecture/     # 全体システム構成、関係、I/O標準
skills/
  _template/           # Skill標準テンプレート
  S01-.../             # サブシステム別Skill
  ...
  S19-capital-gains-tax/ # 譲渡益税の詳細サンプル
```

## 参照モデルの根拠

NRI公開情報では THE STAR は証券会社の総合バックオフィス/勘定系であり、口座開設から注文・決済、情報系、コンプライアンス、営業日報、財務会計までを対象としています。I-STAR/GV の公開機能分類（マスター、取引、精算・決済、残高、コーポレートアクション、会計、外部接続等）も、バックオフィス参照モデルの補助資料として使用します。

- NRI THE STAR: https://www.nri.com/jp/service/solution/the_star.html
- NRI I-STAR/GV: https://www.nri.com/jp/service/solution/i_star_gv.html
- NRI I-STAR/GX: https://www.nri.com/jp/service/solution/i_star_gx.html

## Phase

- **Phase 1**: 全体参照モデル、サブシステムカタログ、関係図、Skill標準
- **Phase 2**: 国内株式中核（注文約定、余力、残高、受渡、権利、譲渡益税、NISA、対客帳票）
- **Phase 3**: 投信、債券、信用、外国証券、外貨、会計、法定報告
- **Phase 4**: 実案件の設計書・QA・障害票・テスト資産から内部 Know-how を追加

## 原則

- ルールは「結論」だけでなく、適用条件・例外・計算・システム影響まで記述する。
- 法令/制度に関する断定は一次情報を優先する。
- 出典不明の経験則は `INTERNAL-KNOWHOW` と明示し、法令根拠と混同しない。
- 制度改正を前提に `as-of` と改定履歴を持つ。
