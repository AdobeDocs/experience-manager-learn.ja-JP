---
title: タグプロパティの作成
description: AEMと統合するための最小限の設定でタグプロパティを作成する方法を説明します。 タグ UI を紹介し、拡張機能、ルール、公開ワークフローについて学習します。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# タグプロパティの作成 {#create-tag-property}

Adobe Experience Managerと統合するための最小限の設定でタグプロパティを作成する方法を説明します。 タグ UI を紹介し、拡張機能、ルール、公開ワークフローについて学習します。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## タグプロパティの作成

タグプロパティを作成するには、次の手順を実行します。

1. ブラウザーで、 [Adobe Experience Cloudホーム](https://experience.adobe.com/) ページにアクセスし、Adobe IDを使用してログインします。

1. 次をクリック： **データ収集** 申請 _クイックアクセス_ 」セクションに表示されます。

1. 次をクリック： **タグ** 左側のナビゲーションからメニュー項目を選択し、 **新しいプロパティ** を右上隅から選択します。

1. を使用してタグプロパティに名前を付けます。 **名前** 必須フィールド。 「ドメイン」フィールドに、ドメイン名を入力します。または、AEMas a Cloud Service環境を使用している場合は、を入力します。 `adobeaemcloud.com` をクリックし、 **保存**.

   ![タグのプロパティ](assets/tag-properties.png)

## 新しいルールを作成

新しく作成したタグプロパティを開くには、 **タグのプロパティ** 表示 また、 _最近のアクティビティ_ の見出しに、Core 拡張機能が追加されていることが表示されます。 Core タグ拡張機能はデフォルトの拡張機能で、ページ読み込み、ブラウザー、フォーム、その他のイベントタイプなどの基本的なイベントタイプを提供します。詳しくは、 [Core 拡張機能の概要](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) を参照してください。

ルールを使用すると、訪問者がAEMサイトとやり取りする際の動作を指定できます。 話を簡単にするために、ブラウザーコンソールに 2 つのメッセージを記録して、データ収集タグ統合がAEM Project コードを更新せずにAEMサイトに JavaScript コードを挿入する方法を示します。

ルールを作成するには、次の手順を実行します。

1. クリック **ルール** から _オーサリング_ セクションを開き、 **新規ルールの作成**

1. 次を使用してルールに名前を付けます。 **名前** 必須フィールド。

1. クリック **追加** から _イベント_ セクションで、 _イベント設定_ フォーム、 **イベントタイプ** ドロップダウン選択 _読み込まれたライブラリ（ページ上部）_ オプションを選択し、 **変更を保持**.

1. クリック **追加** から _アクション_ セクションで、 _アクションの設定_ フォーム、 **アクションタイプ** ドロップダウン選択 _カスタムコード_ オプションを選択し、 **編集画面を開く**.

1. 内 _コードを編集_ モーダルを開く場合は、次の JavaScript コードスニペットを入力し、 **保存**&#x200B;をクリックし、最後に **変更を保持**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. クリック **保存** 」をクリックして、ルール作成プロセスを完了します。

   ![新規ルール](assets/new-rule.png)

## ライブラリを追加して公開する

Tag プロパティ _ルール_ はライブラリを使用してアクティベートされます。ライブラリは、JavaScript コードを含むパッケージと考えてください。 手順に従って、新しく作成したルールをアクティブ化します。

1. クリック **公開フロー** から _公開_ 左側のナビゲーションの「 」セクションで、 **ライブラリを追加**

1. を使用してライブラリに名前を付けます。 **名前** フィールドと選択 _開発（開発）_ オプション **環境** ドロップダウン。

1. タグプロパティの作成後に変更されたすべてのリソースを選択するには、 **+変更されたすべてのリソースを追加**. この操作により、新しく作成したルールとコア拡張機能のリソースがライブラリに追加されます。 最後に、 **開発用に保存およびビルド**.

1. ライブラリが構築されると、 **開発** スイムレーン，使用 _省略記号_ を選択します。 **承認用に送信**

1. 次に、 **送信済み** ～を使ってレーンを泳ぐ _省略記号_ を選択します。 **公開の承認**&#x200B;同様に **ビルドして実稼動環境にパブリッシュ** 内 **承認済み** レーンを泳ぐ。

![公開済みライブラリ](assets/published-library.png)


上記の手順では、ページの読み込み時にメッセージをブラウザーコンソールに記録するルールを持つ、単純なタグプロパティの作成が完了します。 また、ルールとコア拡張機能は、ライブラリを作成して公開されます。

## 次のステップ

[IMS を使用してAEMをタグプロパティに接続](connect-aem-tag-property-using-ims.md)


## その他のリソース {#additional-resources}

* [タグプロパティの作成](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
