# git and GitHub

- [git and GitHub](#git-and-github)
  - [はじめに](#はじめに)
  - [.gitディレクトリ](#gitディレクトリ)
  - [gitコマンド](#gitコマンド)
    - [branch](#branch)
  - [GitHub](#github)

---

## はじめに

gitを適切な方法でインストールする.
Windowsの場合git bashを公式サイトからダウンロードする.
VSCodeにGit GraphやGitLensなどの拡張機能を適宜インストールする.

`git config`を設定する.

- 名前, メールアドレス, エディタ
- SSH接続, GPG署名（[こちら](https://docs.github.com/authentication)を参照）

```bash
git config --global user.name "my name"
git config --global user.email "email@example.com"
git config --global core.editor "C:\...\code --wait"

ssh-keygen -t ed25519 -C "email@example.com"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com

gpg --full-generate-key
gpg --list-secret-keys --keyid-format=long
gpg --armor --export [GPGKeyID]
git config --global user.signingkey [GPGKeyID]
git config --global commit.gpgsign true
git config --global tag.gpgsign true

git config --list
cat ~/.gitconfig
```

## .gitディレクトリ

`.git`ディレクトリにはリポジトリのすべての情報が含まれる.

- **hooksディレクトリ**
  コミットメッセージのバリデーションなど, gitコマンドを実行する際のフックスクリプトを書ける.
- **infoディレクトリ**
  excludeファイルやattributesファイルを使って, `.gitignore`や`.gitattributes`と同じ挙動をさせられる. これらはgit管理下に置かれないため, 個人用の設定として使える.
- **logsディレクトリ**
  参照がこれまでどのように遷移してきたかのログが格納されており, いくつかのgitコマンドで用いられる.
- **objectsディレクトリ**
  gitオブジェクト（blob, tree, commit, tag）が格納されている. ただし, リポジトリがpackされた場合, infoサブディレクトリとpackサブディレクトリにその詳細が記述される.
- **refsディレクトリ**
  参照, すなわちローカルブランチ, リモートブランチや注釈付きのタグが管理されている.
- **config**
  `git config`で指定できるリポジトリのコンフィグ.
- **index**
  ステージングエリアの情報を格納するファイル.
- **HEAD**
  現在チェックアウトしているブランチを指すシンボリック参照. 通常はブランチへの参照だが, `git branch [SHA-1]`などでdetached HEAD状態になるとcommitオブジェクトを指すこともある.

TODO: 他にもMERGE_HEADなど重要な情報があるので後で追加.

## gitコマンド

TODO: gitコマンドを書く

### branch




## GitHub

gitリポジトリを管理することができるホスティングサービス.
はじめにアカウントを作成し, 接続や署名に必要な情報を登録しておく.
リポジトリに対して,

- **Code**
  コード, ブランチ, タグの可視化.
- **Issues**
  TODO, バグ, 新機能の追加などを議論できる.
- **Pull Requests**
  プルリクの管理ができる.
- **Actions**
  デプロイ, CIなどを自動化できる.
- **Project**
  やるべきことリスト, チーム開発ならマネージャーが作るイメージ.
- **Wiki**
  情報共有.
- **Security**
  セキュリティ.
- **Insights**
  統計情報.
- **Settings**
  設定.

といった項目が並んでおり, 開発に役立てることができる.

TODO: Settings/Securityでやることリスト
TODO: Actionsの設定方法（重要）
