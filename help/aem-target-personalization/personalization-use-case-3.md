---
title: Adobe Target Visual Experience Composerを使用したパーソナライゼーション
seo-title: Adobe Target Visual Experience Composer(VEC)を使用したパーソナライゼーション
description: Adobe Target Visual Experience Composer(VEC)を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
seo-description: Adobe Target Visual Experience Composer(VEC)を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---


# Visual Experience Composerを使用したパーソナライゼーション

この章では、Target内からWebページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、**Visual Experience Composer**&#x200B;を使用したエクスペリエンスの作成について説明します。

## シナリオの概要

WKNDサイトのホームページには、ローカルのアクティビティや、カードレイアウトの形で都市の周りを取り巻く最善の方法が表示されます。 マーケターは、カードのレイアウトを変更して、ユーザーエンゲージメントへの影響やコンバージョンの促進を確認することで、ホームページを変更するタスクを割り当てられています。

### 関係するユーザー

この演習では、次のユーザーが関与し、管理アクセスが必要になる可能性のあるタスクを実行する必要があります。

* **コンテンツ作成者/コンテンツエディター** (Adobe Experience Manager)
* **マーケター** (Adobe Target/最適化チーム)

### WKNDサイトのホームページ

![AEM Targetシナリオ1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [4503でのAEMパブリ](./implementation.md#getting-aem) ッシュインスタンス
   * [Adobe Experience Platform Launchを使用したAdobe Targetとの統合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * [Adobe Target](https://experiencecloud.adobe.com)でプロビジョニングされたExperience Cloud

## マーケターアクティビティ

1. マーケターは、Adobe Target内にA/Bターゲットアクティビティを作成します。
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
   7. **エクスペ** リエンスAは、デフォルトのWKNDホームページを提供し、エクスペリエンスBのコンテンツレイアウ **トを編集します**。
      ![エクスペリエンス B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. カードレイアウトコンテナ(*Best Roasters*)の1つをクリックし、「**並べ替え**」オプションを選択します。
      ![コンテナの選択](assets/personalization-use-case-3/container-selection.png)
   9. 並べ替えたいコンテナをクリックし、目的の場所にドラッグ&amp;ドロップします。 1行目の1列目から1行目の3列目まで、*Best Roasters*&#x200B;コンテナを並べ替えます。 *Best Roasters*&#x200B;コンテナが&#x200B;*Photography Exhibitions*コンテナの隣に配置されます。
      ![コンテナの入れ替え](assets/personalization-use-case-3/container-swap.png)

      **スワップ後**
      ![コンテナの入れ替え](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同様に、他のカードコンテナの位置を並べ替えます。
      ![コンテナの入れ替え](assets/personalization-use-case-3/after-swap-all.png)
   11. また、カルーセルコンポーネントの下とカードレイアウトの上にヘッダーテキストを追加します。
   12. カルーセルコンテナをクリックし、「**後に挿入/HTML**」オプションを選択してHTMLを追加します。
      ![テキストを追加](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![テキストを追加](assets/personalization-use-case-3/after-changes.png)
   13. 「**次へ**」をクリックして、アクティビティを続行します。
   14. 「**トラフィック配分方法**」を手動で選択し、100%のトラフィックを&#x200B;**エクスペリエンスB**に割り当てます。
      ![エクスペリエンスBトラフィック](assets/personalization-use-case-2/traffic.png)
   15. 「**次へ**」をクリックします。
   16. アクティビティに&#x200B;**目標指標**を指定し、A/Bテストを保存して閉じます。
      ![A/Bテストの目標指標](assets/personalization-use-case-2/goal-metric.png)
   17. アクティビティの名前(**WKND Home Page Refresh**)を指定し、変更を保存します。
   18. アクティビティの詳細画面で、「**アクティビティをアクティブ化**」を必ず選択します。
      ![アクティビティを有効化](assets/personalization-use-case-3/save-activity.png)
   19. WKNDホームページ(http://localhost:4503/content/wknd/en.html)に移動すると、 WKNDホームページの更新A/Bテストアクティビティに追加した変更に気がつきます。
      ![WKNDホームページが更新されました](assets/personalization-use-case-3/activity-result.png)
   20. ブラウザーコンソールを開き、「ネットワーク」タブを調べて、 WKNDホームページの更新A/Bテストアクティビティのターゲット応答を探します。
      ![ネットワークアクティビティ](assets/personalization-use-case-3/activity-result.png)

## 概要

この章では、テストを実行するコードを変更することなく、Webページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composerを使用したエクスペリエンスを作成できました。
