# CircleCI Hello World

- **CircleCI** での `Hello World` 手順をここに書き置きます

- 以下 _公式_ の内容を元にしつつ、一部アレンジして進めていきます

  - [Hello World](https://circleci.com/docs/ja/2.0/hello-world/)

    - [Docker (CirclCI / Node)](https://hub.docker.com/r/circleci/node)

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

## 手順

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

## その他参考

- [公式 Docs](https://circleci.com/docs/ja/)

  - [入門ガイド](https://circleci.com/docs/ja/2.0/getting-started/#section=getting-started)

- [公式 ブログ](https://circleci.com/ja/blog/tag/tutorials/)

  - [Heroku への継続的デプロイ](https://circleci.com/ja/blog/continuous-deployment-to-heroku/)

- [CircleCI Webinar](https://speakerdeck.com/kurumai/circleci-webinar)

- [CircleCI Connpass 資料](https://circleci.connpass.com/presentation/)

- [いまさらだけど CircleCI に入門したので分かりやすくまとめてみた（Qiita）](https://qiita.com/gold-kou/items/4c7e62434af455e977c2)
