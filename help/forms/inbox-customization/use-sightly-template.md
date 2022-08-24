---
title: sightly テンプレートを使用したインボックスデータの表示
description: sightly テンプレートを使用して、ワークフローの追加データを表示するカスタム列を追加します
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 8%

---

# sightly テンプレートを使用したインボックスデータの表示

sightly テンプレートを使用すると、インボックス列に表示するデータの形式を設定できます。 この例では、「income」列の値に応じた coral-ui アイコンを表示します。 次のスクリーンショットは、所得列でのアイコンの使用を示しています
![income-icons](assets/income-column.PNG)

[sightly テンプレート](assets/sightly-template.zip) カスタム coral ui アイコンの表示に使用する方法は、この記事の一部として提供されています。

## Sightly テンプレート

次に、 sightly テンプレートを示します。 テンプレート内のコードには、収入に応じてアイコンが表示されます。 アイコンは、 [coral ui アイコンライブラリ](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) AEMに付属しています。

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## サービスの実装

次のコードは、所得列を表示するためのサービス実装です。

行 12 は、列を sightly テンプレートに関連付けます

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## サーバーでテストする

>[!NOTE]
>
>この記事では、 [サンプルワークフロー](assets/review-workflow.zip) および [サンプルフォーム](assets/snap-form.zip) から [前の記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) を参照してください。

* [管理者ユーザーとして crx にログインします。](http://localhost:4502/crx/de/index.jsp)
* [sightly テンプレートのインポート](assets/sightly-template.zip)
* [AEM Web コンソールにログイン](http://localhost:4502/system/console/bundles)
* [インボックスカスタマイズバンドルをデプロイして開始します](assets/income-column-customization.jar)
* [インボックスを開く](http://localhost:4502/aem/inbox)
* 「作成」ボタンの横のリスト表示をクリックして、管理コントロールを開きます。
* インボックスに収益列を追加し、変更を保存します
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* を選択します。 _配偶者の有無_ フォームを送信します。
* [インボックスを表示](http://localhost:4502/aem/inbox)

フォームを送信すると、トリガーがワークフローに送信され、タスクが「管理者」ユーザーに割り当てられます。 「所得」列の下に適切なアイコンが表示されます。
