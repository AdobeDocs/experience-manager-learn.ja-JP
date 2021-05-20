---
title: インボックスのカスタマイズ
description: ワークフローの追加データを表示するカスタム列の追加
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 8%

---


# カスタム列の追加

インボックスにワークフローデータを表示するには、ワークフローで変数を定義して設定する必要があります。 変数の値は、タスクをユーザーに割り当てる前に設定する必要があります。 最初に、AEMサーバーにデプロイする準備が整ったサンプルワークフローを用意しました。

* [AEMにログイン](http://localhost:4502/crx/de/index.jsp)
* [レビューワークフローのインポート](assets/review-workflow.zip)
* [ワークフローの確認](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

このワークフローには、2つの変数（isMarriedとincome）が定義され、その値は変数設定コンポーネントを使用して設定されます。 これらの変数は、AEMインボックスに追加する列として使用可能になります

## サービスの作成

インボックスに表示する必要がある列ごとに、サービスを書く必要があります。 次のサービスでは、 isMarried変数の値を表示する列を追加できます

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>上記のコードを動作させるには、プロジェクトにAEM 6.5.5 Uber.jarを含める必要があります

![uber-jar](assets/uber-jar.PNG)

## サーバーでテストする

* [AEM Webコンソールにログインします。](http://localhost:4502/system/console/bundles)
* [インボックスカスタマイズバンドルのデプロイと開始](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [インボックスを開く](http://localhost:4502/aem/inbox)
* _リスト表示_&#x200B;アイコン（「_作成_」ボタンの横）をクリックして、管理コントロールを開きます。
* 既婚の列をインボックスに追加し、変更を保存する
* [「フォームとドキュメント」 UIに移動します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [「作成」メニューから「ファ](assets/snap-form.zip) イルのアップロード」を選択し _て、サンプルフ_ ォームをイン __ ポートします
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _婚姻状況_を選択し、フォームを送信します
   [インボックスの表示](http://localhost:4502/aem/inbox)

フォームを送信すると、トリガーがワークフローに送信され、タスクが「管理者」ユーザーに割り当てられます。 このスクリーンショットに示すように、「既婚」列の下に値が表示されます

![既婚欄](assets/married-column.PNG)
