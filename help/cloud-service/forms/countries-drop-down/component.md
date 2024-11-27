---
title: 国コンポーネントの構造の作成
description: Crx でのコンポーネント構造の作成
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 6%

---

# 国コンポーネントの構造の作成

AEM Forms インスタンスにログインし、次の手順に従って、標準のドロップダウンコンポーネントに基づいて新しいコンポーネントを作成します。

1. CRXDE Liteの「/apps/&lt;yourproject>/components/adaptiveForm/dropdown」に移動します。
2. ドロップダウンコンポーネントをコピーし、同じディレクトリレベルに貼り付けます。
3. コピーしたコンポーネントの名前を国に変更します。
4. cq:template ノードの jcr:title プロパティを Countries に更新します。
5. 変更を保存します。

これで、「国」という名前の新しいコンポーネントが作成されました。これは、標準のドロップダウンコンポーネントの正確なレプリカです。 これは、さらにカスタマイズするための基盤となります。

## HTL ファイルの作成

国コンポーネントの HTL ファイルを作成するには：

1. crx リポジトリの国フォルダーに移動します
2. countries.html という名前の新規ファイルを作成します。
3. crx リポジトリ内の/apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html ファイルを開き、その内容をコピーします。
4. コピーしたコンテンツを countries.html に貼り付けます。
5. 新しい sling モデルを使用するようにコードを変更します（スクリーンショットを参照）。
6. 変更を保存します。

![sling-model](assets/countriesdropdown.png)

最後に、CRX リポジトリ内の変更がAEM プロジェクトに反映されるように、プロジェクトをこれらの更新と同期します。


## 次の手順

[cq-dialog の作成](./dialog.md)
