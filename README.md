# CiCdHandsOn
GithubAction や awsを用いたCICDツールをいろいろやっていくリポジトリ

## CICDを知ろう

### CICDって何ですか??

継続的インテグレーション CI / 継続的デリバリー CD

![image](https://github.com/GitEngHar/CiCdHandsOn/assets/119464648/1bf2fb68-97d2-4da3-a127-a6b73afc46b4)

画像の引用元 : https://www.servicenow.com/jp/products/devops/what-is-cicd.html

CI CDでアプリ開発のステージを一部自動化することで、顧客にアプリケーションを頻繁に提供できる (引用 : [redhad](https://www.redhat.com/ja/topics/devops/what-is-ci-cd))  
結果 市場のニーズに常に対応でき、競争力を保つことができる

- CI
  - アプリケーションに変更を加えた際の **イメージのビルド** や **単体テスト** を自動化
  - 開発したソースコードやImageに **脆弱性がないか診断** や **プログラムのお作法に問題は無いか**
- CD
  - 開発したアプリケーションを実行環境 (開発・検証・本番)へ **デプロイ(アプリケーションの配置)** する

※本番環境へのデプロイは検証環境でエビデンスを取って、チームでレビューしてから実施すると思われます

デプロイとリリースの違いが気になる方へ
<a href='https://zenn.dev/bun913/articles/fa6179472f8f82'>結局デプロイとリリースってどう違うの</a>

## 百聞は一見に如かず

実演用endpoint
```bash
http://cicdha-webAp-eQSCGIFzwy98-1124916881.ap-northeast-1.elb.amazonaws.com
```

### 実行の構成

githubAction で AWS で設定しているCodeDeployを動かしているので、実態はAWSにあると思われる
![simpleWebapp drawio](https://github.com/GitEngHar/CiCdHandsOn/assets/119464648/aa731a1d-bc43-4c35-9e22-d2d3663555bd)

### CICDを使った世界

CI/CD実行 : githubActionで定義
デプロイ設定 : aws codedeploy (cloudformationで管理)

1. ローカル作業pcでGithubリポジトリをclone
2. アプリに変更を加える
3. 変更をGithub リポジトリ に反映

Imageの作成とアプリケーションへの反映は自動で実施される
※検証環境でのデプロイを想定した流れ

### CICDを使わない世界（時間あれば）

ローカル環境でイメージファイルを作成して、アプリケーションを起動します

CI : イメージのビルド、リポジトリへのPush
CD : 本番環境へのデプロイ

1. イメージ構築環境でGithubリポジトリをclone
2. アプリに変更を加える
3. 変更されたDockerImageを作成
4. ImageをAWS ECR へPush
5. 新規Imageのアプリケーション定義を作成
6. タスク・コンテナ等のアプリケーション実行リソースにデプロイする

- 本来は、以下のようになっており、実演した内容よりもっと手間がかかる
    - ローカルではなくクラウド環境や、自社のオンプレで動いている
    - リポジトリも社内DockerリポジトリやAWS ECRなど
    - 単体テスト・脆弱性診断・コード診断

### CICDのココが凄い (まとめ)

CICD前
```text
1. イメージ構築環境でGithubリポジトリをclone
2. アプリに変更を加える
3. 変更されたDockerImageを作成
4. ImageをAWS ECR へPush
5. 新規Imageのアプリケーション定義を作成
6. タスク・コンテナ等のアプリケーション実行リソースにデプロイする
```

CICD後
```text
1. ローカル作業pcでGithubリポジトリをclone
2. アプリに変更を加える
3. 変更をGithub リポジトリ に反映
```

素早い変更の統合(インテグレーション) と デプロイ を実施することで、市場のニーズに常に対応でき、競争力を保つことができる
