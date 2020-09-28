---
title: Visual Experience Composerを使用したパーソナライゼーション
seo-title: Visual Experience Composer(VEC)を使用したパーソナライゼーション
description: Adobe TargetVisual Experience Composer(VEC)を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示すエンドツーエンドのチュートリアルです。
seo-description: Adobe TargetVisual Experience Composer(VEC)を使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示すエンドツーエンドのチュートリアルです。
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---


# Visual Experience Composerを使用したパーソナライゼーション

この章では、ターゲット内からWebページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え **、変更することにより、** Visual Experience Composerを使用したエクスペリエンスの作成について説明します。

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
   * [AEM 4503で実行される発行インスタンス](./implementation.md#getting-aem)
   * [ADOBE EXPERIENCE PLATFORM LAUNCHを使ってAdobe Targetと統合されたAEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * [Adobe Targetで準備されたExperience Cloud](https://experiencecloud.adobe.com)

## マーケティング担当者のアクティビティ

1. マーケターはAdobe Target内にA/Bターゲットアクティビティを作成します。
   1. Adobe Target・ウィンドウから「 **アクティビティ** 」タブに移動します。
   2. 「 **アクティビティを作成** 」ボタンをクリックし、アクティビティタイプを **A/Bテストとして選択します**

      ![Adobe Target-アクティビティの作成](assets/personalization-use-case-2/create-ab-activity.png)
   3. 「 **Web** 」チャネルを選択し、「 **Visual Experience Composer**」を選択します。
   4. **アクティビティURLを入力し** 、「 **次へ** 」をクリックしてVisual Experience Composerを開きます。
      ![Adobe Target-アクティビティの作成](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. **Visual Experience Composerを読み込むには、ブラウザーで「安全でないスクリプトの読み込みを** 許可 **** 」を有効にし、ページを再読み込みします。
      ![エクスペリエンスのターゲット設定アクティビティ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Visual Experience ComposerエディターでWKNDサイトホームページが開きます。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **エクスペリエンスA** はデフォルトのWKNDホームページを提供します。エクスペリエンスB ****のコンテンツレイアウトを編集します。
      ![エクスペリエンス B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. カードレイアウトコンテナの1つ(「*Best Roasters*」)をクリックし、「 **Reargand** 」オプションを選択します。
      ![コンテナの選択](assets/personalization-use-case-3/container-selection.png)
   9. 整列するコンテナをクリックし、目的の位置にドラッグ&amp;ドロップします。 1行目の1列目から1行目の3列目まで、 *最良のばいせん* コンテナを並べ替えます。 「 *ベスト・ロースター* 」コンテナは、「 *フォトグラフィー展示会* 」コンテナの隣にあります。
      ![コンテナの入れ替え](assets/personalization-use-case-3/container-swap.png)

      **スワップ後**
      ![コンテナの入れ替え](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同様に、他のカードコンテナの位置を並べ替えます。
      ![コンテナの入れ替え](assets/personalization-use-case-3/after-swap-all.png)
   11. また、カルーセルコンポーネントの下、およびカードレイアウトの上にヘッダーテキストを追加します。
   12. カルーセルコンテナをクリックし、「 **後に挿入」/「HTML** 」オプションを選択してHTMLを追加します。
      ![テキストを追加](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![テキストを追加](assets/personalization-use-case-3/after-changes.png)
   13. 「 **次へ** 」をクリックしてアクティビティに進みます。
   14. 「 **トラフィック配分方法** 」を手動で選択し、100%のトラフィックを **エクスペリエンスBに割り当てます**。
      ![エクスペリエンスBトラフィック](assets/personalization-use-case-2/traffic.png)
   15. 「**次へ**」をクリックします。
   16. アクティビティの **目標指標を指定し** 、A/Bテストを保存して閉じます。
      ![A/Bテスト目標指標](assets/personalization-use-case-2/goal-metric.png)
   17. アクティビティの名前(**WKNDホームページ更新**)を指定し、変更を保存します。
   18. アクティビティの詳細画面で、「アクティビティを **アクティブ化** 」を確認します。
      ![アクティビティのアクティブ化](assets/personalization-use-case-3/save-activity.png)
   19. WKNDホームページ(http://localhost:4503/content/wknd/en.html)に移動すると、WKNDホームページの更新A/Bテストアクティビティに追加した変更が表示されます。
      ![WKNDホームページが更新されました](assets/personalization-use-case-3/activity-result.png)
   20. ブラウザコンソールを開き、ネットワークタブを調べて、WKNDホームページの更新A/Bテストアクティビティに対するターゲット応答を探します。
      ![ネットワークアクティビティ](assets/personalization-use-case-3/activity-result.png)

## 概要

この章では、マーケティング担当者が、テストを実行するコードを変更することなく、Webページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composerを使用してエクスペリエンスを作成できました。
