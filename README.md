# CircleCI Hello World

[![miolab](https://circleci.com/gh/miolab/circleci_helloworld.svg?style=svg)](https://github.com/miolab/circleci_helloworld)

- **CircleCI** での `Hello World` 手順をここに書き置きます

- 以下 _CircleCI公式_ の内容を元にしつつ、一部アレンジして進めていきます

  - [Hello World](https://circleci.com/docs/ja/2.0/hello-world/)

    - [Docker (CirclCI / Node)](https://hub.docker.com/r/circleci/node)

  - [入門ガイド](https://circleci.com/docs/ja/2.0/getting-started/#section=getting-started)

### 前提

- GitHub で対象とするリポジトリは作成済（ローカル・リモート）

  - ここでは、リポジトリ名 **circleci_helloworld** として進めています

- GitHub と CircleCI との連携済

- 環境

  - macOS 10.14.6

  - CircleCI 2.1

  - CircleCI の [ローカル CLI](https://circleci.com/docs/ja/2.0/local-cli/) 導入済

    - `brew install circleci`

---

## Hello world 手順

### :rocket: `.circleci/config.yml` 作成

- ローカルリポジトリで `git checkout -b init_build` でブランチを切ってから、`.circleci/config.yml` を作成します

  ```yml
  version: 2.1
  jobs:
    build:
      docker:
        - image: circleci/node:4.8.2
      steps:
        - checkout
        - run: echo "Hello world, im!"
  ```

  - インデントにご注意ください

- ローカルで一度確認をするために、`circleci build` します

  ```bash
  $ circleci build

  Downloading latest CircleCI build agent...
  Docker image digest:
    .
    .
  Starting container circleci/node:4.8.2
    image cache not found on this host, downloading circleci/node:4.8.2
  4.8.2: Pulling from circleci/node
    .
    .
  Status: Downloaded newer image for circleci/node:4.8.2
    pull stats: download 326.1MiB in 2m29.396s (2.183MiB/s), extract 326MiB in 12.977s (25.12MiB/s)
    .
    .

  The redacted variables listed above will be masked in run step output.====>> Checkout code
  Making checkout directory "/home/circleci/project"
  Copying files from "/tmp/_circleci_local_build_repo" to "/home/circleci/project"
  ====>> echo "Hello world, im!"
    #!/bin/bash -eo pipefail
  echo "Hello world, im!"
  Hello world, im!
  Success!
  ```

- 問題ないようなので、`git push --set-upstream origin init_build` で **push** します

### :rocket: CircleCI 管理画面側の設定

- [CircleCI Projects（ログイン画面）](https://app.circleci.com/projects) から **Project** 画面に進んで、対象リポジトリの `Set Up Project` を選択します

  <img alt="" src="https://user-images.githubusercontent.com/33124627/91522522-69747600-e935-11ea-8169-1d9ddc1064c8.png">

- `Use Existing Config` を選択します

- `Start Building` で build が走り始めますので、しばらく待ちます

- build が通って、ログに `Hello world` が出力されました！

  <img width="1162" alt="スクリーンショット 2020-08-28 20 08 17" src="https://user-images.githubusercontent.com/33124627/91630561-45846380-ea0d-11ea-8ec1-44b19f667e61.png">

---

## ワークフロー導入

`workflow` を導入して、jobs を並列実行可能にします

- リファレンス（公式）

  - [ワークフローを使用したジョブのスケジュール](https://circleci.com/docs/ja/2.0/workflows/)

  - [CircleCI を設定する（workflows）](https://circleci.com/docs/ja/2.0/configuration-reference/#workflows)

  - [コンセプト](https://circleci.com/docs/ja/2.0/concepts/)

### :rocket: `.circleci/config.yml` アップデート

- ローカルで新しいブランチを切ります

  `$ git checkout -b "add_workflows"`

- `.circleci/config.yml` を、以下の内容へとアップデートします

  ```yml
  version: 2.1
  jobs:
    build_one:
      docker:
        - image: circleci/node:4.8.2
      steps:
        - checkout
        - run: echo "Hello world one, im!"
        - run: sleep 25
    build_two:
      docker:
        - image: circleci/node:4.8.2
      steps:
        - checkout
        - run: echo "Hello world two, im!"
        - run: sleep 15
  workflows:
    version: 2
    one_and_two:
      jobs:
        - build_one
        - build_two
  ```

  - __jobs名__（build_one, build_two の部分）は __任意の名前__ でOK

    - `workflow` を導入しない場合（= `jobs` が1つだけの場合）は、__jobs名__ を `build` と命名する必要があります

  - __workflow名__ も __任意の名前__ でOK

- push します

  `git push --set-upstream origin add_workflows`

### :rocket: CircleCI プロジェクト管理画面で結果確認

- Pipeline で job が __並列実行__ で走っています

  ![スクリーンショット 2020-08-31 19 14 01](https://user-images.githubusercontent.com/33124627/91710379-1dd4fd00-ebbf-11ea-9373-2050d8329051.png)

  ![スクリーンショット 2020-08-31 19 14 29](https://user-images.githubusercontent.com/33124627/91710381-1e6d9380-ebbf-11ea-8d1c-4de32b4f6248.png)

  ![スクリーンショット 2020-08-31 19 15 31](https://user-images.githubusercontent.com/33124627/91710382-1f062a00-ebbf-11ea-8eb3-f4275cf4896f.png)

  すべてのjobが通りました

- 念のため、`build_one` のログを確認してみます

  ![スクリーンショット 2020-08-31 19 16 41](https://user-images.githubusercontent.com/33124627/91710384-1f9ec080-ebbf-11ea-9fec-e4c686c5ff27.png)

  Hello world が出力されています！

---
---

## 余談

### :rocket: CircleCI の `Status Badge` を README に載せる

- Status Badge

  1. [![miolab](https://circleci.com/gh/miolab/circleci_helloworld.svg?style=svg)](https://github.com/miolab/circleci_helloworld)
  1. [![miolab](https://circleci.com/gh/miolab/circleci_helloworld.svg?style=shield)](https://github.com/miolab/circleci_helloworld)

  のようなバッヂを載せましょう

#### 手順

以下コードを、`README.md` に埋め込みます（パブリックリポジトリの場合）

```
[![【organization名】](https://circleci.com/【GitHubならgh。BitBucketならbb】/【organization名】/【リポジトリ名】.svg?style=【バッヂタイプ1.ならsvg、タイプ2.ならshield】)](【リポジトリURL】)`

# 例
[![miolab](https://circleci.com/gh/miolab/circleci_helloworld.svg?style=svg)](https://github.com/miolab/circleci_helloworld)
```

- 参考

  [CircleCI / Adding Status Badges](https://circleci.com/docs/2.0/status-badges/)

---

## その他参考

- [公式 Docs](https://circleci.com/docs/ja/)

  - [サポートセンター](https://support.circleci.com/hc/ja)

  - [CircleCI を設定する](https://circleci.com/docs/ja/2.0/configuration-reference/)

  - __Orbs__

    1. [Orbs、ジョブ、ステップ、ワークフロー](https://circleci.com/docs/ja/2.0/jobs-steps/#section=getting-started)

    1. [Orb の概要](https://circleci.com/docs/ja/2.0/orb-intro/)

    1. [Orb のコンセプト](https://circleci.com/docs/ja/2.0/using-orbs/)

    1. [Orbs リファレンス ガイド](https://circleci.com/docs/ja/2.0/reusing-config/)

  - [CircleCI 構成クックブック](https://circleci.com/docs/ja/2.0/configuration-cookbook/)

  - __Docker__

    - machine executor

      - [Executor タイプを選択する / Machine の使用](https://circleci.com/docs/ja/2.0/executor-types/#machine-%E3%81%AE%E4%BD%BF%E7%94%A8)

      - [Running Docker Commands / Mounting Folders](https://circleci.com/docs/2.0/building-docker-images/#mounting-folders)

    - other

      - [カスタム ビルドの Docker イメージの使用](https://circleci.com/docs/ja/2.0/custom-images/)

      - [CircleCI のビルド済み Docker イメージ](https://circleci.com/docs/ja/2.0/circleci-images/)

      - [docker-compose のインストールと使用](https://circleci.com/docs/ja/2.0/docker-compose/)

      - [Docker コマンドの実行手順](https://circleci.com/docs/ja/2.0/building-docker-images/)

  - [チュートリアル](https://circleci.com/docs/ja/2.0/tutorials/)

- [公式 ブログ](https://circleci.com/ja/blog/tag/tutorials/)

  - [カスタム Docker イメージを作成して CI ビルドを実行する](https://circleci.com/ja/blog/creating-a-custom-docker-image-to-run-your-ci-builds/)

  - [Heroku への継続的デプロイ](https://circleci.com/ja/blog/continuous-deployment-to-heroku/)

- [CircleCI Webinar](https://speakerdeck.com/kurumai/circleci-webinar)

- [CircleCI Connpass 資料](https://circleci.connpass.com/presentation/)

- [いまさらだけど CircleCI に入門したので分かりやすくまとめてみた（Qiita）](https://qiita.com/gold-kou/items/4c7e62434af455e977c2)

- __[CircleCI status](https://status.circleci.com/)__
