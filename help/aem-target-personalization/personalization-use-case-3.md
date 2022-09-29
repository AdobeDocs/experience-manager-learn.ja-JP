---
title: Adobe Target Visual Experience Composer を使用したパーソナライゼーション
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Adobe Target Visual Experience Composer(VEC) を使用してパーソナライズされたエクスペリエンスを作成して配信する方法を示す、エンドツーエンドのチュートリアルです。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 2%

---

# Visual Experience Composer を使用したパーソナライゼーション

この章では、 **Visual Experience Composer** Target 内から Web ページのレイアウトやコンテンツをドラッグ&amp;ドロップ、入れ替え、変更する。

## シナリオの概要

WKND サイトのホームページには、市内のアクティビティや最善の行動がカードレイアウトで表示されます。 マーケターは、カードのレイアウトを変更して、ユーザーエンゲージメントに与える影響を確認し、コンバージョンを促進することで、ホームページを変更するタスクを割り当てられました。

### 関連するユーザー

この練習では、次のユーザーが関与し、管理者アクセスが必要なタスクを実行する必要があります。

* **コンテンツ作成者/コンテンツエディター** (Adobe Experience Manager)
* **マーケター** (Adobe Target/最適化チーム )

### WKND サイトホームページ

![AEM Target シナリオ 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [AEMパブリッシュインスタンス](./implementation.md#getting-aem) 4503 で実行中
   * [Adobe Experience Platform Launchを使用したAdobe Targetとの統合](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * でプロビジョニングされたExperience Cloud [Adobe Target](https://experiencecloud.adobe.com)

## マーケターアクティビティ

1. マーケターがAdobe Target内に A/B ターゲットアクティビティを作成します。
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
   7. **エクスペリエンス A** はデフォルトの WKND ホームページを提供し、次の用にコンテンツレイアウトを編集します。 **エクスペリエンス B**.
      ![エクスペリエンス B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. カードレイアウトコンテナ (*ベストばいせん業者*) をクリックし、「 **並べ替え** オプション。
      ![コンテナの選択](assets/personalization-use-case-3/container-selection.png)
   9. 並べ替えたいコンテナをクリックし、目的の場所にドラッグ&amp;ドロップします。 並べ替えましょう *ベストばいせん業者* 1 番目の行の 1 番目の列から 1 番目の行の 3 番目の列までのコンテナ。 次に、 *ベストばいせん業者* コンテナが次の隣にある *写真展* コンテナ。
      ![コンテナの入れ替え](assets/personalization-use-case-3/container-swap.png)

      **スワップ後**
      ![入れ替えられたコンテナ](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同様に、他のカードコンテナの位置を並べ替えます。
      ![入れ替えられたコンテナ](assets/personalization-use-case-3/after-swap-all.png)
   11. また、カルーセルコンポーネントの下、およびカードレイアウトの上にヘッダーテキストを追加します。
   12. カルーセルコンテナをクリックし、 **後に挿入/HTML** オプションを使用してHTMLを追加します。
      ![テキストを追加](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![テキストを追加](assets/personalization-use-case-3/after-changes.png)
   13. クリック **次へ** をクリックして、アクティビティを続行します。
   14. を選択します。 **トラフィック配分方法** 手動で、100%のトラフィックをに割り当てる **エクスペリエンス B**.
      ![エクスペリエンス B トラフィック](assets/personalization-use-case-2/traffic.png)
   15. 「**次へ**」をクリックします。
   16. 提供 **目標指標** をクリックし、A/B テストを保存して閉じます。
      ![A/B テストの目標指標](assets/personalization-use-case-2/goal-metric.png)
   17. 名前 (**WKND ホームページの更新**) をクリックし、変更を保存します。
   18. アクティビティの詳細画面で、 **有効化** アクティビティを選択します。
      ![アクティビティを有効化](assets/personalization-use-case-3/save-activity.png)
   19. WKND ホームページ (http://localhost:4503/content/wknd/en.html) に移動し、 WKND ホームページの更新 A/B テストアクティビティに追加した変更に気がつきます。
      ![WKND ホームページが更新されました](assets/personalization-use-case-3/activity-result.png)
   20. ブラウザーコンソールを開き、「ネットワーク」タブを調べて、 WKND ホームページの更新 A/B テストアクティビティに対する Target 応答を探します。
      ![ネットワークアクティビティ](assets/personalization-use-case-3/activity-result.png)

## 概要

この章では、Web ページのレイアウトとコンテンツを、テストを実行するコードを変更することなくドラッグ&amp;ドロップ、入れ替え、変更することで、Visual Experience Composer を使用してエクスペリエンスを作成できました。
