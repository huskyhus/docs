# TypeScript

TypeScript は、Microsoft によって開発されたオープンソースのプログラミング言語です。JavaScript の構文と機能を拡張したもので、静的型付けをサポートしています。静的型付けにより、コード内のエラーを事前に検出しやすくし、大規模なプロジェクトでの開発を容易にします。
TypeScript のコードは、開発者がソースコードを書く段階で型チェックされますが、コンパイルされる際に JavaScript に変換されます。これにより、TypeScript で書かれたコードはすべて、標準の JavaScript ランタイムで実行することができます。

- [TypeScript](#typescript)
  - [開発環境](#開発環境)
    - [Node.js](#nodejs)
    - [nvm](#nvm)
    - [npm](#npm)
    - [npx](#npx)
    - [package.json](#packagejson)
    - [package-lock.json](#package-lockjson)
    - [node_modules](#node_modules)
    - [tsc](#tsc)
    - [tsconfig.json](#tsconfigjson)
    - [tsconfig.json の target オプション](#tsconfigjson-の-target-オプション)
    - [tsconfig.json の lib オプション](#tsconfigjson-の-lib-オプション)
    - [tsconfig.json の module オプション](#tsconfigjson-の-module-オプション)
    - [.ts ファイル](#ts-ファイル)
    - [.d.ts ファイル](#dts-ファイル)
    - [JSX](#jsx)
    - [.tsx ファイル](#tsx-ファイル)
    - [ESLint](#eslint)
    - [Prettier](#prettier)
    - [Node.js 最小環境構築手順](#nodejs-最小環境構築手順)
    - [Node.js 推奨環境構築手順](#nodejs-推奨環境構築手順)
  - [ライブラリ・フレームワーク紹介](#ライブラリフレームワーク紹介)
    - [フロントエンド開発](#フロントエンド開発)
    - [バックエンド開発](#バックエンド開発)
    - [ORM](#orm)
    - [テスト](#テスト)
  - [言語仕様](#言語仕様)

---

## 開発環境

### Node.js

Node.js は、Chrome の V8 JavaScript ランタイムに基づいて構築されたオープンソースのランタイム環境です。これにより、JavaScript を使用して Web サーバーやネットワークアプリケーションを構築することができます。
Node.js の特徴の一つは、非同期処理によって I/O の待ち時間を最小限に抑え、効率的なスケーラビリティを提供することです。これにより、リアルタイムアプリケーションや高負荷なネットワークアプリケーションの開発に適しています。

### nvm

nvm は、Node.js のバージョン管理ツールで、複数の Node.js バージョンをインストール、管理、切り替えるために使用されます。nvm は Node Version Manager の略で、Node.js のバージョンを簡単に切り替えることができるため、異なるプロジェクト間で異なる Node.js のバージョンを使い分ける必要がある場合に便利です。
nvm を使用すると、コマンドラインから簡単に Node.js の特定バージョンをインストールし、アクティブなバージョンを切り替えることができます。これにより、特定のプロジェクトが要求する Node.js のバージョンに対応することができます。また、nvm は Linux、macOS、Windows などのさまざまなオペレーティングシステムで利用可能です。

### npm

npm（Node Package Manager）は、Node.js のデフォルトのパッケージ管理システムです。npm は、Node.js パッケージのインストール、公開、管理を行うためのコマンドラインユーティリティおよびオンラインデータベースの役割を果たしています。開発者は npm を使用して、プロジェクトに必要なモジュールやライブラリを簡単にインストールし、プロジェクトの依存関係を管理することができます。
npm は、パッケージのインストール、アンインストール、更新、検索、公開、管理などのさまざまな機能を提供します。さらに、npm はパッケージの依存関係を自動的に管理し、パッケージのバージョンを解決することができます。これにより、開発者は効率的にアプリケーションを開発し、再利用可能なコードやライブラリを簡単に統合することができます。
npm は、Node.js コミュニティで広く使用されており、数多くのモジュールやパッケージが npm のデータベースで公開されています。これにより、開発者は自分のプロジェクトに必要な機能や機能拡張を簡単に見つけて導入することができます。

なお、現在`npm`の他に`yarn`や`pnpm`等のパッケージマネージャがある。

### npx

npx は、npm パッケージを実行するためのコマンドラインツールです。npx を使うと、単にローカルに存在するパッケージを実行する他に、npm レジストリからパッケージを一時的にダウンロードして実行することもできます。

### package.json

package.json は、Node.js プロジェクトやパッケージの設定やメタデータを記述するためのファイルです。通常、プロジェクトのルートディレクトリに配置されます。このファイルには、プロジェクトの名前、バージョン、依存関係、スクリプトなどの情報が含まれています。

package.json ファイルには、以下のような情報が含まれています。

- `name`: パッケージの名前
- `version`: パッケージのバージョン
- `description`: パッケージの簡単な説明
- `main`: エントリーポイントとなるメインの JavaScript ファイル
- `scripts`: プロジェクトで実行可能なスクリプトの一覧
- `dependencies`: 依存する外部パッケージの一覧
- `devDependencies`: 開発時にのみ必要な外部パッケージの一覧
- `author`: パッケージの作者
- `license`: パッケージのライセンス情報

package.json ファイルは、npm によってパッケージの依存関係を管理するために使用されます。このファイルに記述された依存関係は、npm install コマンドを使用して一括でインストールすることができます。また、プロジェクトの設定やスクリプトなどのカスタム設定も、このファイルに記述されます。

### package-lock.json

package-lock.json は、npm によって生成されるファイルで、プロジェクトの依存関係を固定するためのものです。このファイルは、プロジェクトで使用されている各パッケージの特定のバージョンや、その依存関係に関する情報を含んでいます。この情報は、npm が再現可能な状態でパッケージをインストールするのに役立ちます。
package-lock.json ファイルは、プロジェクトで使用されている各パッケージの正確なバージョン、依存関係の解決結果、およびインストールプロセスで生成される特定のファイル構造を記録します。これにより、開発者は同じ環境での再現可能性を確保し、開発、テスト、およびプロダクション環境での一貫性を維持することができます。

### node_modules

node_modules は、Node.js プロジェクトにおける依存関係のパッケージがインストールされるディレクトリです。このディレクトリには、プロジェクトが依存する外部ライブラリやモジュールが含まれています。通常、開発者は npm、yarn、pnpm などのパッケージマネージャーを使用して、プロジェクトの依存関係を管理し、node_modules ディレクトリにパッケージをインストールします。
node_modules ディレクトリには、プロジェクトが使用するパッケージのすべてのファイルや依存関係が含まれています。これには、JavaScript ファイル、画像、CSS、およびその他のアセットファイルが含まれます。これらのパッケージは、プロジェクトの実行時に必要となるため、プロジェクトを実行するためにはこのディレクトリが必要となります。
node_modules ディレクトリは、通常、バージョン管理システム（Git など）に含められません。代わりに、package.json と package-lock.json（または yarn.lock など）にはプロジェクトの依存関係が記録され、他の開発者が同じ環境でプロジェクトを再現できるようになります。これにより、プロジェクトのサイズが大幅に増加するのを防ぎ、バージョン管理の効率を高めることができます。

### tsc

tsc は、TypeScript コンパイラのコマンドラインツールです。tsc コマンドは、TypeScript ファイル（拡張子が.ts のファイル）を JavaScript ファイル（拡張子が.js のファイル）に変換するために使用されます。このコマンドを使用すると、TypeScript プロジェクトをコンパイルして JavaScript に変換することができます。

TypeScript プロジェクトをコンパイルするために、以下のように tsc コマンドを使用します。

```bash
tsc yourfile.ts
```

このコマンドを実行すると、TypeScript ファイルが JavaScript ファイルにコンパイルされます。また、tsconfig.json ファイルがプロジェクトのルートディレクトリに存在する場合、tsc コマンドはその設定に従ってプロジェクトをコンパイルします。tsconfig.json ファイルにはコンパイラの設定が含まれており、tsc コマンドはこれらの設定を参照してコンパイルを行います。

### tsconfig.json

tsconfig.json は、TypeScript プロジェクトのコンパイラ設定を定義するためのファイルです。このファイルは、TypeScript コンパイラがプロジェクトをビルドする際の動作をカスタマイズするために使用されます。tsconfig.json ファイルには、プロジェクトのコンパイルオプションやファイルの含め方などの設定が含まれています。
tsconfig.json ファイルを使用すると、開発者はプロジェクト固有のニーズに合わせて TypeScript コンパイラを設定することができます。これには、コンパイルオプション、ファイルの含め方、出力ファイルの場所などが含まれます。また、tsconfig.json ファイルを使用することで、複数のファイルで共通のコンパイラ設定を共有し、一貫したビルドプロセスを確保することができます。

以下は、tsconfig.json ファイルの例です。

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "./dist",
    "strict": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### tsconfig.json の target オプション

tsconfig.json ファイルの"target"オプションは、TypeScript コンパイラが生成する JavaScript コードの ECMAScript ターゲットバージョンを指定するための設定です。このオプションには、コンパイルされた JavaScript コードが準拠すべき ECMAScript のバージョンを指定します。
"target"オプションには、一般的な ECMAScript バージョン（ES3、ES5、ES6/ES2015、ES2016、ES2017 など）や、より新しいバージョン（ES2018、ES2019、ES2020 など）が設定できます。選択したターゲットバージョンによって、生成される JavaScript コードの構文や機能が変わります。
"target"オプションを適切に設定することで、ターゲット環境に最適化された JavaScript コードを生成することができます。ただし、ターゲットバージョンをより新しいものに設定する場合は、実行環境がその機能をサポートしていることを確認する必要があります。

おそらく、一般に`es5`を target とすることは間違った選択肢ではないと言えます。

### tsconfig.json の lib オプション

tsconfig.json ファイルの"lib"オプションは、TypeScript コンパイラが使用するライブラリのリストを指定するための設定です。このオプションによって、コンパイラがコンパイル時に利用できる（すなわち、JavaScript コードが実行される環境において利用可能であると仮定してよい） JavaScript の組み込みライブラリや DOM の定義などを指定することができます。
"lib"オプションには、コンパイル時に利用可能な組み込みライブラリの名前が含まれます。例えば、"DOM"、"ES5"、"ES6"、"ES2015"、"ES2016"、"ES2017"などがあります。
"lib"オプションを適切に設定することで、コンパイラが必要な型定義を正しく解釈し、コードの型チェックや補完を行うことができます。たとえば、"DOM"ライブラリを指定すると、コンパイラはブラウザの DOM API に関連する型定義を利用してコードを解析します。
プロジェクトの要件に応じて、適切な"lib"オプションを選択して設定することが重要です。これにより、コンパイラが必要なライブラリを正しく解釈し、コードの互換性と正確性を確保することができます。

### tsconfig.json の module オプション

tsconfig.json ファイルの"module"オプションは、TypeScript コンパイラがどのモジュールシステムに対してコンパイルするかを指定するための設定です。一般的な"module"オプションの値には、"commonjs"、"amd"、"system"、"umd"、"es6"などがあります。
このオプションは、ライブラリを npm 上に公開するか自分だけで使うか、Node.js 上で使うかブラウザで実行するか、ブラウザで実行する場合どのバンドラを使うかに応じて行うべき選択が多岐に渡るため複雑です。

おそらく、一般に

- Node.js 上で実行: `commonjs`
- ブラウザ上で実行: `create-react-app`や`create-next-app`がデフォルトで与える設定

に従うことは間違った選択ではないと言えます。

### .ts ファイル

TypeScript ファイル（.ts ファイル）には、JavaScript と同様に、変数、関数、クラス、インターフェースなどの要素が含まれています。また、JavaScript の機能に加えて、静的型付けや型注釈などの TypeScript 固有の機能が含まれることもあります。
TypeScript ファイルは、TypeScript コンパイラ（tsc）を使用して JavaScript にコンパイルすることができます。このプロセスにより、TypeScript で書かれたコードは JavaScript に変換され、標準の JavaScript ランタイムで実行することができます。

### .d.ts ファイル

.d.ts ファイルは、TypeScript の型定義ファイルの拡張子です。これらのファイルは、JavaScript で書かれたライブラリやモジュールの型情報を提供するために使用されます。TypeScript では、型情報を持たない JavaScript のライブラリや外部モジュールを使用する際に、.d.ts ファイルを定義してそのライブラリの型情報を提供します。
.d.ts ファイルは、通常、外部ライブラリやモジュールの型情報を手動で記述することで作成されます。これにより、TypeScript コンパイラはそれらのライブラリやモジュールの型情報を理解し、コードの型チェックや補完を行うことができます。一部のライブラリやフレームワークは、.d.ts ファイルを提供して公式に TypeScript のサポートを提供しています。
.d.ts ファイルは、TypeScript の静的型付け機能を活用して、JavaScript のライブラリやモジュールをより安全かつ効果的に使用するのに役立ちます。これにより、開発者は外部ライブラリやモジュールとの統合をスムーズに行い、より信頼性の高いコードを開発することができます。

### JSX

JSX（JavaScript XML）は、JavaScript の構文拡張で、JavaScript ファイル内に HTML のようなコードを記述できるようにするものです。JSX の構文は HTML に似ており、開閉タグ、属性、入れ子要素などがあります。

たとえば、次のような JSX コードを書いて、単純な見出し要素を表示することができます。

```jsx
const heading = <h1>Hello, JSX!</h1>;
```

### .tsx ファイル

.tsx ファイルは、TypeScript を使用して書かれた JSX（JavaScript XML）を含むファイルの拡張子です。.tsx ファイルを使用することで、TypeScript は JSX を正しく解析し、コンポーネントの型チェックを行うことができます。

### ESLint

ESLint は、JavaScript および TypeScript の静的コード解析ツールであり、コード内の潜在的な問題やエラーを特定し、開発者が一貫したコーディングスタイルとベストプラクティスを遵守するのを支援します。ESLint は、カスタマイズ可能なルールセットを使用してコードを検証し、問題がある箇所を警告やエラーとして報告します。
ESLint を使用すると、開発者はコードの品質を向上させ、バグを事前に検出することができます。ESLint は様々なカスタマイズオプションを提供し、開発者がプロジェクトのニーズに合わせてルールをカスタマイズすることができます。さらに、ESLint はプラグインや拡張機能をサポートしており、さまざまなプロジェクトで利用できる幅広い機能を提供しています。
ESLint は主にコマンドラインやビルドツールと統合して使用されます。開発者は、ESLint をプロジェクトに導入し、コードベース全体でコーディングスタイルの一貫性を確保したり、コードの品質を維持したりするために利用します。ESLint は、コードベースの可読性や保守性を向上させるために広く使用されています。

### Prettier

Prettier は、コードのフォーマットを自動化するためのツールであり、主に JavaScript や TypeScript などのプログラミング言語で使用されます。Prettier は、コードのレイアウトやスタイルを自動的に整形し、一貫性のあるフォーマットを適用することで、コードの見た目を統一します。
Prettier は、コードのインデント、スペース、改行、カンマの位置などのスタイルを自動的に調整します。これにより、開発者は手動でコードを整形する手間を省くことができます。Prettier は多くのプロジェクトで使用されており、コードベース全体の一貫性を保ち、コードレビューの手間を軽減するのに役立ちます。
Prettier は、コマンドラインから直接実行することもできますし、多くの統合開発環境（IDE）やテキストエディターのプラグインとしても利用できます。また、Prettier はカスタマイズ可能であり、使用するプロジェクトのニーズに合わせて設定を調整することができます。

### Node.js 最小環境構築手順

Node.js を、公式サイトの通りにインストールする。（この際、自動的に npm もインストールされる。）バージョンを以下のように確認する。

```bash
node -v
npm -v
```

プロジェクトを作成する。慣例に従い、`src`フォルダを作成してソースファイルはそこに置く。

```bash
mkdir helloworld
cd helloworld
mkdir src

npm init -y
npm i -D typescript @types/node
npx tsc --init

touch src/index.ts
```

`package.json`に以下を追加。

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node ./dist/index.js",
    "dev": "tsc --watch"
  }
}
```

`tsconfig.json`に以下を追加。

```json
{
  "compilerOptions": {
    "outDir": "./dist"
  },
  "include": ["./src/**/*.ts"]
}
```

- 開発時は`npm run dev`で dev サーバを立ち上げる。
- 実行したいときは`npm start`を実行、エントリポイントは`./dist/index.js`
- 手動でビルドするために`npm run build`も可能。

### Node.js 推奨環境構築手順

先ほどの最小環境よりも良い開発体験のため、

- `eslint`
- `prettier`
- `husky`
- `lint-staged`

を導入する手順を紹介する。

まず、ESLint, Prettier を設定する。対話的な設定は自然なものを選ぶ。（この際、自動的に必要な devDependencies がインストールされる。）

```bash
npm i -D eslint prettier eslint-config-prettier
npx eslint --init
```

`.eslintrc.json`が作成されるが、自分の都合のいいように書き換える。この時、`extends`の最後尾に`"prettier"`を付加する。一例では、

```json
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "rules": {
    "indent": ["error", 2],
    "linebreak-style": ["error", "windows"],
    "quotes": ["error", "double"],
    "semi": ["error", "always"]
  },
  "ignorePatterns": ["dist/**/*.js", "dist/**/*.jsx"]
}
```

また、`.prettierrc.json`を作成し、自分の都合のいいように書く。一例では、

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "semi": true,
  "singleQuote": false,
  "trailingComma": "es5",
  "endOfLine": "crlf"
}
```

明示的にリント・フォーマットをしたい場合のために`package.json`に以下を追記する。

```json
{
  "scripts": {
    "lint": "eslint ./src/ --fix",
    "fmt": "prettier ./src/ --write"
  }
}
```

ここからは、git hooks の管理用ライブラリである`husky`、commit 時の自動検証ライブラリである`lint-staged`を導入する。

```bash
git init
echo /node_modules > .gitignore

npx husky-init
npm i
```

この時点で git hooks path が書き換えられていることに注意する。（`.husky`ディレクトリのシェルスクリプトが参照されるようになっている。）
`.husky/pre-commit`シェルスクリプトの末尾にコマンドが書かれていることを確認する。これは`npx husky add`あるいは`npx husky set`を用いて変更できる。逆に言えば、husky はこの程度の機能のみを提供している。）

```bash
npm i -D lint-staged
npx husky set .husky/pre-commit "npx lint-staged"
```

`package.json`に以下を追記する。なお、lint-staged は非同期で各ファイルに対して操作を行うため、複数の glob パターンが同一のファイルにマッチし、かつ書き込み操作が行われる場合競合する可能性があるため注意する。
```json
"lint-staged": {
    "./src/**/*.{ts, tsx}": ["eslint --fix", "prettier --write"]
  }
```

---

## ライブラリ・フレームワーク紹介

### フロントエンド開発

React、あるいは React フレームワークである Next.js を使用する場合、

- `npx create-react-app`
- `npx create-next-app`

から構築するのが容易である。なお、React の競合フレームワークで Vue、そのフレームワークで Nuxt.js というものがある。
いずれにせよ、個々のサイトの

### バックエンド開発

Express, Fastify, NestJS などの選択肢がある。

### ORM

Prisma、あるいは TypeORM などがある。

### テスト

Jest が有名。

---

## 言語仕様
