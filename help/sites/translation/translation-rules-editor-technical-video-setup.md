---
title: AEMでの翻訳ルールの設定
description: 翻訳設定 UI を使用すると、AEM Sitesでコンテンツを翻訳するためのルールを管理できます。 このビデオでは、カスタムコンポーネントの新しい翻訳ルールの作成について詳しく説明します。
feature: Language Copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
exl-id: 359da531-839c-4680-abf9-c880cc700159
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 6%

---

# 翻訳ルールを設定 {#set-up-translation-rules-in-aem}

翻訳設定 UI を使用すると、AEM Sitesでコンテンツを翻訳するためのルールを管理できます。 このビデオでは、カスタムコンポーネントの新しい翻訳ルールの作成について詳しく説明します。

>[!NOTE]
>
> 以下のビデオはAEM 6.3 で録画されました。AEM 6.4 以降では、翻訳ルール XML ファイルを保存するための新しいリポジトリ構造を導入しました。 AEM 6.4 以降で翻訳設定 UI を使用する場合、ルールは場所に保存されます `/conf/global/settings/translation/rules/translation_rules.xml`. 詳しくは、 [翻訳するコンテンツの識別](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

翻訳ルールは、翻訳用に抽出されるAEM内のコンテンツを識別します。 標準の翻訳ルールでは、テキストコンポーネントや画像コンポーネントの代替テキストなど、一般的な使用例を扱っています。 プロジェクトの翻訳要件に応じて、追加のルールが必要になる場合があります。 一般的な翻訳ルールでは、ユーザーは次の項目を指定できます。

1. パスやリソースタイプに基づいて翻訳する必要があるプロパティ
2. 翻訳しないプロパティのフィルター
3. 翻訳する必要がある参照コンテンツ（画像またはコンテンツフラグメント）

翻訳 xml ファイルを更新する翻訳ルールエディター。 翻訳設定 UI を使用すると、XML を直接編集する際の様々な翻訳ルールの管理が容易になり、入力ミスに対する対策が容易になります。

翻訳設定 UI にアクセスします。

* **[!UICONTROL AEM Start Menu] > [!UICONTROL ツール] > [!UICONTROL 一般] > [[!UICONTROL 翻訳設定]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3 以前 {#prior-to-aem}

以前のAEMバージョンの翻訳ルールは、翻訳ワークフローの下にある XML ファイルを編集することで手動で更新されていました。 `/etc/workflow/models/translation/translation_rules.xml`.

## その他のリソース {#additional-resources}

* [翻訳するコンテンツの特定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [多言語サイトのコンテンツの翻訳](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻訳のベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
