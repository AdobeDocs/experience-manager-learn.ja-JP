---
title: Adobe Target を使用したパーソナライズ機能
description: Adobe Target を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 100%

---

# Adobe Target を使用した完全な web ページエクスペリエンスのパーソナライズ機能

前の章では、エクスペリエンスフラグメントとして作成され、AEM から HTML オファーとして書き出されたコンテンツを使用して、Adobe Target 内で位置情報ベースのアクティビティを作成する方法について説明しました。

この章では、AEM でホストされているサイトページを新しいページにリダイレクトするアクティビティを、Adobe Target を使用して作成する方法について説明します。

## シナリオの概要

WKND サイトはホームページのデザインを一新し、現在のホームページの訪問者を新しいホームページにリダイレクトしたいと考えています。 同時に、再設計されたホームページがユーザーエンゲージメントと収益の改善にどのように役立つかも把握する必要があります。 マーケターは、訪問者を新しいホームページにリダイレクトするアクティビティを作成するタスクを割り当てられています。 WKND サイトのホームページを参照し、Adobe Target を使用してアクティビティを作成する方法を確認していきましょう。

### 関係するユーザー

この演習では、次のユーザーが関与し、管理者アクセスが必要なタスクを実行する必要があります。

* **コンテンツプロデューザー／コンテンツ編集者**（Adobe Experience Manager）
* **マーケター**（Adobe Target／最適化チーム）

### WKND サイトのホームページ

![AEM Target シナリオ 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * それぞれがローカルホスト 4502 と 4503 で実行中の [AEM オーサーインスタンスとパブリッシュインスタンス](./implementation.md#getting-aem)。
   * [タグを使用して AEM を Adobe Target と統合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織の Adobe Experience Cloud にアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションをプロビジョニングされた Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

## コンテンツエディターアクティビティ

1. マーケターは、AEM コンテンツエディターで WKND ホームページのデザイン刷新に関するディスカッションを開始し、要件の詳細を説明します。
   * ***要件***：カードベースのデザインで WKND サイトのホームページのデザインを刷新します。
2. AEM コンテンツエディターは、要件に基づいてカードベースのデザインで新しい WKND サイトのホームページを作成し、新しいホームページを公開します。

## マーケターアクティビティ

1. マーケターは、リダイレクトオファーをエクスペリエンスとして A／B ターゲットアクティビティを作成し、成功の目標と指標を追加した新しいホームページに 100％の web サイトトラフィックを割り当てます。
   1. Adobe Target ウィンドウから「**アクティビティ**」タブに移動します。
   2. 「**アクティビティの作成**」ボタンをクリックし、アクティビティの種類として **A/B テスト** を選択します。
      ![Adobe Target - アクティビティを作成](assets/personalization-use-case-2/create-ab-activity.png)
   3. **Web** チャネルを選択し、**Visual Experience Composer** を選択します。
   4. **アクティビティ URL** を入力し、「**次へ**」をクリックして Visual Experience Composer を開きます。
      ![Adobe Target - アクティビティの作成](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **Visual Experience Composer** を読み込むには、ブラウザーで「**安全でないスクリプトの読み込みを許可する**」を有効にして、ページをリロードします。
      ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. WKND サイトのホームページが Visual Experience Composer エディターで開いていることに注意してください。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **エクスペリエンス B** にポインタを合わせて、「他のオプションを表示」を選択します。
      ![エクスペリエンス B](assets/personalization-use-case-2/redirect-url.png)
   8. 「**URL にリダイレクト**」オプションを選択し、新しい WKND ホームページへの URL を入力します。（http://localhost:4503/content/wknd/en1.html）
      ![エクスペリエンス B](assets/personalization-use-case-2/redirect-url-2.png)
   9. 変更を&#x200B;**保存**&#x200B;して、アクティビティ作成の次のステップに進みます。
   10. 「**トラフィックの割り当て方法**」で「手動」を選択し、100％のトラフィックを&#x200B;**エクスペリエンス B** に割り当てます。
      ![エクスペリエンス B トラフィック](assets/personalization-use-case-2/traffic.png)
   11. 「**次へ**」をクリックします。
   12. アクティビティの&#x200B;**目標指標**を提供し、A/B テストを保存して閉じます。
      ![A／B テストの目標指標](assets/personalization-use-case-2/goal-metric.png)
   13. アクティビティに名前（**新しいデザインの WKND ホームページ**）を付けて、変更を保存します。
   14. アクティビティの詳細画面から、アクティビティを&#x200B;**アクティベート**します。
      ![アクティビティのアクティベート](assets/personalization-use-case-2/ab-activate.png)
   15. WKND ホームページ（http://localhost:4503/content/wknd/en.html）に移動すると、新しいデザインの WKND サイトのホームページ（http://localhost:4503/content/wknd/en1.html）にリダイレクトされます。
      ![新しいデザインの WKND ホームページ](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 概要

この章では、AEM でホストされているサイトページを新しいページにリダイレクトするアクティビティを、マーケターが Adobe Target を使用して作成しました。
