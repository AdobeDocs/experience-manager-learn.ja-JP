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
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# AEMプロジェクトを cloud manager git リポジトリにプッシュ

前の手順では、AEMプロジェクトをAEMインスタンスで作成されたアダプティブFormsおよびテーマと同期しました。
次に、これらの変更をローカルの Git リポジトリに追加し、ローカルの Git リポジトリを cloud Manager の Git リポジトリにプッシュする必要があります。
コマンドプロンプトを開き、 c:\cloudmanager\aem-banking-app Execute the following commandsに移動します。

```
git add .
```

これにより、新しいファイルがローカル Git リポジトリのステージブランチに追加されます

```
git commit -m "My First AF"
```

これにより、ファイルがローカル Git リポジトリーの master ブランチにコミットされます

```
git push -f bankingapp master:"MyFirstAF"
```

上記のコマンドでは、ローカル Git リポジトリーから bankingapp わかりやすい名前で識別される Cloud Manager リポジトリーの MyFirstAF ブランチに master ブランチをプッシュします。
