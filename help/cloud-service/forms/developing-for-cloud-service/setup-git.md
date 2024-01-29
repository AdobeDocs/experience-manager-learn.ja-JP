---
title: Git をインストールおよびセットアップ
description: ローカル Git リポジトリを初期化します。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 59
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '207'
ht-degree: 100%

---

# Git のインストール


[Git をインストールします](https://git-scm.com/downloads)。デフォルトの設定を選択し、インストールプロセスを完了できます。
コマンドプロンプトに移動します。
c:\cloudmanager\aem-banking-app に移動します。
git --version と入力します。 システムにインストールされている GIT のバージョンが表示されます。

## ローカル Git リポジトリを初期化

c:\cloudmanager\aem-banking-app folder にいることを確認します。 

```
git init
```

上記のコマンドは、プロジェクトを Git ローカルリポジトリとして初期化します。

```
git add .
```

これにより、Git リポジトリにコミットする準備が整った Git リポジトリに、すべてのプロジェクトファイルが追加されます。

```
git commit -m "initial commit"
```

これにより、ファイルが Git リポジトリにコミットされます



## Cloud Manager リポジトリをローカル Git リポジトリに登録する

Cloud Manager リポジトリにアクセス
![担当者情報にアクセス](assets/cloud-manager-repo.png)
Cloud Manager のリポジトリ資格情報を取得します。
![get-credentials](assets/cloud-manager-repo1.png)

ユーザー名を設定ファイルに保存します。

```java
git config --global credential.username "gbedekar-adobe-com"
```

パスワードを設定ファイルに保存します。

```java
git config --global user.password "XXXX"
```

（パスワードは Cloud Manager の Git リポジトリのパスワードです）

Cloud Manager の Git リポジトリをローカル Git リポジトリに登録します。 以下のコマンドは、**bankingapp** をリモート Cloud Manager Git リポジトリに関連付けます。**bankingapp** の代わりに任意の名前を使用できました。


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

（リポジトリ URL を使用していることを確認）

リモートリポジトリが登録されているかどうかを確認します。

```java
git remote -v
```

## 次の手順

[IntelliJ のAEMをAEM Project と同期](./intellij-and-aem-sync.md)
