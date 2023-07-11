---
title: AEM プロジェクトを Cloud Manager リポジトリにプッシュ
description: ローカル Git リポジトリを Cloud Manager リポジトリにプッシュ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: ht
source-wordcount: '147'
ht-degree: 100%

---

# AEM プロジェクトを Cloud manager Git リポジトリにプッシュ

前の手順では、AEM プロジェクトを AEM インスタンスで作成されたアダプティブフォームおよびテーマと同期しました。
次に、これらの変更をローカルの Git リポジトリに追加し、ローカルの Git リポジトリを Cloud Manager の Git リポジトリにプッシュする必要があります。
コマンドプロンプトを開き、 c:\cloudmanager\aem-banking-app に移動します。
次のコマンドを実行します。

```
git add .
```

これにより、新しいファイルがローカル Git リポジトリのステージブランチに追加されます。

```
git commit -m "My First AF"
```

これにより、ファイルがローカル Git リポジトリのメインブランチにコミットされます

```
git push -f bankingapp master:"MyFirstAF"
```

上記のコマンドでは、ローカル Git リポジトリから bankingapp のわかりやすい名前で識別される Cloud Manager リポジトリの MyFirstAF ブランチにメインブランチをプッシュします。

## 次の手順

[プロジェクトを開発環境にデプロイする](./deploy-to-dev-environment.md)
