# INPUT / OUTPUT 記述標準

## 1. INPUT

各INPUTを項目レベルで記述する。

| 項目 | 必須 |
|---|---|
| Input ID | Yes |
| 名称 | Yes |
| 送信元 | Yes |
| 受信先 | Yes |
| Business Trigger | Yes |
| Timing/Cutoff | Yes |
| Format | Yes |
| Business Key | Yes |
| 項目一覧 | Yes |
| Validation | Yes |
| Error処理 | Yes |
| 再送/重複制御 | Yes |
| 出典 | Yes |

## 2. OUTPUT

### データ/イベント
上記INPUTと同等の定義を行う。

### 帳票
帳票は以下を必須とする。

| 項目 | 内容 |
|---|---|
| Report ID | 一意ID |
| 正式名称 | 法定名称を優先 |
| 種別 | 対客/法定/社内 |
| Recipient | 顧客/税務署/当局等 |
| Trigger | 取引/日次/月次/年次/イベント |
| Cutoff | 対象期間・締め |
| Issue Date | 作成/交付/提出期限 |
| Layout Version | 様式版 |
| Page Size | A4等 |
| Layout | Mermaid/ASCIIでセクション構成 |
| Fields | 項目名、型、桁、必須、導出元 |
| Calculation | 合計・税・差引 |
| Source Systems | 元データ |
| Reissue | 再発行/訂正 |
| Legal Source | 法令/様式/記載要領 |
| Current Official Form | 最新公式URL |

## 3. 帳票Layout表現

著作権保護された公式様式を丸ごと複製せず、**システム設計用の論理レイアウト**として再構成する。正式な印字座標/枠線は公式様式を参照する。
