---
title: カスタム列の追加
description: ワークフローの追加データを表示するためのカスタム列の追加
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 8%

---

# カスタム列の追加

インボックスにワークフローデータを表示するには、ワークフローで変数を定義し、設定する必要があります。 変数の値は、タスクがユーザーに割り当てられる前に設定する必要があります。 優先的に作業を開始するために、AEMサーバーにデプロイする準備が整ったサンプルワークフローを用意しています。

* [AEMにログイン](http://localhost:4502/crx/de/index.jsp)
* [レビューワークフローのインポート](assets/review-workflow.zip)
* [ワークフローの確認](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

このワークフローには 2 つの変数（isMarried と income）が定義され、その値は変数設定コンポーネントを使用して設定されます。 これらの変数は、AEMインボックスに追加する列として使用可能になります

## サービスを作成

インボックスに表示する必要のある列ごとに、サービスを記述する必要があります。 次のサービスでは、isMarried 変数の値を表示する列を追加できます

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
>上記のコードを機能させるには、プロジェクトにAEM 6.5.5 Uber.jar を含める必要があります

![uber-jar](assets/uber-jar.PNG)

## サーバーでテストする

* [AEM Web コンソールにログイン](http://localhost:4502/system/console/bundles)
* [インボックスカスタマイズバンドルをデプロイして開始します](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [インボックスを開く](http://localhost:4502/aem/inbox)
* 次をクリックして管理コントロールを開く： _リスト表示_ 隣のアイコン _作成_ ボタン
* 既婚の列をインボックスに追加し、変更を保存する
* [FormsAndDocuments UI に移動](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [サンプルフォームをインポートします](assets/snap-form.zip) 選択する _ファイルのアップロード_ から _作成_ メニュー
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* を選択します。 _配偶者の有無_ フォームを送信します。
   [インボックスを表示](http://localhost:4502/aem/inbox)

フォームを送信すると、トリガーがワークフローに送信され、タスクが「管理者」ユーザーに割り当てられます。 このスクリーンショットに示すように、「Married」列の下に値が表示されます。

![既婚欄](assets/married-column.PNG)
