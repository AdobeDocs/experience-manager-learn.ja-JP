---
title: インボックスのカスタマイズ
description: sightlyテンプレートを使用して、ワークフローの追加データを表示するカスタム列を追加します
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
source-wordcount: '294'
ht-degree: 9%

---

# sightlyテンプレートを使用したインボックスデータの表示

sightlyテンプレートを使用して、インボックス列に表示するデータの形式を設定できます。 この例では、「income」列の値に応じてcoral-uiアイコンを表示します。 次のスクリーンショットは、所得列でのアイコンの使用を示しています
![income-icons](assets/income-column.PNG)

[カスタムの](assets/sightly-template.zip) coral uiアイコンの表示に使用するsightlyテンプレートは、この記事の一部として提供されています。

## Sightlyテンプレート

次に、 sightlyテンプレートを示します。 テンプレート内のコードは、所得に応じてアイコンを表示します。 アイコンは、AEMに付属する[coral uiアイコンライブラリ](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)の一部として使用できます。

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

次のコードは、所得列を表示するサービス実装です。

12行目は、列をsightlyテンプレートに関連付けます

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
>この記事では、このシリーズの[前の記事](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md)の[サンプルワークフロー](assets/review-workflow.zip)と[サンプルフォーム](assets/snap-form.zip)をインストール済みであることを前提としています。

* [crxに管理者ユーザーとしてログインします。](http://localhost:4502/crx/de/index.jsp)
* [sightlyテンプレートのインポート](assets/sightly-template.zip)
* [AEM Webコンソールにログインします。](http://localhost:4502/system/console/bundles)
* [インボックスカスタマイズバンドルのデプロイと開始](assets/income-column-customization.jar)
* [インボックスを開く](http://localhost:4502/aem/inbox)
* 「作成」ボタンの横にある「リスト表示」をクリックして、管理コントロールを開きます。
* インボックスに所得列を追加し、変更を保存します
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _婚姻状況_&#x200B;を選択し、フォームを送信します
* [インボックスの表示](http://localhost:4502/aem/inbox)

フォームを送信すると、トリガーがワークフローに送信され、タスクが「管理者」ユーザーに割り当てられます。 「所得」列に適切なアイコンが表示されます
