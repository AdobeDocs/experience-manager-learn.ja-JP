---
title: ブロックのオーサリング
description: ユニバーサルエディターを使用して Edge Delivery Services ブロックを作成します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 77beb9f543bc6dc8c1ab4993c969375ce3e238e8
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 93%

---

# ブロックのオーサリング

[ティーザーブロックの JSON](./5-new-block.md) を `teaser` 分岐にプッシュすると、ブロックは AEM ユニバーサルエディターで編集できるようになります。

開発中のブロックのオーサリングは、次のようないくつかの理由で重要です。

1. ブロックの定義とモデルが正確であることを確認します。
1. これにより、開発者は開発の基盤となるブロックのセマンティック HTML を確認できます。
1. これにより、プレビュー環境に対するコンテンツとセマンティック HTML の両方のデプロイメントが可能になり、より迅速なブロック開発がサポートされます。

## `teaser` 分岐のコードを使用してユニバーサルエディターを開く

1. AEM オーサーにログインします。
2. **Sites** に移動して、[前の章](./2-new-aem-site.md)で作成したサイト（WKND（ユニバーサルエディター））を選択します。

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. ページを作成または編集して新しいブロックを追加し、ローカル開発をサポートするコンテキストが使用できることを確認します。ページはサイト内のどこにでも作成できますが、多くの場合、新しい作業内容ごとに個別のページを作成することが最適です。**分岐**&#x200B;という名前の新しい「フォルダー」ページを作成します。各サブページは、同じ名前の Git 分岐の開発をサポートするのに使用されます。

   ![AEM Sites - 分岐を作成ページ](./assets/6-author-block/branches-page-3.png)

4. **分岐**&#x200B;ページで、開発分岐名と一致する&#x200B;**ティーザー**&#x200B;というタイトルの新しいページを作成し、「**開く**」をクリックしてページを編集します。

   ![AEM Sites - ティーザーを作成ページ](./assets/6-author-block/teaser-page-3.png)

5. URL に `?ref=teaser` を追加して、ユニバーサルエディターを更新し、`teaser` 分岐からコードを読み込みます。`#` 記号の&#x200B;**前**&#x200B;にクエリパラメーターを追加します。

   ![ユニバーサルエディター – ティーザー分岐の選択](./assets/6-author-block/select-branch.png)

6. **メイン**&#x200B;の下にある最初のセクションを選択し、「**追加**」ボタンをクリックして、**ティーザー**&#x200B;ブロックを選択します。

   ![ユニバーサルエディター - ブロックの追加](./assets/6-author-block/add-teaser-2.png)

7. キャンバス上で、新しく追加されたティーザーを選択し、右側のフィールドまたはインライン編集機能を使用してフィールドを作成します。

   ![ユニバーサルエディター - ブロックの作成](./assets/6-author-block/author-block.png)

8. オーサリングが完了したら、ユニバーサルエディターの右上にある「**公開**」ボタンを選択し、「**プレビュー** に公開」を選択して、変更内容をプレビュー環境に公開します。 変更は、web サイトの `aem.page` ドメインに公開されます。
   ![AEM Sites - 公開またはプレビュー](./assets/6-author-block/publish-to-preview.png)

9. 変更がプレビューに公開されるまで待ってから、[AEM CLI](./3-local-development-environment.md#install-the-aem-cli) 経由で [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser) の web ページを開きます。

   ![ローカルサイト - 更新](./assets/6-author-block/preview.png)

これで、作成したティーザーブロックのコンテンツとセマンティック HTML がプレビュー web サイトで使用でき、ローカル開発環境で AEM CLI を使用して開発する準備が整いました。
