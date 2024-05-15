---
title: リポジトリブラウザーを使用した AEM のデバッグ
description: リポジトリブラウザーは、AEM の基盤となるデータストアを可視化する強力なツールで、AEM as a Cloud Service 環境のデバッグを容易にします。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 100%

---

# リポジトリブラウザーを使用した AEM as a Cloud Service のデバッグ

リポジトリブラウザーは、AEM の基盤となるデータストアを可視化する強力なツールで、AEM as a Cloud Service 環境のデバッグを容易にします。リポジトリブラウザーは、実稼動、ステージングおよび開発時の AEM のリソースとプロパティおよびオーサー、パブリッシュ、プレビューの各サービスの読み取り専用ビューをサポートしています。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

リポジトリブラウザーは、AEM as a Cloud Service 環境で&#x200B;__のみ__&#x200B;使用できます（ローカル AEM SDK をデバッグするには [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) を使用します）。

## リポジトリブラウザーへのアクセス

AEM as a Cloud Service でリポジトリブラウザーにアクセスするには：

1. ユーザーが[必要なアクセス権](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=ja#access-prerequisites)を持っていることを確認します。
1. [Cloud Manager](https://my.cloudmanager.adobe.com) にログインします。
1. デバッグする AEM as a Cloud Service 環境を含んだプログラムを選択します。
1. デバッグする AEM as a Cloud Service 環境に対応する [Developer Console](./developer-console.md) を開きます。
1. 「__リポジトリブラウザー__」タブを選択します。
1. 参照する AEM サービス層を選択します。
   + すべてのオーサー
   + すべてのパブリッシュ
   + すべてのプレビュー
1. 「__リポジトリブラウザーを開く__」を選択します。

選択したサービス層（オーサー、パブリッシュまたはプレビュー）のリポジトリブラウザーが読み取り専用モードで開き、ユーザーがアクセスできるリソースとプロパティが表示されます。

## パブリッシュとプレビューへのアクセス

デフォルトでは、「パブリッシュ」や「プレビュー」へのアクセスは制限されているので、リポジトリブラウザーで使用可能なリソースが少なくなります。[パブリッシュ（またはプレビュー）ですべてのリソースを表示するには、ユーザーをパブリッシュ（またはプレビュー）管理者の役割に追加します。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=ja#navigate-the-hierarchy)
