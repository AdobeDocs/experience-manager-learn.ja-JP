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
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: ht
source-wordcount: '139'
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
