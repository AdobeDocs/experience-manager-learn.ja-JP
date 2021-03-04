---
title: スマートイメージング
description: Dynamic Mediaクラシックのスマートイメージングは、クライアントブラウザーの機能に基づいて画像形式と画質を自動的に最適化することで、配信のパフォーマンスを強化します。 これは、Adobe SenseiAIの機能を活用し、既存の画像プリセットを操作することで実現できます。 スマートイメージングの詳細と、ページの読み込みを高速化して顧客体験をより良くオファーする方法について説明します。
sub-product: dynamic-media
feature: Dynamic Mediaクラシック
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: コンテンツ管理
role: 開業医
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 8%

---


# スマートイメージング {#smart-imaging}

Webサイト、モバイルサイト、アプリでの顧客体験の最も重要な側面の1つは、ページ読み込み時間です。 ページの読み込みに時間がかかり過ぎる場合、顧客はサイトやアプリを放棄することがよくあります。 画像は、ページ読み込み時間の大部分を占めます。 Dynamic Mediaクラシックのスマートイメージングは、クライアントブラウザーの機能に基づいて画像形式と画質を自動的に最適化することで、配信のパフォーマンスを強化します。 これは、Adobe SenseiAIの機能を活用し、既存の画像プリセットを操作することで実現できます。 スマートイメージングは、画像サイズを30%以上縮小します。これにより、ページ読み込み時間が短縮され、顧客体験が向上します。

また、スマートイメージングは、Adobeのクラス最高のプレミアムサービスと完全に統合され、パフォーマンスの向上をももたらします。 このサービスが、サーバー、ネットワークおよびピアリングポイント間を結ぶ、最適なインターネットルートを見つけます。最適なインターネットルートとは、待ち時間が最小限であったり、インターネット上のデフォルトルートよりもパケット損失率が低かったりするルートです。

[スマートイメージング](https://docs.adobe.com/content/help/ja/experience-manager-64/assets/dynamic/imaging-faq.html)の詳細を表示します。

## スマートイメージングの利点

画像はページの読み込み時間の大部分を占めるので、スマートイメージングによるパフォーマンスの向上は、コンバージョンの増加、サイトでの滞在時間の増加、サイトの直帰率の低下など、ビジネスKPIに大きな影響を与えます。

![画像](assets/smart-imaging/smart-imaging-1.png)

## スマートイメージングの仕組み

前述したように、スマートイメージングはAdobe SenseiのAI機能を活用し、既存の画像プリセットと連携して、画像を視覚的な忠実度を維持しながら、WebPなどの最適な次世代画像形式に自動的に変換します。

[スマートイメージングの仕組み](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)について、サポートされている画像形式（およびこれらの形式を使用しない場合は何が起こります）や、使用中の既存の画像プリセットへの影響について詳しく説明します。

## スマートイメージングの影響

スマートイメージングを利用するために、サイト上のURL、画像プリセット、コードを変更する必要があると考えられる場合があります。 スマートイメージングの使用に関する前提条件を満たし、サポートされるJPEGおよびPNG画像形式の画像のみを扱う場合は、変更を行う必要はありません。

スマートイメージングは、HTTP、HTTPS、およびHTTP/2経由で配信される画像と連携します。

>[!NOTE]
>
>Smart Imagingに移動すると、CDNのキャッシュがクリアされます。 通常、CDNのキャッシュは1 ～ 2日以内に再び構築されます。

スマートイメージングは、既存のDynamic Mediaクラシックのライセンスに含まれています。 この機能に追加費用はかかりません。 この機能を活用するには、次の2つの要件を満たす必要があります。には、AdobeバンドルCDNと専用ドメインがあります。 その後、アカウントで有効にする必要があります。これは、アカウントでは自動的には有効にならないからです。

テクニカルサポートを送信した後に、スマートイメージング開始を有効にするには、 |サポートケースの作成| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). スマートイメージングに関連付けるカスタムドメインを設定する場合は、サポートがご協力いたします。 キャッシュ(Time To Live(TTL)に関連する1つのパラメーターを変更すると、キャッシュがクリアされます。 また、実稼動環境にプッシュする前に、必要に応じてステージングのステップをオプションで実行することもできます。 その後、スマートイメージングをオンにすると、より小さいサイズの画像を顧客に提供し、要求された品質を維持します。 つまり、ページの読み込み時間が短くなります。これらの処理は、Adobe Senseiが最も効率的なサイズを選ぶのに役立つので、自動的に行われます。

スマートイメージングを有効にしたら、期待どおりに動作していることを確認します。

スマートイメージングに関するその他の質問があると思います。 よくある質問(FAQ)のリストをまとめ、回答を記載しました。 [FAQs](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)を読んでください。

## その他のリソース

スマートイメージングの詳細については、[Dynamic Mediaクラシック最適化ページパフォーマンススキルビルダー](https://seminars.adobeconnect.com/pzc1gw0cihpv)のオンデマンドウェビナーを参照してください。
