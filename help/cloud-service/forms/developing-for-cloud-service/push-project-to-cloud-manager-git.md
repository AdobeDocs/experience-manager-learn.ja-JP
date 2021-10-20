---
title: AEMプロジェクトを Cloud Manager リポジトリにプッシュ
description: ローカル Git リポジトリを Cloud Manager リポジトリにプッシュ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# AEMプロジェクトを Cloud Manager Git 担当者にプッシュ

前の手順では、 AEMプロジェクトを、 AEMインスタンスで作成したアダプティブFormsおよびテーマと同期しました。
次に、これらの変更をローカルの Git リポジトリに追加し、ローカルの Git リポジトリを cloud Manager の Git リポジトリにプッシュする必要があります

コマンドプロンプトを開き、 c:\cloudmanager\aem-banking-app Execute the following commandsに移動します。

```
git add .**
```

これにより、新しいファイルがローカル Git リポジトリのステージブランチに追加されます

```
git commit -m "My First AF"
```

これにより、ファイルがローカル Git リポジトリの master ブランチにコミットされます

```
git push -f bankingapp master:"My First AF"
```

上記のコマンドでは、ローカル Git リポジトリーから Cloud Manager リポジトリーの My First AF ブランチに、 bankingapp わかりやすい名前で識別されるマスターブランチをプッシュします。



