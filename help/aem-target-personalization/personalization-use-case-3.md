---
title: Adobe Target Visual Experience Composer を使用したパーソナライゼーション
description: パーソナライズされたエクスペリエンスを Adobe Target Visual Experience Composer（VEC）で作成して配信する方法を示す、エンドツーエンドのチュートリアルです。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 112
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '581'
ht-degree: 100%

---

# Visual Experience Composer を使用したパーソナライズ機能

この章では、**Visual Experience Composer** を使用することにより、Target 内から web ページのレイアウトとコンテンツをドラッグ＆ドロップ、スワップ、変更してエクスペリエンスを作成する方法について説明します。

## シナリオの概要

WKND サイトのホームページには、カードレイアウトの形式で、地域のアクティビティや市内のおすすめアクティビティが表示されます。マーケターとして、あなたはホームページを改修する仕事を任されました。カードレイアウトを再配置して、それがユーザーのエンゲージメントにどのように影響し、コンバージョンがどのように促進されるかを確認します。

### 関係するユーザー

この演習では、次のユーザーが関与し、管理者アクセスが必要なタスクを実行する必要があります。

* **コンテンツプロデューザー／コンテンツ編集者**（Adobe Experience Manager）
* **マーケター**（Adobe Target／最適化チーム）

### WKND サイトのホームページ

![AEM Target シナリオ 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * 4503 で実行されている [AEM パブリッシュインスタンス](./implementation.md#getting-aem)
   * [タグを使用して AEM を Adobe Target と統合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織の Adobe Experience Cloud へのアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * [Adobe Target](https://experiencecloud.adobe.com) でプロビジョニングされた Experience Cloud

## マーケターアクティビティ

1. マーケターが Adobe Target 内に A/B ターゲットアクティビティを作成します。
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
   7. **エクスペリエンス A** はデフォルトの WKND ホームページを提供します。**エクスペリエンス B** のコンテンツレイアウトを編集しましょう。
      ![エクスペリエンス B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. カードレイアウトコンテナの 1 つ（*最高のロースター*）をクリックし、**並べ替え**オプションを選択します。
      ![コンテナの選択](assets/personalization-use-case-3/container-selection.png)
   9. 並べ替えたいコンテナをクリックし、目的の場所にドラッグ＆ドロップします。*最高のロースター*&#x200B;コンテナを 1 行目の 1 列目から 1 行目の 3 列目に並べ替えましょう。現在、*最高のロースター*&#x200B;コンテナは&#x200B;*写真展*コンテナの隣にあります。
      ![コンテナの入れ替え](assets/personalization-use-case-3/container-swap.png)
      **並べ替え後**
      ![入れ替えられたコンテナ](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同じように、他のカードコンテナの位置を並べ替えます。
      ![入れ替えられたコンテナ](assets/personalization-use-case-3/after-swap-all.png)
   11. また、カルーセルコンポーネントの下とカードレイアウトの上に、ヘッダーテキストを追加してみましょう。
   12. カルーセルコンテナをクリックし、**後に挿入／HTML** オプションを選択して HTML を追加します。
      ![テキストを追加](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![テキストを追加](assets/personalization-use-case-3/after-changes.png)
   13. 「**次へ**」をクリックして、アクティビティを続行します。
   14. **トラフィックの割り当て方法**&#x200B;として手動を選択し、100％のトラフィックを&#x200B;**エクスペリエンス B** に割り当てます。
      ![エクスペリエンス B トラフィック](assets/personalization-use-case-2/traffic.png)
   15. 「**次へ**」をクリックします。
   16. アクティビティの&#x200B;**目標指標**を提供し、A/B テストを保存して閉じます。
      ![A/B テストの目標指標](assets/personalization-use-case-2/goal-metric.png)
   17. アクティビティに名前（**WKND ホームページ更新**）を付けて、変更を保存します。
   18. アクティビティの詳細画面から、アクティビティを&#x200B;**アクティベート**してください。
      ![アクティビティをアクティベート](assets/personalization-use-case-3/save-activity.png)
   19. WKND ホームページ（http://localhost:4503/content/wknd/en.html）に移動すると、WKND ホームページ更新 A/B テストアクティビティに追加されたことがわかります。
      ![WKND ホームページが更新されました](assets/personalization-use-case-3/activity-result.png)
   20. ブラウザーコンソールを開き、「ネットワーク」タブを調べて、 WKND ホームページの更新 A/B テストアクティビティに対するターゲット応答を探します。
      ![ネットワークアクティビティ](assets/personalization-use-case-3/activity-result.png)

## 概要

この章では、テストを実行するコードを変更することなく、web ページのレイアウトとコンテンツをドラッグ＆ドロップ、入れ替え、変更することで、Visual Experience Composer を使用してエクスペリエンスを作成できました。
