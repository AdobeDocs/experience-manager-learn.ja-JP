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
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---


# AEMプロジェクトを Cloud Manager の Git リポジトリにプッシュ

前の手順では、 AEMプロジェクトを、 AEMインスタンスで作成したアダプティブFormsおよびテーマと同期しました。
次に、これらの変更をローカルの Git リポジトリに追加し、ローカルの Git リポジトリを cloud Manager の Git リポジトリにプッシュする必要があります。
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
git push -f bankingapp master:"MyFirstAF"
```

上記のコマンドでは、ローカル Git リポジトリーの master ブランチを、 bankingapp わかりやすい名前で識別される Cloud Manager リポジトリーの MyFirstAF ブランチにプッシュします。



