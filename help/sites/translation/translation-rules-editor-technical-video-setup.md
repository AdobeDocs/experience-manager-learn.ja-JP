---
title: AEMでの翻訳ルールの設定
description: 翻訳設定UIを使用すると、AEM Sitesでコンテンツを翻訳するためのルールを管理できます。 このビデオでは、カスタムコンポーネント用の新しい翻訳ルールの作成について詳しく説明します。
feature: language-copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 4%

---


# 翻訳ルールの設定 {#set-up-translation-rules-in-aem}

翻訳設定UIを使用すると、AEM Sitesでコンテンツを翻訳するためのルールを管理できます。 このビデオでは、カスタムコンポーネント用の新しい翻訳ルールの作成について詳しく説明します。

>[!NOTE]
>
> 次のビデオはAEM 6.3で録画されています。AEM 6.4以降では、変換ルールXMLファイルを保存するための新しいリポジトリ構造が導入されています。 AEM 6.4以降で翻訳設定UIを使用する場合、ルールは場所に保存され `/conf/global/settings/translation/rules/translation_rules.xml`ます。 詳しくは、 [翻訳するコンテンツの識別](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) （英語）を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

翻訳ルールは、翻訳用に抽出するAEMのコンテンツを識別します。 初期設定の変換ルールでは、テキストコンポーネントや画像コンポーネントの代替テキストなど、一般的な使用例について説明しています。 プロジェクトの翻訳要件に応じて、追加のルールが必要になる場合があります。 一般的な翻訳ルールでは、ユーザーは次の項目を指定できます。

1. パスやリソースの種類に基づいて変換する必要があるプロパティ
2. 変換しないプロパティのフィルター
3. 翻訳する必要のある参照先のコンテンツ（画像やコンテンツフラグメントなど）

変換xmlファイルを更新する変換ルールエディター。 翻訳設定UIを使用すると、様々な翻訳ルールを簡単に管理でき、XMLを直接編集する際の入力ミスを防ぐことができます。

翻訳設定UIにアクセスします。

* **[!UICONTROL AEM開始メニュー]/[!UICONTROL ツール]/[!UICONTROL 一般]/[[!UICONTROL 翻訳設定]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3より前 {#prior-to-aem}

以前のAEMバージョンの変換ルールは、変換ワークフローの下にあるXMLファイルを編集することで手動で更新されていました。 `/etc/workflow/models/translation/translation_rules.xml`.

## その他のリソース {#additional-resources}

* [翻訳するコンテンツの特定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [多言語サイトのコンテンツの翻訳](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻訳のベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
