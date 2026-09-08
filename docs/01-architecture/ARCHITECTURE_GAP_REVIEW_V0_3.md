# Architecture Gap Review v0.3

**Status:** Completed review / Proposed v0.4 changes  
**as-of:** 2026-09-08  
**Target:** 日本の総合証券会社向け基幹・バックオフィス（THE STAR クラスをベンチマークとする独自参照モデル）

> 本レビューは公開一次情報と日本証券業務の一般的な実務構造に基づく。NRI THE STAR の非公開内部構成を示すものではない。

---

## 1. Review目的

v0.3 の `SS01-SS39 + CS01-CS06` について、以下を確認した。

1. 商品・取引種別がサブシステムに混在していないか
2. 証券会社の主要End-to-End業務で機能欠落がないか
3. 同一サブシステムに異質な業務ライフサイクルが混在していないか
4. 外部市場インフラとの接続先責務が明確か
5. Data Authorityを一意に置けるか
6. THE STAR級リテール証券バックオフィスに必要な制度対応が抜けていないか

---

## 2. 結論

v0.3 の基本構造は妥当。ただし、以下3機能は独立した論理サブシステムとして追加すべきと判断した。

| Proposed ID | サブシステム | 判定 | 理由 |
|---|---|---|---|
| SS40 | 約定照合・決済照合管理 | **追加** | JASDECの決済照合は、取引当事者間の約定照合と決済当事者間の決済照合を独立した業務ライフサイクルとして持つ。単なる残高照合/例外管理とは異なる。 |
| SS41 | 取引先・決済条件（SSI）管理 | **追加** | 市場参加者、清算/決済機関、銀行、カストディ、決済口座、SSI等のReference Dataが現行Master群に存在しない。決済Instruction生成の前提となる。 |
| SS42 | 交付書面・目論見書・同意管理 | **追加** | 契約締結前交付書面、目論見書、説明書、確認書、電子交付同意等は取引前の法定/業務条件であり、取引後帳票SS32とはライフサイクル・Authorityが異なる。 |

その結果、v0.4候補は以下とする。

- 業務サブシステム: **42**
- 共通系サブシステム: **6**
- 合計論理サブシステム: **48**

---

## 3. SS24の責務修正

### v0.3

`SS24 照合・例外・Fail管理`

この名称では次の2種類が混在する。

1. **外部取引/決済Matching**
   - 約定照合
   - 決済照合
   - SSIを使った決済指図生成
2. **Reconciliation / Exception**
   - 残高照合
   - 資金照合
   - 社内/外部不一致
   - Settlement Fail
   - Break解消

この2つは分離する。

### v0.4

- `SS24 残高・資金照合／例外・Fail管理`
- `SS40 約定照合・決済照合管理`

SS40で不一致となった案件は、必要に応じてSS24のException/Break lifecycleへ引き渡す。

---

## 4. 追加Gap詳細

### GAP-01 取引先・決済条件Master

現行Masterは以下に偏っている。

- SS05 銘柄
- SS06 市場・営業日
- SS07 時価/Rate
- SS08 制度Parameter

しかし実決済には次が必要。

- Counterparty / Legal Entity
- Broker / Custodian / Trust Bank
- Clearing Participant
- JASDEC Participant / Participant Code
- Settlement Bank
- Cash Account / Nostro
- Custody Account
- Standing Settlement Instruction (SSI)
- Market/Settlement Place
- Currency
- Effective Date / Version

これを `SS41` のAuthorityとする。

公開根拠:

- JASDEC SSI機能: https://faq.jasdec.com/faq/show/862
- JASDEC 決済照合: https://faq.jasdec.com/faq/show/861
- NRI I-STAR/SC（SSI情報等属性管理を提供）: https://www.nri.com/jp/service/solution/

---

### GAP-02 約定照合・決済照合

JASDECの決済照合システムでは、

- 証券会社と運用会社等の**約定照合**
- 証券会社と信託銀行等の**決済照合**
- SSIを利用した決済指図自動生成

が明確に別業務として定義されている。

この機能は、残高Reconciliationや決済Fail解消とは分離する。

公開根拠:

- JASDEC 決済照合システム概要: https://faq.jasdec.com/faq/show/854
- JASDEC 決済照合: https://faq.jasdec.com/faq/show/861
- JASDEC SSI: https://faq.jasdec.com/faq/show/862
- NRI I-STAR/MX: NRIサービス一覧に保振決済照合ソリューションとして掲載
- NRI I-STAR/GV: 取引管理に約定照合/市場照合、精算・決済管理に精算指示照合を明示
  - https://www.nri.com/jp/service/solution/i_star_gv.html

---

### GAP-03 交付書面・目論見書・同意

SS32は主に、

- 取引報告書
- 取引残高報告書
- 税務帳票

等の**取引後/期間帳票**を扱う。

一方、以下は取引前または契約時の業務条件である。

- 契約締結前交付書面
- 契約締結時等交付書面
- 目論見書
- 重要情報シート
- 商品/取引説明書
- 確認書
- 電子交付承諾/同意
- 承諾/同意の撤回
- 文書Version/effective period
- 顧客への交付証跡

これらは注文可否、勧誘、適合性、電子交付の証跡に直結するため `SS42` とする。

公開根拠:

