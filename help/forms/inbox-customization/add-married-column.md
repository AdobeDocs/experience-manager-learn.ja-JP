---
title: カスタム列の追加
description: ワークフローの追加データを表示するためのカスタム列の追加
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 75
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '276'
ht-degree: 100%

---

# カスタム列の追加

インボックスにワークフローデータを表示するには、ワークフローで変数を定義し、設定する必要があります。変数の値は、タスクがユーザーに割り当てられる前に設定する必要があります。素早く作業を開始できるように、AEM サーバーにデプロイする準備が整ったサンプルワークフローを用意しています。

* [AEM へのログイン](http://localhost:4502/crx/de/index.jsp)
* [レビューワークフローの読み込み](assets/review-workflow.zip)
* [ワークフローのレビュー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

このワークフローには 2 つの変数（isMarried と income）が定義されており、それらの値は変数設定コンポーネントを使用して設定されます。これらの変数は、AEM インボックスに追加する列として使用できるようになります。

## サービスの作成

インボックスに表示する必要のある列ごとに、サービスを記述する必要があります。次のサービスでは、isMarried 変数の値を表示する列を追加できます。

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
>上記のコードを機能させるには、プロジェクトに AEM 6.5.5 Uber.jar を含める必要があります

![uber-jar](assets/uber-jar.PNG)

## サーバーでテストします

* [AEM web コンソールにログインします](http://localhost:4502/system/console/bundles)
* [インボックスカスタマイズバンドルをデプロイして開始します](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [インボックスを開きます](http://localhost:4502/aem/inbox)
* 「_作成_」ボタンの横にある「_リスト表示_」アイコンをクリックして管理コントロールを開きます
* 既婚の列をインボックスに追加し、変更を保存します
* [FormsAndDocuments UI に移動します](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* _作成_&#x200B;メニューから&#x200B;_ファイルをアップロード_&#x200B;を選択して、[サンプルフォームを読み込みます](assets/snap-form.zip)
* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _婚姻ステータス_を選択し、フォームを送信します
  [インボックスを表示します](http://localhost:4502/aem/inbox)

フォームを送信するとワークフローがトリガーされ、タスクが「admin」ユーザーに割り当てられます。このスクリーンショットに示すように、「既婚」列の下に値が表示されます。

![既婚列](assets/married-column.PNG)

## 次の手順

[既婚列の表示](./use-sightly-template.md)
