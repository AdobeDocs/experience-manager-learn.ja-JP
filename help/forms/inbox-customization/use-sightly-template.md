---
title: インボックスのカスタマイズ
description: 適切なテンプレートを使用してワークフローの追加データを表示する追加カスタム列
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 8%

---

# 適切なテンプレートを使用したインボックスデータの表示

適切なテンプレートを使用して、インボックス列に表示するデータの形式を設定できます。 この例では、収入列の値に応じて、coral-uiアイコンを表示します。 次のスクリーンショットは、所得列でのアイコンの使用を示しています
![income-icons](assets/income-column.PNG)

[カスタムのサンゴuiアイコンの表示に使用する](assets/sightly-template.zip) テンプレートを、この記事の一部として示します。

## Sightlyテンプレート

次に、適切なテンプレートを示します。 テンプレート内のコードは、収入に応じてアイコンが表示されます。 このアイコンは、AEMに付属の[coral ui icon library](https://helpx.adobe.com/jp/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)の一部として使用できます。

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

## サービスの導入

次のコードは、所得列を表示するサービスの実装です。

行12は、列と見栄えの良いテンプレートを関連付けます

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

## サーバーでのテスト

>[!NOTE]
>
>この記事では、このシリーズの[前の記事](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md)の[サンプルワークフロー](assets/review-workflow.zip)と[サンプルフォーム](assets/snap-form.zip)を既にインストール済みであることを前提としています。

* [crxに管理者ユーザーとしてログイン](http://localhost:4502/crx/de/index.jsp)
* [見栄えの良いテンプレートの読み込み](assets/sightly-template.zip)
* [AEM Webコンソールにログイン](http://localhost:4502/system/console/bundles)
* [展開と開始インボックスカスタマイズバンドル](assets/income-column-customization.jar)
* [受信トレイを開く](http://localhost:4502/aem/inbox)
* 「作成」ボタンの横にあるリスト表示をクリックして、管理コントロールを開きます。
* 「受信ト追加レイ」の「所得」列に移動し、変更内容を保存します。
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 「_婚姻状況_」を選択し、フォームを送信します
* [表示受信トレイ](http://localhost:4502/aem/inbox)

フォームを送信するとワークフローがトリガーされ、タスクが「管理者」ユーザーに割り当てられます。 所得列の下に適切なアイコンが表示されます。
