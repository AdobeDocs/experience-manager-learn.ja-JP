---
title: アセット共有コモンズのテーマ設定の概要
seo-title: アセット共有コモンズのテーマ設定の概要
description: 資産共有コモンズに関する機能的および技術的な理解のための資料
seo-description: 資産共有コモンズに関する機能的および技術的な理解のための資料
uuid: 5991a015-392a-4bb5-8332-192681505b07
discoiquuid: 08a5a394-c62b-4748-b303-33117f283612
contentOwner: dgonzale
feature: asset-share, brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 1%

---


# アセット共有コモンズでのテーマ設定の概要{#asset-share-commons-theme}

アセット共有コモンズでのテーマ設定について簡単に説明します。 このビデオでは、カスタムカラースキームを使用して新しいテーマを作成するプロセスについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

このビデオでは、アセット共有コモンズのダークテーマに基づいて新しいテーマが作成されます。 カラースキームは、サイトのルック&amp;フィールを一貫させるためにカスタムロゴと一致します。

## テーマ変数

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

[カスタムクライアントライブラリテーマ](assets/asc-theme-demo.zip)のダウンロード

## その他のリソース{#additional-resources}

* [アセット共有コモンズリリースのダウンロード](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0以降のリリースのダウンロード](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)