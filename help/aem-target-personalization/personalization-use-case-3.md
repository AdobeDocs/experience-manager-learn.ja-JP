---
title: Visual Experience Composerを使用したパーソナライゼーション
seo-title: Visual Experience Composer(VEC)を使用したパーソナライゼーション
description: Adobe TargetVisual Experience Composer(VEC)を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示すエンドツーエンドのチュートリアルです。
seo-description: Adobe TargetVisual Experience Composer(VEC)を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示すエンドツーエンドのチュートリアルです。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---


# Visual Experience Composerを使用したパーソナライゼーション

この章では、ターゲット内からWebページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、**Visual Experience Composer**&#x200B;を使用したエクスペリエンスの作成について説明します。

## シナリオの概要

WKNDサイトのホームページは、市内のアクティビティ、または市内周辺で行う最善の方法をカードレイアウトの形で表示します。 マーケターには、ホームページの変更タスクが割り当てられています。カードのレイアウトを並べ替え直すことで、ユーザーの関与やコンバージョンへの影響を確認できます。

### 関係するユーザー

この練習では、次のユーザが関与し、管理者アクセスが必要となるタスクを実行する必要があります。

* **コンテンツプロデューサー/コンテンツエディタ** (Adobe Experience Manager)
* **マーケティング担当者** (Adobe Target/最適化チーム)

### WKNDサイトホームページ

![AEMターゲットシナリオ1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [AEM 4503での発行](./implementation.md#getting-aem) インスタンス
   * [Adobe Experience Platform Launchを使ってAdobe Targetと統合されたAEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * [Adobe Target](https://experiencecloud.adobe.com)でプロビジョニングされたExperience Cloud

## マーケティング担当者のアクティビティ

1. マーケターはAdobe Target内にA/Bターゲットアクティビティを作成します。
   1. Adobe Targetのウィンドウで、**アクティビティ**&#x200B;タブに移動します。
   2. 「**アクティビティを作成**」ボタンをクリックし、アクティビティの種類を「**A/Bテスト**」に選択します

      ![Adobe Target-アクティビティの作成](assets/personalization-use-case-2/create-ab-activity.png)
   3. 「**Web**」チャネルを選択し、「**Visual Experience Composer**」を選択します。
   4. **アクティビティURL**&#x200B;を入力し、「**次へ**」をクリックしてVisual Experience Composerを開きます。
      ![Adobe Target-アクティビティの作成](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **Visual Experience Composer**&#x200B;を読み込むには、ブラウザーで「安全でないスクリプトを読み込むことを許可&#x200B;**」を有効にし、ページを再読み込みします。**
      ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Visual Experience ComposerエディターでWKNDサイトホームページが開きます。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **エクスペリエンス** AはデフォルトのWKNDホームページです。エクスペリエンスBのコンテンツレイアウトを編集し **ます**。
      ![エクスペリエンス B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. カードレイアウトコンテナの1つ(*Best Roasters*)をクリックし、「**Rearrange**」オプションを選択します。
      ![コンテナの選択](assets/personalization-use-case-3/container-selection.png)
   9. 整列するコンテナをクリックし、目的の位置にドラッグ&amp;ドロップします。 *最良のばいせん業者*&#x200B;のコンテナを、1行目の1列目から1行目の3列目に並べ替えます。 *Best Roasters*&#x200B;コンテナは、*フォトグラフィー展覧会*コンテナの隣に置かれる。
      ![コンテナの入れ替え](assets/personalization-use-case-3/container-swap.png)

      **スワップ後**
      ![コンテナの入れ替え](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同様に、他のカードコンテナの位置を並べ替えます。
      ![コンテナの入れ替え](assets/personalization-use-case-3/after-swap-all.png)
   11. また、カルーセルコンポーネントの下、およびカードレイアウトの上にヘッダーテキストを追加します。
   12. カルーセルコンテナをクリックし、HTMLを追加するには、**後に挿入/HTML**オプションを選択します。
      ![テキストを追加](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![テキストを追加](assets/personalization-use-case-3/after-changes.png)
   13. 「**次へ**」をクリックして、アクティビティを続行します。
   14. **トラフィック配分方法**&#x200B;を手動で選択し、100%のトラフィックを&#x200B;**エクスペリエンスB**に割り当てます。
      ![エクスペリエンスBトラフィック](assets/personalization-use-case-2/traffic.png)
   15. 「**Next**」をクリックします。
   16. アクティビティに&#x200B;**目標指標**を指定し、A/Bテストを保存して閉じます。
      ![A/Bテスト目標指標](assets/personalization-use-case-2/goal-metric.png)
   17. アクティビティの名前(**WKNDホームページ更新**)を指定し、変更を保存します。
   18. アクティビティの詳細画面で、**アクティビティを**アクティブにします。
      ![アクティビティのアクティブ化](assets/personalization-use-case-3/save-activity.png)
   19. WKNDホームページ(http://localhost:4503/content/wknd/en.html)に移動すると、WKNDホームページの更新A/Bテストアクティビティに追加した変更が表示されます。
      ![WKNDホームページが更新されました](assets/personalization-use-case-3/activity-result.png)
   20. ブラウザコンソールを開き、ネットワークタブを調べて、WKNDホームページの更新A/Bテストアクティビティに対するターゲット応答を探します。
      ![ネットワークアクティビティ](assets/personalization-use-case-3/activity-result.png)

## 概要

この章では、マーケティング担当者が、テストを実行するコードを変更することなく、Webページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composerを使用してエクスペリエンスを作成できました。
