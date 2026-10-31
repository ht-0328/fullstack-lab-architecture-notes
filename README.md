# fullstack-lab-architecture-notes

仕組みを理解するための図解と解説を置くリポジトリです。題材は学習用のタスク管理アプリ [fullstack-lab](https://github.com/ht-0328/fullstack-lab) で、そこで実際に動いている設定から読み取った構成を、人が読むための HTML にしています。

fullstack-lab の中に置かないのは、あちらの `docs/` が AI コーディングエージェントも読む設計書と設定の置き場で、人が見るための HTML を混ぜたくないためです。fullstack-lab からは、`references/engineering-docs-standard` と同じように git submodule（`references/architecture-notes`）として参照します。

## ページ

| ページ | 内容 | 読み取った時点 |
|---|---|---|
| [basics/login-basics.html](basics/login-basics.html) | 最初に読むページ。ログインの仕組みの基礎。札（Cookie）、受付（Nginx）、確認係（oauth2-proxy）、窓口（Keycloak）、身分証（JWT）を、ビルに入るときの例えで | 一般的な仕組みの説明（設定に依存しない）。例の値は認証・認可のページと合わせる |
| [auth/authentication-authorization.html](auth/authentication-authorization.html) | 認証・認可の仕組み。Keycloak の 2 つの realm、Nginx と oauth2-proxy による前段認証、BFF での JWT の検証、認可 API の判定、認証 API による会員登録とパスワード変更 | 2026-10-30（fullstack-lab の設計書 0005 の実装後） |
| [aws/aws-architecture.html](aws/aws-architecture.html) | AWS 構成（予定）。ALB の認証（ホストごとのリスナールール）、ECS on Fargate、RDS、IdP（Keycloak / Okta）、CI/CD（IAM ロール、ECR、CodeBuild、CodeArtifact）、ローカルとの対応、未決の点 | 2026-10-31（fullstack-lab の docs/architecture.md と docs/aws-roadmap.md から） |

## 見方

GitHub Pages で公開しています: https://ht-0328.github.io/fullstack-lab-architecture-notes/ （main のルートからそのまま配信。`index.html` がページの一覧）。clone して HTML をブラウザで開いても同じです。フォントは Google Fonts から読み込みますが、読めなくても代替フォントで表示できます。

## ページの書き方

読者は「新しくチームに入った人」と「まだスキルの無い人」です。ぱっと見て全体像がつかめ、読み進めるほど詳しくなる作りにします。ここが決まりの正典で、AI エージェントがページを作る・直すときは、このリポジトリのスキル [.agents/skills/writing-architecture-notes](.agents/skills/writing-architecture-notes/SKILL.md)（手順と確認方法）を使い、そこからここを参照します（仕組みは [.agents/README.md](.agents/README.md)）。

### 1. 最初の 1 画面で結論を言い切る

- 見出し → 3 文以内の導入 → 「この資料の読み方」（3 層のどこまで読めばよいか）→ 層ごとに分けた目次、の順で始めます。
- 01 の節は図 1 枚と 3 行の文で、そのページの結論（例: 「リクエストは 3 つの門を通る」）を言います。ここだけ読めば会話についていける状態にします。

### 2. 3 層に分け、層の順に並べる

| 層 | 読者 | 中身 | 見せ方 |
|---|---|---|---|
| 層 1 ・ 全体像 | 初めて見る人。5 分 | 結論の図、登場人物と役割、まず覚える言葉（8 語まで） | 常に表示。図は登場人物 7 つまで |
| 層 2 ・ 流れ | 担当になった人。20 分 | 「誰を、どの順で通るか」の図と、3〜4 コマの要約 | 図と要約は表示。長い手順の説明は折りたたみ |
| 層 3 ・ 設定と根拠 | 設定を触る人 | 設定値の表、用語の表、ファイルの場所、確かめ方 | 見出しだけ表示し、中身は折りたたみ（`details.deep`） |

層はページの中で混ぜず、層 1 → 2 → 3 の順に並べます。層の切れ目に `layer-head` の区切りを置き、目次も層ごとに分けます。

### 3. 言葉

- 専門用語は初出で一言そえます。例: 「JWT（ログインした人の身分証。Keycloak が発行する）」。
- 層 1 に「まず覚える言葉」を 8 語まで置きます（`glossary`）。
- 比喩はページをまたいで同じにします（「門」= 守りの段、「身分証」= JWT、「入口」= Nginx や ALB）。
- 設定値、URL、ヘッダー名、ファイル名は `code` にして「実際の値」だと分かるようにします。

### 4. 図

- 1 枚の図で言うことは 1 つ。言いたいことが 2 つあれば図を分けます。
- 図の下のキャプションは 1〜2 文で「この図で分かること」を書きます。
- 色は意味を表し、全ページで同じにします（`assets/notes.css` の変数）。

| 色（変数） | 意味 |
|---|---|
| 緑 `--c` | 一般利用者の経路 |
| 紫 `--s` | 社員の経路 |
| 橙 `--k` | IdP（Keycloak / Okta）とのやり取り |
| 黄 `--jwt` | トークン（JWT）が運ばれる線 |
| 青 `--z` | 認可 API（判定） |
| 黄緑 `--au` | 認証 API（ユーザーの作成・変更） |
| 緑 `--ok` / 赤 `--ng` | 許可 / 拒否 |

- アイコンは役割を表し、同じ役割には全ページで同じアイコンを使います（一覧は認証・認可ページの凡例）。出典は「更新のきまり」のとおり。
- 図は HTML 内の SVG（`viewBox`）で描き、色や文字サイズは SVG に直書きせず `notes.css` のクラス（`t11` `tm` `ln c` `box k` など）を使います。文字は 11px 以上、層 1 の図は 12px 以上。
- スマホでは図が枠内で横スクロールします。縮めて読めなくするより、スクロールさせます。

### 5. 文章

- 1 段落は 3 文まで。1 行は 40 文字前後（CSS が `p` の幅を絞ります）。
- 並ぶものは箇条書き、順序があるものは番号つき、数や対応は表にします。
- 「誰が、何を、どうする」の順で書きます。主語を省きません。

### 6. 共通 CSS と部品

- 見た目は `assets/notes.css` に集めてあり、各ページは `<link rel="stylesheet" href="../assets/notes.css">` で読みます。そのページだけの装飾は `<style>` に数行だけ残します。`notes.css` を変えたら全ページを描画して確かめます。
- 使える部品: `howto`（読み方）、`layer-head`（層の区切り）、`nav.toc .group`（層ごとの目次）、`details.deep`（折りたたみ）、`glossary`（言葉）、`strip`（3〜4 コマの要約）、`legend`（アイコンの凡例）、`cards` / `card`（横並びのカード）、`callout`（補足の囲み）、`ol.steps`（手順）、`tbl-wrap`（表）、`bar` / `persona`（視点の切り替え）。
- 新しいページは既存のページ（認証・認可）を型にして、同じ順序・同じ部品で作ります。

### 7. 事実と時点

- 各ページは fullstack-lab のその時点の設定から読み取ったものです。「読み取った時点」をフッター、この README の表、`index.html` に書き、設定を変えたら更新します。
- 正は fullstack-lab 側の設計書と設定ファイルです。食い違ったときはそちらに合わせます。未決や仮定は「未決」「仮定」と書き、事実のようには書きません。

## 更新のきまり

- 各ページは、その時点の fullstack-lab の設定（Nginx、compose、Keycloak の realm、BFF、認可 API、認証 API）から読み取ったものです。設定を変えたら、該当するページも直します。`basics/` の基礎ページだけは一般的な仕組みの説明で、設定が変わっても中身は変わりません（例として挙げる名前と値だけ合わせます）。
- 正は fullstack-lab 側の設計書（`docs/design/`）と設定ファイルです。食い違ったときはそちらに合わせます。
- 図は外部のライブラリに頼らず、HTML の中に SVG で書いています。直すときはその SVG を直接編集します。見た目は `assets/notes.css` に集めてあります（「ページの書き方」）。
- AWS のサービスとグループ枠のアイコンは [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)（2026-07-31 版のパッケージ、2026-10-31 に取得）から、使うものだけを `assets/aws-icons/` に改変せず置き、SVG の `<image href>` で参照しています。利用条件は配布ページの記載に従います。AWS のページでは、Keycloak、Okta、GitHub など AWS 以外のものは枠だけで描きます。
- 認証・認可のページでは、仕組みの部品（利用者、Cookie、鍵、錠前、JWT、判定、データ）を [Font Awesome Free](https://fontawesome.com/) 6.7.2（アイコンは CC BY 4.0）から、製品のロゴ（Keycloak、NGINX、Nuxt）を [Simple Icons](https://simpleicons.org/) 16.36.0（CC0 1.0）から取っています。色を CSS で経路ごとに変えるため、ファイルは置かず、SVG のパスを HTML の `<body>` 先頭の `<symbol>` に取り込んで `<use>` で参照します。使ったアイコンの一覧と出典はその `<symbol>` の直前のコメントにあり、同じ意味には同じアイコンを使います（凡例は「登場人物と役割」の節）。ロゴの扱いは各ブランドのガイドラインに従います。
