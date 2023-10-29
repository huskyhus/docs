# Docker 備忘録

Docker を用いることで、開発環境がコンテナ化（共通化・統一化）されます。これにより以下の恩恵が得られます。

- 軽量・高速な動作
- 環境の配布が可能
- 環境差異によるバグを低減
- CI/CD

従って、Docker は現代のコンテナ開発ワークフローの中核をなす技術であり、デファクトスタンダードになっています。

- [Docker 備忘録](#docker-備忘録)
  - [Docker の基本的な概念](#docker-の基本的な概念)
    - [Docker クライアント](#docker-クライアント)
    - [Docker デーモン](#docker-デーモン)
    - [Docker イメージ](#docker-イメージ)
    - [Docekr コンテナ](#docekr-コンテナ)
    - [コンテナ・ネットワーク](#コンテナネットワーク)
    - [ストレージ](#ストレージ)
  - [Docker クライアントのコマンド(1)](#docker-クライアントのコマンド1)
    - [基本情報](#基本情報)
    - [Docker イメージ操作](#docker-イメージ操作)
    - [Docker コンテナ操作](#docker-コンテナ操作)
    - [メンテナンス](#メンテナンス)
    - [ネットワーク](#ネットワーク)
    - [ボリューム](#ボリューム)
  - [Docker クライアントのコマンド(2)](#docker-クライアントのコマンド2)
    - [docker build](#docker-build)
    - [docker run](#docker-run)
  - [Dockerfile](#dockerfile)
    - [Dockerfile とは](#dockerfile-とは)
    - [.dockerignore](#dockerignore)
  - [Docker compose](#docker-compose)
    - [Docker compose とは](#docker-compose-とは)
    - [compose.yaml](#composeyaml)
    - [docker compose コマンド](#docker-compose-コマンド)
  - [オーケストレーション](#オーケストレーション)

---

## Docker の基本的な概念

### Docker クライアント

Docker クライアントは、Docker の CLI であり、ユーザがさまざまな Docker 操作を行うためのインターフェースを提供します。

### Docker デーモン

Docker デーモンは、Docker の中核を成すバックグラウンドプロセスであり、Docker コンテナの作成、実行、および管理を担当します。Docker デーモンはホストマシン上で実行され、Docker コンテナを操作するための Docker クライアントからの API リクエストを TCP ソケットまたは Unix ソケットを介して受け付け、それに応じてリソースを割り当てます。

具体的には、

- イメージの管理: Docker デーモンは、Docker イメージの取得、ビルド、保存、および削除を管理します。
- コンテナの管理: Docker デーモンは、コンテナの作成、開始、停止、および削除を管理します。
- ネットワーキング: Docker デーモンは、コンテナ間およびコンテナとホストマシン間のネットワーク接続を処理します。
- ストレージ管理: Docker デーモンは、ボリュームの作成、削除、およびマウントを管理します。

### Docker イメージ

Docker イメージは、Docker コンテナの実行に必要なファイルシステム、アプリケーションコード、ランタイム、ライブラリ、環境変数、設定などを含んだ集合体で、Docker コンテナを作成するためのテンプレートと思うことができます。

Docker イメージは`docker build`により、`Dockerfile`から生成することができます（この際、ビルドコンテキストを指定し、`.dockerignore`にビルドコンテキストから除外したいファイルを指定します）。もちろん、Docker hub などのリポジトリに様々な Docker イメージが存在し、それを利用することもできます。

Docker イメージは`docker run`により、Docker コンテナとして起動することができます。

### Docekr コンテナ

Docker コンテナは、アプリケーションやサービスを環境から独立して実行するためのソフトウェアの実行環境です。

### コンテナ・ネットワーク

[Docker コンテナ・ネットワーク](https://docs.docker.jp/engine/userguide/networking/dockernetworks.html)により、コンテナ間の通信を実現できます。デフォルトでは、`bridge`、`host`、`none`の 3 つのコンテナネットワークが提供され、ユーザが定義することもできます。

### ストレージ

Docker コンテナからアクセスできる[ストレージ](https://matsuand.github.io/docs.docker.jp.onthefly/storage/)領域を用いることで、コンテナデータの永続化やコンテナ間でのデータの共有が可能になります。ストレージの概念としては、`volume`、`bind mount`、`tmpfs`があり、以下の特徴を持ちます。

- `volume`: ホスト PC の Docker が管理する領域にデータを配置し、`docker volume`コマンドで管理される。
- `bind mount`: ホスト PC のファイルシステム上にデータを配置する。
- `tmpfs`: メモリ上にデータを配置する。

`volume`のほうが`bind mount`より速く、`tmpfs`と違いデータが永続化されるため、特段の理由がなければ`volume`の使用が推奨されます。

---

## Docker クライアントのコマンド(1)

### 基本情報

- `docker version`: バージョン情報
- `docker info`: 詳細な情報

### Docker イメージ操作

- `docker images`: ローカルイメージの一覧
- `docker build`: イメージのビルド（後述）
- `docker commit`: コンテナのスナップショットからイメージを生成
- `docker rmi`: イメージの削除
- `docker tag`: イメージのタグ付け

### Docker コンテナ操作

- `docker create`: コンテナの作成（起動はしない）
- `docker run`: コンテナの作成＋起動（後述）
- `docker start`: コンテナの起動
- `docker restart`: コンテナの再起動
- `docker stop`: コンテナの停止（正常終了`SIGTERM`）
- `docker kill`: コンテナの停止（強制終了`SIGKILL`）
- `docker pause`, `docker unpause`: コンテナの一時停止・再開
- `docker rm`: コンテナの削除

### メンテナンス

- `docker ps`: 現在のコンテナ群の情報を表示
  - `-a`: 停止したコンテナも含む
  - `docker rm $(docker ps -aq)`: 停止しているコンテナをすべて削除
- `docker inspect`: コンテナまたはイメージの詳細を表示
- `docker logs CONTAINER`: コンテナのログを表示
- `docker exec CONTAINER COMMAND`: コンテナで新しいコマンドを実行

### ネットワーク

- `docker network ls`: ネットワークの一覧を表示
- `docker network inspect NETWORK_NAME`: ネットワークの詳細情報を表示
- `docker network create NETWORK_NAME`: ユーザ定義のブリッジネットワークを作成
- `docker network connect NETWORK_NAME CONTAINER_NAME`: コンテナをネットワークに接続
- `docker network disconnect NETWORK_NAME CONTAINER_NAME`: コンテナをネットワークから切断
- `docker network rm NETWORK_NAME`: ネットワークを削除

デフォルトで定義されている`bridge`ネットワークでは、IP アドレス指定でコンテナ間通信ができます。一方で、ユーザ定義ブリッジネットワークでは、Docker デーモンの組み込み DNS が名前解決を行うため、コンテナ名を用いてコンテナ間通信を行うこともできます。

### ボリューム

- `docker volume ls`: ボリュームの一覧を表示
- `docker volume inspect VOLUME_NAME`: ボリュームの詳細情報を表示
- `docker volume create VOLUME_NAME`: ユーザ定義のボリュームを作成
- `docker volume rm VOLUME_NAME`: ボリュームを削除

---

## Docker クライアントのコマンド(2)

### docker build

[`docker build`](https://docs.docker.jp/engine/reference/commandline/build.html)はイメージを生成するためのコマンドです。

- `-t`: 名前:タグの形式でタグをつける
- `-f`: Dockerfile の指定（デフォルトは`Dockerfile`）
- `--build-arg`: `ARG`の指定
- `--no-cache`: キャッシュを使用せずにビルド

```bash
docker build . -t John/myapp
```

### docker run

`docker run`はコンテナを起動するためのコマンドです。

- `-i`: インタラクティブ実行
- `-t`: TTY を表示（インタラクティブ実行の際、コマンドラインの見た目がわかりやすくなる）
- `-d`: バックグラウンド実行
- `-h`: ホストマシン名を指定
- `--rm`: 終了時にコンテナを自動的に削除（`-d`と併用不可）
- `--name MYNAME`: コンテナに名前を設定
- `-p HOST_PORT:CONTAINER_PORT`: コンテナにポートの指定
- `-v HOST_DIR:CONTAINER_DIR`: コンテナにボリュームの指定（`--mount`のほうが推奨されている？）
- `--network NETWORK_NAME`: 接続するネットワークを指定（デフォルトでは`bridge`だが明示的に指定すると`bridge`には接続しない）

```bash
docker run --rm -it debian /bin/bash
```

---

## Dockerfile

### Dockerfile とは

`Dockerfile` は、Docker イメージに含めるべきファイル、環境設定、コマンド等を指定するための手順書と考えることができます。`Dockerfile` はいくつかの命令から成り、各命令が Docker イメージのレイヤを構成します。

[Dockerfile リファレンス](https://docs.docker.jp/engine/reference/builder.html)

- `FROM`: ベースイメージの指定
- `MAINTAINER`: メンテナの情報の指定
- `ARG`: ビルド時に渡す変数の指定
- `ENV`: 環境変数の指定（これ以降の命令で呼び出せる）
- `WORKDIR`: 作業ディレクトリの指定（これ以降の`RUN`, `CMD`, `COPY`命令などで使用される）
- `COPY`: ビルドコンテキストから Docker イメージへファイルをコピー
- `RUN`: 指定された命令を実行
- `USER`: ユーザ名を指定
- `ENTRYPOINT`: コンテナの起動時に実行される実行可能ファイルの指定（`CMD`命令や`docker run`のパラメータが指定した実行可能ファイルのパラメータとして与えられる）
- `CMD`: コンテナの起動時に指定された命令を実行（`docker run`のパラメータで上書きされる可能性があり、また`ENTRYPOINT`が指定されている場合はそれのパラメータとして用いられる）

```dockerfile
FROM node
WORKDIR /usr/src/app
COPY . /usr/src/app
RUN npm install
CMD "npm" "start"
```

[Docker を使用して Node.js Web アプリケーションをコンテナ化するためのベストプラクティス 10 項目](https://snyk.io/jp/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/)

### .dockerignore

`.dockerignore`では、ビルド時のビルドコンテキストにおいて無視されるべきファイルやディレクトリを指定します。

```
.git
node_modules
```

---

## Docker compose

### Docker compose とは

Docker compose を用いることにより、複数のコンテナをまとめた単位（プロジェクト）を立ち上げることができます。例えば Web アプリケーションにおいて、フロントエンドサーバ、バックエンドサーバ、データベースがあるという状況を考えます。この 3 つを`docker compose`コマンドを使用することで同時に立ち上げることができます。

### compose.yaml

`compose.yaml`（あるいは、`docker-compose.yaml`）では、複数のコンテナがネットワーク、ボリュームを使用しているようなシステムを記述することができます。

- `services`: サービス（コンテナ）名を並べる。`docker run`のオプションに類似する起動オプションを指定する。
- `networks`: ネットワークを指定する。
- `volumes`: ボリュームを指定する。

以下は`compose.yaml`の書式の[例](https://github.com/compose-spec/compose-spec/blob/master/spec.md)です。

```yml
services:
  frontend:
    image: example/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: example/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"

configs:
  httpd-config:
    external: true

secrets:
  server-certificate:
    external: true

networks:
  # The presence of these objects is sufficient to define them
  front-tier: {}
  back-tier: {}
```

### docker compose コマンド

- `docker compose build`: イメージのビルド
  - `--no-cache`: キャッシュを利用しない（Dockerfile の更新時などに必要）
- `docker compose up`: プロジェクトの起動
  - `-d`: バックグラウンドで動作
- `docker compose down`: プロジェクトの終了
  - `-v`: ボリュームも削除
  - `--rmi all`: イメージも削除
- `docker compose ls`: プロジェクトの一覧
- `docker compose ps`: サービスの一覧
- `docker compose logs SERVICE`: ログの表示
  - `--since 10m`: 最近 10 分のログを表示
  - `--tail 10`: 最近 10 行のログを表示

---

## オーケストレーション

さらに Docker デーモンが入っているマシンが複数あり、相互に通信しながら機能するような場合を考えると、オーケストレーションの概念が必要になります。

- `Docker swarm`
- `Kubernetes`（`k8s`）

またそのような場合、典型的には特定のクラウドを利用することになるでしょう。

- `AWS`
- `Azure`
- `GCP`