- 日本証券業協会「契約締結前交付書面」: https://www.jsda.or.jp/shijyo/seido/jishukisei/words/0086.html
- 日本証券業協会「電磁的方法による交付に関するQ&A」: https://www.jsda.or.jp/shijyo/seido/jishukisei/web-handbook/301_hourei/index.html
- 日本証券業協会「投資信託等の目論見書に関するQ&A」: 同上

---

## 5. 新サブシステムにしないGap

### 5.1 現物取引 / 信用取引

取引種別（TR）として維持する。独立サブシステムにはしない。

### 5.2 株式 / 債券 / 投信 / ETF / デリバティブ

商品（PR）として維持する。独立サブシステムにはしない。

### 5.3 貸借・貸株・Repo

`TR08 貸借取引` として扱い、主に SS09/12/13/14/18/21/22/25 等へ商品・取引別Ruleを追加する。

日本証券金融の貸借取引は、制度信用取引決済に必要な資金・株式を証券会社へ供給する明確な外部業務であるが、「貸借」という取引そのものをシステム分類の第一軸にはしない。

参考: https://www.jsf.co.jp/ja/business/r_taisyaku.html

### 5.4 最良執行 / SOR

新サブシステムにはせず、責務を以下へ分担する。

- SS09: 注文回送/執行先選択/執行結果
- SS34: 最良執行方針・ルール遵守/監視
- SS42: 最良執行方針等の顧客向け交付/Version証跡

金融庁監督指針では最良執行方針、注文回送ルール等が明確な監督対象である。

参考: https://www.fsa.go.jp/common/law/guide/kinyushohin/04a.html

---

## 6. Core外または周辺Systemとする領域

以下は証券会社として必要だが、本リポジトリの「取引基幹/バックオフィス」Authorityの外側に置き、I/Fのみ定義する。

| 領域 | 扱い |
|---|---|
| 全社GL/連結会計 | SS31から連携する外部/全社会計System |
| 自己資本規制比率・会社全体Risk | SS31/SS39/SS43候補ではなく、現段階は周辺Risk Systemとして扱う。Architecture Freeze前に再判定する。 |
| 人事・給与 | Scope外 |
| CRM/営業提案UI | Scope外。SS01/04/37と連携 |
| Web/スマホFront | Scope外。Channelとして扱う |
| 投資一任/Wrap運用Engine | 別業務Platform。必要時PR/TR拡張または周辺Systemとして定義 |
| 引受審査/ECM・DCM | 募集配分SS11の外側にあるInvestment Banking領域。必要時Extension定義 |

---

## 7. v0.4で必須となる責務補強

新SS追加以外に、既存SSへ以下を明記する。

| 対象 | 追加する責務 |
|---|---|
| SS09 注文・約定 | 執行先選択、SOR/最良執行、取引所/PTS routing、約定訂正 |
| SS12 建玉 | 信用/先物/Option/貸借のPosition lifecycleを取引別Ruleで保持 |
| SS13 担保・保証金 | 代用評価、追証、不足、担保差入/返戻、貸借担保 |
| SS18 証券残高 | 顧客/自己、預り区分、拘束、Stock Recordを明確化 |
| SS25 権利 | 顧客意思を伴う権利選択/Election、外国CA連携 |
| SS34 Compliance | 最良執行、適合性、内部者、売買審査、利益相反との境界を明確化 |
| SS36 分別管理 | 顧客分別金信託、有価証券分別、外部監査用Evidence |

---

## 8. Architecture Freeze前の残課題

- [ ] SS40/41/42をSUBSYSTEM_CATALOGへ正式追加
- [ ] SS24の責務をReconciliation/Exceptionへ限定
- [ ] SYSTEM_LANDSCAPEへSS40-42を反映
- [ ] DATA_AUTHORITY_MAPへTradeMatch/SSI/DocumentVersionを追加
- [ ] EXTERNAL_RELATION_MAPへJASDEC決済照合/文書Sourceを反映
- [ ] End-to-End Flowへ「投信購入」「信用新規→返済」「機関投資家約定→決済照合」を追加/再確認
- [ ] Core / Conditional / AdjacentのScope Tierを確定
- [ ] IDはArchitecture Freeze（v1.0）までProvisionalとする

---

## 9. Reviewに用いた主要公開資料

- NRI THE STAR: https://www.nri.com/jp/service/solution/the_star.html
- NRI I-STAR/CORE: https://www.nri.com/jp/service/solution/i_star_core.html
- NRI I-STAR/GV: https://www.nri.com/jp/service/solution/i_star_gv.html
- NRI サービス一覧（I-STAR/MX, I-STAR/SC等）: https://www.nri.com/jp/service/solution/index.html
- JASDEC 決済照合: https://faq.jasdec.com/faq/show/854
- JASDEC SSI: https://faq.jasdec.com/faq/show/862
- JSCC PFMI開示: https://www.jpx.co.jp/jscc/
- 日本証券業協会 自主規制Web Handbook: https://www.jsda.or.jp/shijyo/seido/jishukisei/
- 金融庁 金融商品取引業者等向け総合的監督指針: https://www.fsa.go.jp/common/law/guide/kinyushohin/
- 日本証券金融 貸借取引: https://www.jsf.co.jp/ja/business/r_taisyaku.html
