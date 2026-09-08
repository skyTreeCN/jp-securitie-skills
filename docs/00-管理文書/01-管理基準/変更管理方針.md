# 変更管理ポリシー

## 変更単位

- 制度改正
- 市場/決済制度改正
- 帳票様式改定
- 内部Know-how追加
- 誤り訂正
- 参照モデル境界変更

## 必須記録

| 項目 | 内容 |
|---|---|
| Change ID | `CHG-YYYY-NNN` |
| 対象Skill | Sxx |
| 旧Rule | 変更前 |
| 新Rule | 変更後 |
| 適用日 | effective_from |
| 根拠 | URL/法令/通知 |
| System Impact | DB/IF/帳票/Batch/Test |
| Backward Impact | 過年度再計算・再発行の有無 |

## 将来施行ルール

将来日付の制度は現行Ruleを上書きせず、別バージョンとして保持する。
