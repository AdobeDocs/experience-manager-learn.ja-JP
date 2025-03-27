---
title: 国コンポーネントの構造の作成
description: CRX でのコンポーネント構造の作成
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: 87e790c9-6ef6-4337-90b8-687ca576b21a
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '215'
ht-degree: 100%

---

# 国コンポーネントの構造の作成

AEM Forms インスタンスにログインし、次の手順に従って、標準のドロップダウンコンポーネントに基づいて新しいコンポーネントを作成します。

1. CRXDE Lite で「/apps/&lt;yourproject>/components/adaptiveForm/dropdown」に移動します。
2. ドロップダウンコンポーネントをコピーし、同じディレクトリレベルにペーストします。
3. コピーしたコンポーネントの名前を国に変更します。
4. cq:template ノードの jcr:title プロパティを国に更新します。
5. 変更を保存します。

これで、国という名前の新しいコンポーネントが作成されます。これは、標準のドロップダウンコンポーネントの正確なレプリカです。これは、さらなるカスタマイズの基盤として機能します。

## HTL ファイルの作成

国コンポーネントの HTL ファイルを作成するには、次の手順に従います。

1. CRX リポジトリの国フォルダーに移動します
2. countries.html という名前の新しいファイルを作成します。
3. CRX リポジトリ内の /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html ファイルを開き、その内容をコピーします。
4. コピーした内容を countries.html にペーストします。
5. スクリーンショットに示すように、新しい Sling モデルを使用するようにコードを変更します
6. 変更を保存します。

![Sling モデル](assets/countriesdropdown.png)

最後に、CRX リポジトリ内の変更が AEM プロジェクトに反映されるように、プロジェクトをこれらの更新と同期します。


## 次の手順

[cq-dialog の作成](./dialog.md)
