---
title: Adobe Targetを使用したパーソナライゼーション
seo-title: Adobe Targetを使用したパーソナライゼーション
description: Adobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、提供する方法を示す、エンドツーエンドのチュートリアルです。
seo-description: Adobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、提供する方法を示す、エンドツーエンドのチュートリアルです。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# Adobe Targetを使用した完全なWebページエクスペリエンスのパーソナライズ

前の章では、エクスペリエンスフラグメントとして作成され、AEMからHTMLオファーとして書き出されたコンテンツを使用して、Adobe Target内で位置情報ベースのアクティビティを作成する方法を学びました。

この章では、Adobe Targetを使用して、AEMでホストされるサイトページを新しいページにリダイレクトするアクティビティの作成について説明します。

## シナリオの概要

WKNDサイトのデザインが変更され、現在のホームページの訪問者を新しいホームページにリダイレクトしたいと考えています。 同時に、再設計されたホームページがユーザーエンゲージメントと売上高を向上させる方法も理解します。 マーケターには、訪問者を新しいホームページにリダイレクトするアクティビティを作成するタスクが割り当てられています。 WKNDサイトのホームページを参照し、Adobe Targetを使用してアクティビティを作成する方法を学びます。

### 関係するユーザー

この演習では、次のユーザーが関与し、管理アクセスが必要になる可能性のあるタスクを実行する必要があります。

* **コンテンツ作成者/コンテンツエディター** (Adobe Experience Manager)
* **マーケター** (Adobe Target/最適化チーム)

### WKNDサイトのホームページ

![AEM Targetシナリオ1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEMオーサーインスタンスとパブリッシュイン](./implementation.md#getting-aem) スタンスは、それぞれlocalhost 4502と4503で実行されます。
   * [Adobe Experience Platform Launchを使用したAdobe Targetとの統合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

## コンテンツエディターのアクティビティ

1. マーケターがWKNDホームページのデザイン変更に関するディスカッションをAEM Content Editorで開始し、要件の詳細を説明します。
   * ***要件*** :カードベースのデザインでWKNDサイトのホームページを再設計します。
2. 要件に基づいて、AEM Content Editorは、カードベースのデザインを持つ新しいWKNDサイトのホームページを作成し、新しいホームページを公開します。

## マーケターアクティビティ

1. マーケティング担当者は、リダイレクトオファーをエクスペリエンスとし、100%のWebサイトトラフィックを新しいホームページに割り当てて、成功目標と指標を追加したA/Bターゲットアクティビティを作成します。
   1. Adobe Targetウィンドウで、「**アクティビティ**」タブに移動します。
   2. 「**アクティビティを作成**」ボタンをクリックし、アクティビティのタイプを「**A/Bテスト**」と選択します。

      ![Adobe Target — アクティビティを作成](assets/personalization-use-case-2/create-ab-activity.png)
   3. **Web**&#x200B;チャネルを選択し、**Visual Experience Composer**&#x200B;を選択します。
   4. **アクティビティURL**&#x200B;を入力し、「**次へ**」をクリックしてVisual Experience Composerを開きます。
      ![Adobe Target — アクティビティを作成](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **Visual Experience Composer**&#x200B;を読み込むには、ブラウザーで「**安全でないスクリプトを読み込む**を許可」を有効にして、ページを再読み込みします。
      ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. WKNDサイトのホームページがVisual Experience Composerエディターで開いていることに注意してください。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **エクスペリエンスB**の上にマウスポインターを置き、「その他のオプションを表示」を選択します。
      ![エクスペリエンス B](assets/personalization-use-case-2/redirect-url.png)
   8. 「**URLにリダイレクト**」オプションを選択し、新しいWKNDホームページのURLを入力します。 (http://localhost:4503/content/wknd/en1.html)
      ![エクスペリエンス B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** 変更を保存し、次のアクティビティ作成手順に進みます。
   10. 「**トラフィック配分方法**」を手動で選択し、100%のトラフィックを&#x200B;**エクスペリエンスB**に割り当てます。
      ![エクスペリエンスBトラフィック](assets/personalization-use-case-2/traffic.png)
   11. 「**次へ**」をクリックします。
   12. アクティビティに&#x200B;**目標指標**を指定し、A/Bテストを保存して閉じます。
      ![A/Bテストの目標指標](assets/personalization-use-case-2/goal-metric.png)
   13. アクティビティの名前(**WKND Home Page Redesign**)を指定し、変更を保存します。
   14. アクティビティの詳細画面で、「**アクティビティをアクティブ化**」を必ず選択します。
      ![アクティビティを有効化](assets/personalization-use-case-2/ab-activate.png)
   15. WKNDホームページ(http://localhost:4503/content/wknd/en.html)に移動すると、再設計されたWKNDサイトホームページ(http://localhost:4503/content/wknd/en1.html)にリダイレクトされます。
      ![WKNDホームページの再設計](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 概要

この章では、マーケターが、Adobe Targetを使用してAEM上でホストされるサイトページを新しいページにリダイレクトするアクティビティを作成できました。
