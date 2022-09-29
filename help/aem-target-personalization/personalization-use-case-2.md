---
title: Adobe Targetを使用したパーソナライゼーション
seo-title: Personalization using Adobe Target
description: Adobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# Adobe Targetを使用した完全な Web ページエクスペリエンスのパーソナライズ

前の章では、エクスペリエンスフラグメントとして作成し、AEMからHTMLオファーとして書き出したコンテンツを使用して、Adobe Target内で位置情報ベースのアクティビティを作成する方法を学びました。

この章では、Adobe Targetを使用して、AEMでホストされているサイトページを新しいページにリダイレクトするアクティビティの作成について説明します。

## シナリオの概要

WKND サイトがホームページのデザインを一新し、現在のホームページの訪問者を新しいホームページにリダイレクトしたいと考えています。 同時に、再設計されたホームページがユーザーエンゲージメントと売上高の改善にどのように役立つかも理解します。 マーケターは、訪問者を新しいホームページにリダイレクトするアクティビティを作成するタスクを割り当てられています。 WKND サイトのホームページを参照し、Adobe Targetを使用してアクティビティを作成する方法を学びましょう。

### 関連するユーザー

この練習では、次のユーザーが関与し、管理者アクセスが必要なタスクを実行する必要があります。

* **コンテンツ作成者/コンテンツエディター** (Adobe Experience Manager)
* **マーケター** (Adobe Target/最適化チーム )

### WKND サイトホームページ

![AEM Target シナリオ 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEMオーサーインスタンスとパブリッシュインスタンス](./implementation.md#getting-aem) localhost 4502 と 4503 でそれぞれ実行している
   * [Adobe Experience Platform Launchを使用したAdobe Targetとの統合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

## コンテンツエディターアクティビティ

1. マーケターがAEMコンテンツエディターで WKND ホームページの再設計に関するディスカッションを開始し、要件の詳細を説明します。
   * ***要件*** :カードベースのデザインで WKND サイトホームページを再設計します。
2. 要件に基づいて、AEM Content Editor は、カードベースのデザインを持つ新しい WKND サイトホームページを作成し、新しいホームページを公開します。

## マーケターアクティビティ

1. マーケティング担当者は、リダイレクトオファーをエクスペリエンスとし、100%の Web サイトトラフィックを新しいホームページに割り当てて、成功の目標と指標を追加した A/B ターゲットアクティビティを作成します。
   1. Adobe Targetウィンドウで、に移動します。 **アクティビティ** タブをクリックします。
   2. クリック **アクティビティを作成** ボタンをクリックし、「アクティビティタイプ」として「 **A/B テスト**

      ![Adobe Target — アクティビティを作成](assets/personalization-use-case-2/create-ab-activity.png)
   3. を選択します。 **Web** チャネルと選択 **Visual Experience Composer**.
   4. 次を入力します。 **アクティビティ URL** をクリックし、 **次へ** をクリックして Visual Experience Composer を開きます。
      ![Adobe Target — アクティビティを作成](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. の場合 **Visual Experience Composer** 読み込むには、有効にするには **安全でないスクリプトの読み込みを許可** ブラウザーで、ページをリロードします。
      ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. WKND サイトのホームページが Visual Experience Composer エディターで開いていることに注意してください。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. 上にマウスポインターを置く **エクスペリエンス B** 「他のオプションを表示」を選択します。
      ![エクスペリエンス B](assets/personalization-use-case-2/redirect-url.png)
   8. 選択 **URL にリダイレクト** 」オプションを選択し、新しい WKND ホームページの URL を入力します。 (http://localhost:4503/content/wknd/en1.html)
      ![エクスペリエンス B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **保存** 変更を加え、アクティビティの作成の次の手順に進みます。
   10. を選択します。 **トラフィック配分方法** 手動で、100%のトラフィックをに割り当てる **エクスペリエンス B**.
      ![エクスペリエンス B トラフィック](assets/personalization-use-case-2/traffic.png)
   11. 「**次へ**」をクリックします。
   12. 提供 **目標指標** をクリックし、A/B テストを保存して閉じます。
      ![A/B テストの目標指標](assets/personalization-use-case-2/goal-metric.png)
   13. 名前 (**WKND ホームページの再設計**) をクリックし、変更を保存します。
   14. アクティビティの詳細画面で、 **有効化** アクティビティを選択します。
      ![アクティビティを有効化](assets/personalization-use-case-2/ab-activate.png)
   15. WKND ホームページ (http://localhost:4503/content/wknd/en.html) に移動すると、再設計された WKND サイトホームページ (http://localhost:4503/content/wknd/en1.html) にリダイレクトされます。
      ![WKND ホームページの再設計](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 概要

この章では、マーケターが、Adobe Targetを使用してAEMでホストされるサイトページを新しいページにリダイレクトするアクティビティを作成できました。
