# .agents/

AI コーディングエージェント向けの設定を置くディレクトリです。作りは [fullstack-lab](https://github.com/ht-0328/fullstack-lab) の `.agents/` と同じです。

- ページの決まりの正典は [../README.md の「ページの書き方」](../README.md#ページの書き方)。スキルはそれを参照し、手順と確認方法だけを持ちます。
- スキルの正典は [skills/](skills/)。`.claude/skills` は `../.agents/skills` へのシンボリックリンクで、Claude Code はこれ経由で同じスキルを検出します。
- fullstack-lab のルートで作業しているときは Claude Code がこのディレクトリを検出しないため、あちらにある同名の薄いスキルがここの本体を読むように案内します。中身をあちらに写さないでください。

| スキル | 使うとき |
|---|---|
| [writing-architecture-notes](skills/writing-architecture-notes/SKILL.md) | このリポジトリのページ（HTML、SVG、`assets/notes.css`、README、`index.html`）を作る、または直す |
