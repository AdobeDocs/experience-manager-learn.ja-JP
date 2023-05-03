---
title: スマートイメージング
description: Dynamic Media Classicのスマートイメージングは、クライアントのブラウザー機能に基づいて画像形式と画質を自動的に最適化することで、画像配信のパフォーマンスを向上させます。 これをおこなうには、Adobe Sensei AI 機能を活用し、既存の画像プリセットを操作します。 スマートイメージングの詳細と、それを使用してページ読み込みを高速化して、より優れた顧客体験を提供する方法について説明します。
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 6%

---

# スマートイメージング {#smart-imaging}

Web サイト、モバイルサイトまたはアプリでの顧客体験の最も重要な側面の 1 つは、ページ読み込み時間です。 ページの読み込みに時間がかかりすぎる場合、顧客は多くの場合、サイトまたはアプリを放棄します。 画像は、ページの読み込み時間の大部分を構成します。 Dynamic Media Classicのスマートイメージングは、クライアントのブラウザー機能に基づいて画像形式と画質を自動的に最適化することで、画像配信のパフォーマンスを向上させます。 これをおこなうには、Adobe Sensei AI 機能を活用し、既存の画像プリセットを操作します。 スマートイメージングにより、画像サイズが 30%以上縮小され、ページ読み込みが高速化され、顧客体験が向上します。

スマートイメージングをAdobeのクラス最高のプレミアムサービスと完全に統合することで、パフォーマンスを大幅にアップさせることもできます。 このサービスが、サーバー、ネットワークおよびピアリングポイント間を結ぶ、最適なインターネットルートを見つけます。最適なインターネットルートとは、待ち時間が最小限であったり、インターネット上のデフォルトルートよりもパケット損失率が低かったりするルートです。

詳細情報： [スマートイメージング](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## スマートイメージングの利点

画像はページの読み込み時間の大部分を占めるので、スマートイメージングによるパフォーマンスの向上は、ビジネス KPI（高いコンバージョン、サイトでの滞在時間、低いサイト直帰率）に大きな影響を与えます。

![画像](assets/smart-imaging/smart-imaging-1.png)

## スマートイメージングの仕組み

前述したように、スマートイメージングはAdobe Sensei AI 機能を利用し、既存の画像プリセットと連携して、画像を視覚的な忠実性を維持しながら、WebP などの最適な次世代画像形式に自動的に変換します。

詳細情報： [スマートイメージングの仕組み](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)( サポートされる画像形式などの詳細（およびこれらの形式を使用しない場合は何が起こるか）と、使用中の既存の画像プリセットへの影響を含む )

## スマートイメージングの影響

スマートイメージングを利用するには、サイト上の URL、画像プリセットおよびコードを変更する必要が生じる可能性があります。 スマートイメージングを使用するための前提条件を満たし、サポートされるJPEGおよび PNG 画像形式の画像のみを扱う場合は、変更を加える必要はありません。

スマートイメージングは、HTTP、HTTPS および HTTP/2 で配信された画像に対して機能します。

>[!NOTE]
>
>スマートイメージングに移行すると、CDN のキャッシュがクリアされます。 CDN のキャッシュは、通常、1 ～ 2 日以内に再構築されます。

スマートイメージングは、Dynamic Media Classicの既存のライセンスに含まれています。 この機能に追加費用はかかりません。 これを活用するには、次の 2 つの要件を満たす必要があります。には、Adobeバンドル CDN と専用ドメインがあります。 次に、アカウントは自動的には有効にならないので、アカウントに対して有効にする必要があります。

スマートイメージングを有効にするには、まず、次の手順でテクニカルサポートにリクエストを送信します： |サポートケースの作成| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). スマートイメージングに関連付けるカスタムドメインを設定するためのサポートがお客様と連携します。 キャッシュに関連するパラメーターを 1 つ変更し (Time To Live、TTL)、サポートによってキャッシュがクリアされます。 実稼動にプッシュする前に、必要に応じてオプションのステージング手順を実行することもできます。 スマートイメージングをオンにすると、より小さいサイズの画像が、要求された画質で顧客に提供されます。 つまり、Adobe Senseiが最も効率的なサイズを選択できるので、ページ読み込み時間が短縮されます。これらすべてが自動的におこなわれます。

スマートイメージングを有効にしたら、期待どおりに動作していることを確認できます。

スマートイメージングに関して他に質問がある場合があります。 よくある質問 (FAQ) のリストと回答をまとめました。 詳しくは、 [よくある質問](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## その他のリソース

次を監視： [Dynamic Media Classic Optimizing Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) スマートイメージングの詳細については、オンデマンドの Web セミナーを参照してください。
