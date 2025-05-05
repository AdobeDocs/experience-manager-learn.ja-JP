---
title: タグプロパティの作成
description: AEM と統合するための最小限の設定でタグプロパティを作成する方法を説明します。タグ UI を紹介し、拡張機能、ルールおよび公開ワークフローについて説明します。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '593'
ht-degree: 100%

---

# タグプロパティの作成 {#create-tag-property}

Adobe Experience Manager と統合するための最小限の設定でタグプロパティを作成する方法を説明します。タグ UI を紹介し、拡張機能、ルールおよび公開ワークフローについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/327449?quality=12&learn=on&captions=jpn)

## タグプロパティの作成

タグプロパティを作成するには、以下の手順を実行します。

1. ブラウザーで [Adobe Experience Cloud ホーム](https://experience.adobe.com/)ページにアクセスし、Adobe ID を使用してログインします。

1. Adobe Experience Cloud ホームページの「_クイックアクセス_」セクションで「**データ収集**」アプリケーションをクリックします。

1. 左側のナビゲーションから「**タグ**」メニュー項目をクリックし、右上隅にある「**新規プロパティ**」をクリックします。

1. 必須の「**名前**」フィールドを使用して、タグプロパティに名前を付けます。「ドメイン」フィールドにドメイン名を入力するか、AEM as a Cloud Service を使用している場合は、`adobeaemcloud.com` と入力し、「**保存**」をクリックします。

   ![タグプロパティ](assets/tag-properties.png)

## 新しいルールの作成

**タグプロパティ**&#x200B;ビューで名前をクリックして、新しく作成したタグプロパティを開きます。また、_最近のアクティビティ_&#x200B;見出しの下に、コア拡張機能が追加されていることがわかります。コアタグ拡張機能はデフォルトの拡張機能で、ページ読み込み、ブラウザー、フォーム、その他のイベントタイプなどの基本的なイベントタイプを提供します。詳しくは、[コア拡張機能の概要](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=ja)を参照してください。

ルールを使用すると、訪問者が AEM サイトとやり取りする際の動作を指定できます。話を簡単にするために、ブラウザーコンソールに 2 つのメッセージを記録して、データ収集タグ統合によって、AEM プロジェクトコードを更新することなく、AEM サイトにどのようにして JavaScript コードを挿入できるようになるかを示します。

ルールを作成するには、次の手順を実行します。

1. 左側のナビゲーションの「_オーサリング_」セクションで「**ルール**」をクリックし、「**新しいルールを作成**」をクリックします。

1. 必須の「**名前**」フィールドを使用して、ルールに名前を付けます。

1. 「_イベント_」セクションで「**追加**」をクリックしたあと、_イベント設定_&#x200B;フォームの「**イベントタイプ**」ドロップダウンで「_ライブラリの読み込み (ページのトップ)_」オプションを選択し、「**変更を維持**」をクリックします。

1. 「_アクション_」セクションで「**追加**」をクリックしたあと、_アクション設定_&#x200B;フォームの「**アクションタイプ**」ドロップダウンで「_カスタムコード_」オプションを選択し、「**エディターを開く**」をクリックします。

1. 「_コードを編集_」モーダルで次の JavaScript コードスニペットを入力したあと、「**保存**」をクリックし、最後に「**変更を維持**」をクリックします。

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 「**保存**」をクリックして、ルール作成プロセスを終了します。

   ![新しいルール](assets/new-rule.png)

## ライブラリの追加と公開

タグプロパティの&#x200B;_ルール_&#x200B;は、ライブラリを使用してアクティブ化されます。ライブラリは、JavaScript コードを含んだパッケージと考えてください。手順に従って、新しく作成したルールをアクティブ化します。

1. 左側のナビゲーションの「_公開_」セクションで「**公開フロー**」をクリックしたあと、「**ライブラリを追加**」をクリックします。

1. **名前**&#x200B;フィールドを使用してライブラリに名前を付け、「**環境**」ドロップダウンで「_開発(開発)_」オプションを選択します。

1. タグプロパティの作成以降に変更されたすべてのリソースを選択するには、「**+ 変更されたすべてのリソースを追加**」をクリックします。このアクションにより、新しく作成したルールとコア拡張機能のリソースがライブラリに追加されます。最後に、「**保存して開発用にビルド**」をクリックします。

1. **開発**&#x200B;スイムレーン用にライブラリがビルドされたら、_省略記号（...）_&#x200B;を使用して「**承認用に送信**」を選択します。

1. 次に、**送信済み**&#x200B;スイムレーンで&#x200B;_省略記号（...）_&#x200B;を使用して「**公開の承認**」を選択します。同様に、**承認済み**&#x200B;スイムレーンで「**ビルドして実稼動環境に公開**」を選択します。

![公開済みライブラリ](assets/published-library.png)


以上の手順で、ページが読み込まれたときにブラウザーのコンソールにメッセージを記録するルールを持つシンプルなタグプロパティの作成が完了しました。また、ルールとコア拡張機能は、ライブラリを作成することで公開されます。

## 次の手順

[IMS を使用して AEM をタグプロパティに接続する方法](connect-aem-tag-property-using-ims.md)


## その他のリソース {#additional-resources}

* [タグプロパティの作成](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ja)
