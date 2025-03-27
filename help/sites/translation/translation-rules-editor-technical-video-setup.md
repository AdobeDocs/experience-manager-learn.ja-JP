---
title: AEM で翻訳ルールを設定
description: 翻訳設定 UI を使用すると、AEM Sites でコンテンツを翻訳するためのルールを管理できます。このビデオでは、カスタムコンポーネントの新しい翻訳ルールの作成について詳しく説明します。
feature: Language Copy
version: Experience Manager 6.4, Experience Manager 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 542
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '283'
ht-degree: 100%

---

# 翻訳ルールのセットアップ {#set-up-translation-rules-in-aem}

翻訳設定 UI を使用すると、AEM Sites でコンテンツを翻訳するためのルールを管理できます。このビデオでは、カスタムコンポーネントの新しい翻訳ルールの作成について詳しく説明します。

>[!NOTE]
>
> 以下のビデオは AEM 6.3 で録画されました。AEM 6.4 以降では、翻訳ルール XML ファイルを保存するための新しいリポジトリ構造を導入しました。AEM 6.4 以降で翻訳設定 UI を使用する場合、ルールは `/conf/global/settings/translation/rules/translation_rules.xml` の場所に保存されます。詳しくは、[翻訳するコンテンツの識別](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html?lang=ja-JP) を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

翻訳ルールは、翻訳用に抽出される AEM 内のコンテンツを識別します。標準搭載の翻訳ルールは、テキストコンポーネントや画像コンポーネントの代替テキストなど、一般的なユースケースに対応します。プロジェクトの翻訳要件に応じて、追加のルールが必要になる場合があります。一般的な翻訳ルールでは、ユーザーは次の項目を指定できます。

1. パスやリソースタイプに基づいて翻訳する必要があるプロパティ
2. 翻訳しないプロパティのフィルター
3. 翻訳する必要がある参照コンテンツ（画像またはコンテンツフラグメント）

翻訳 xml ファイルを更新する翻訳ルールエディター。翻訳設定 UI を使用すると、XML を直接編集する際の様々な翻訳ルールの管理が容易になり、入力ミスに対する対策が容易になります。

翻訳設定 UI にアクセスします。

* **[!UICONTROL AEM スタートメニュー]／[!UICONTROL ツール]／[!UICONTROL 一般]／[[!UICONTROL 翻訳設定]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3 以前 {#prior-to-aem}

以前の AEM バージョンの翻訳ルールは、翻訳ワークフロー `/etc/workflow/models/translation/translation_rules.xml` の下にある XML ファイルを編集することで手動で更新されていました。

## その他のリソース {#additional-resources}

* [翻訳するコンテンツの特定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html?lang=ja-JP)
* [多言語サイトのコンテンツの翻訳](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/tc-manage.html](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-manage.html?lang=ja)
* [翻訳のベストプラクティス](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-bp.html?lang=ja)
