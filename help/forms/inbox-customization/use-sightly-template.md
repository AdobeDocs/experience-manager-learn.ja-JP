---
title: Sightly テンプレートを使用したインボックスデータの表示
description: Sightly テンプレートを使用して、ワークフローの追加データを表示するカスタム列を追加します
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '256'
ht-degree: 100%

---

# Sightly テンプレートを使用したインボックスデータの表示

Sightly テンプレートを使用すると、インボックス列に表示するデータの形式を設定できます。 この例では、「income」列の値に応じた coral-ui アイコンを表示します。 次のスクリーンショットは、「income」列でのアイコンの使用を示しています
![income-icons](assets/income-column.PNG)

カスタムの coral ui アイコンを表示するために使用される [Sightly テンプレート](assets/sightly-template.zip) は、この記事の一部として提供されています。

## Sightly テンプレート

次に、Sightly テンプレートを示します。テンプレート内のコードには、収入に応じてアイコンが表示されます。 アイコンは、AEM に付属の [coral ui アイコンライブラリ](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)の一部として利用できます。

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

次のコードは、「income」列を表示するサービスの実装です。

行 12 は、列を Sightly テンプレートに関連付けます

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

## サーバーでテストします

>[!NOTE]
>
>この記事は、このシリーズの[以前の記事](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html?lang=ja)の[サンプルワークフロー](assets/review-workflow.zip)と[サンプルフォーム](assets/snap-form.zip)がインストールされていることを前提としています。

* [管理者ユーザーとして crx にログインします](http://localhost:4502/crx/de/index.jsp)
* [Sightly テンプレートを読み込みます](assets/sightly-template.zip)
* [AEM Web コンソールにログインします](http://localhost:4502/system/console/bundles)
* [インボックスカスタマイズバンドルをデプロイして開始します](assets/income-column-customization.jar)
* [インボックスを開きます](http://localhost:4502/aem/inbox)
* 「作成」ボタンの横のリスト表示をクリックして、管理コントロールを開きます。
* インボックスに「income」列を追加し、変更を保存します
* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _配偶者の有無_&#x200B;を選択し、フォームを送信します
* [インボックスを表示します](http://localhost:4502/aem/inbox)

フォームを送信するとワークフローがトリガーされ、タスクが「admin」ユーザーに割り当てられます。「収入」列の下に適切なアイコンが表示されます。
