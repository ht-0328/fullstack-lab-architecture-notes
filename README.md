# fullstack-lab-architecture-notes

仕組みを理解するための図解と解説を置くリポジトリです。題材は学習用のタスク管理アプリ [fullstack-lab](https://github.com/ht-0328/fullstack-lab) で、そこで実際に動いている設定から読み取った構成を、人が読むための HTML にしています。

fullstack-lab の中に置かないのは、あちらの `docs/` が AI コーディングエージェントも読む設計書と設定の置き場で、人が見るための HTML を混ぜたくないためです。fullstack-lab からは、`references/engineering-docs-standard` と同じように git submodule（`references/architecture-notes`）として参照します。

## ページ

| ページ | 内容 | 読み取った時点 |
|---|---|---|
| [auth/authentication-authorization.html](auth/authentication-authorization.html) | 認証・認可の仕組み。Keycloak の 2 つの realm、Nginx と oauth2-proxy による前段認証、BFF での JWT の検証、認可 API の判定、認証 API による会員登録とパスワード変更 | 2026-10-30（fullstack-lab の設計書 0005 の実装後） |

## 見方

GitHub Pages で公開しています: https://ht-0328.github.io/fullstack-lab-architecture-notes/ （main のルートからそのまま配信。`index.html` がページの一覧）。clone して HTML をブラウザで開いても同じです。フォントは Google Fonts から読み込みますが、読めなくても代替フォントで表示できます。

## 更新のきまり

- 各ページは、その時点の fullstack-lab の設定（Nginx、compose、Keycloak の realm、BFF、認可 API、認証 API）から読み取ったものです。設定を変えたら、該当するページも直します。
- 正は fullstack-lab 側の設計書（`docs/design/`）と設定ファイルです。食い違ったときはそちらに合わせます。
- 図は外部のライブラリに頼らず、HTML の中に SVG で書いています。直すときはその SVG を直接編集します。
