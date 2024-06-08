# 第13回課題

- CircleCI のサンプルに ServerSpec や Ansible の処理を追加してください。
- Ansible はいきなりやりたいことを実装するのではなく、最低限の「必ず成功する Playbook」を用意して徐々に仕事を追加しましょう。

---

## 実施概要

Raisetechより提供されたCRUD 処理が出来るRailsアプリケーションを稼働させるインフラストラクチャを自動構築する。  
使用ツールは下記の通り。

**CircleCI**  
- CloudFormationの実行  
- Ansibleの実行
- ServerSpecの実行

**CloudFormation** 
- VPC、EC2、RDS、ELB、S3を作成（詳細は構成図に記載）

**Ansible**
- Railsアプリケーション用の環境構築を行う

**ServerSpec**
- Railsアプリケーションのレスポンスを確認

---
## 構成図

![構成図](images/Diagram.png)

---

## 事前設定
必要な環境変数はCircleCIの `Environment Variables` に登録しておく。 

![Environment Variables](images/01.png)

---

## 手順

1. CloudFormationの設定
2. Ansibleの設定
3. ServerSpecの設定
4. Circleciの実行結果を確認
5. Railsアプリケーションの動作確認

---

### CloudFormationの設定

AWS CLIを用いてCloudFormationを実行する設定を[config.yml](.circleci/config.yml)に追加。  

---

### Ansibleの設定
[config.yml](.circleci/config.yml)にAnsibleのplaybook.ymlを実行する設定を追加。（[Ansibleの設定ファイル](ansible)）

---

### ServerSpecの設定

第11回で作成したServerSpecのテストスクリプトを実行する設定を[config.yml](.circleci/config.yml)に追加。

---

### Circleciの実行結果を確認

1. 結果一覧<br>![全結果](images/02.png)
2. provisioning-stackの結果<br>![cfn-lint](images/03.png)
3. execute-ansible-playbookの結果<br>![execute-cfn](images/04.png)
4. execute-serverspecの結果<br>![execute-ansible](images/05.png)

---

### Railsアプリケーションの動作確認

1. ALBのDNS名でアクセスし、画像の保存が成功することを確認<br>![画像の保存成功](images/06.png)
2. S3へ画像が保存出来ていることを確認<br>![S3への保存成功](images/07.png)

---

## 所感
- 各アプリケーションのインストール工程やファイル設定などを把握しないと構築できないためとても苦戦した。
- 自動化を行う前に手動構築を行うことでイメージがつき、取り組みやすくなると感じた。
- CI/CDツールを利用することで環境構築が自動で行われ業務効率化に結び付いているのだなと実感することができた。

