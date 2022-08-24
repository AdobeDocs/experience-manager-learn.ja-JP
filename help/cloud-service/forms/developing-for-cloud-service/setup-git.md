---
title: Git のインストールとセットアップ
description: ローカル Git リポジトリを初期化します
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Git のインストール


[Git のインストール](https://git-scm.com/downloads). デフォルトの設定を選択し、インストールプロセスを完了できます。
コマンドプロンプトに移動します。 c:\cloudmanager\aem-banking-app type in git —version に移動します。 システムにインストールされている GIT のバージョンが表示されます

## ローカル Git リポジトリを初期化

次の場所にいることを確認します。 c:\cloudmanager\aem-banking-app folder

```
git init
```

上記のコマンドは、プロジェクトを Git ローカルリポジトリーとして初期化します

```
git add .
```

これにより、Git リポジトリにコミットする準備が整った Git リポジトリに、すべてのプロジェクトファイルが追加されます

```
git commit -m "initial commit"
```

これにより、ファイルが Git リポジトリにコミットされます



## Cloud Manager リポジトリをローカル Git リポジトリに登録する

Cloud Manager リポジトリーへのアクセス
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

Cloud Manager の Git リポジトリをローカル Git リポジトリに登録します。 以下のコマンドは関連付けられます。 **bankingapp** リモート cloud manager git リポジトリを使用します。 の代わりに任意の名前を使用できました。 **bankingapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

（リポジトリ URL を使用していることを確認）

リモートリポジトリが登録されているかどうかを確認します

```java
git remote -v
```
