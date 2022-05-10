---
title: AEM Forms Portal コンポーネントの有効化
description: コアコンポーネントを使用したAEM Forms Portal の構築
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 17%

---

# Forms Portal コンポーネント

AEM Forms は、次のポータルコンポーネントを標準搭載しています。

**Search &amp; Lister**:このコンポーネントを使用すると、フォームリポジトリのフォームをポータルページに一覧表示でき、指定した条件に基づいてフォームを一覧表示する設定オプションを提供します。

**ドラフトと送信**:Search &amp; Lister コンポーネントには、Formsの作成者が公開したフォームが表示され、Drafts &amp; Submissions コンポーネントには、後で完了するためにドラフトとして保存されたフォームと送信済みのフォームが表示されます。 このコンポーネントはログインユーザーに対してパーソナライズされたエクスペリエンスを提供します。

**リンク**:このコンポーネントを使用すると、ページの任意の場所にフォームへのリンクを作成できます。

## フォームポータルコンポーネントを有効にする

IntelliJ を起動し、 [以前の手順です。](./getting-started.md) ui.apps->src->main->content->jcr_root->apps.bankingapplication->components を展開します。

Adobe Experience Manager（AEM）サイトで任意のコアコンポーネント（標準のポータルコンポーネントを含む）を使用するには、プロキシコンポーネントを作成して、サイトに対してそれを有効にする必要があります。新しく作成したプロキシコンポーネントは、すべてをそのコンポーネントから継承するように、標準のフォームコンポーネントを指す必要があります。 これは、プロキシコンポーネントの content.xml 内の resourceSuperType を変更することで行われます。 content.xml では、タイトルとコンポーネントグループも指定します。
>[!NOTE]
>
> 各 [ここからこれらのコンポーネント](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### ドラフトと送信

既存のコンポーネントのコピーを作成する ( 例： `button`) を _draftsandssubmits_.
![draftsandssubmits](assets/forms-portal-components2.png)
次の `.content.xml` を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Search &amp; Lister

ボタンコンポーネントのコピーを作成し、名前をに変更します。 _searchandlister_.
次の `.content.xml` を次の XML に置き換えます。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### リンクコンポーネント

ボタンコンポーネントのコピーを作成し、名前をに変更します。 _リンク_.
次の `.content.xml` を次の XML に置き換えます。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

プロジェクトがデプロイされると、AEMページでこれらのコンポーネントを使用してFormsポータルを作成できるようになります。