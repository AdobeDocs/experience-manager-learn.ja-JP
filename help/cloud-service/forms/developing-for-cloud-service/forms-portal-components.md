---
title: AEM Forms ポータルコンポーネントの有効化
description: コアコンポーネントを使用して AEM Forms ポータルを作成します。
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 92
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '332'
ht-degree: 100%

---

# フォームポータルコンポーネント

AEM Forms は、次のポータルコンポーネントを標準搭載しています。

**検索とリスター**：このコンポーネントを使用すると、フォームリポジトリ内のフォームをポータルページに一覧表示できます。また、指定した基準に基づいてフォームを一覧表示するための設定オプションが提供されます。

**ドラフトと送信**：「検索とリスター」コンポーネントがフォーム作成者が公開したフォームを表示するのに対し、「ドラフトと送信」コンポーネントは、後で完成させるためにドラフトとして保存されたフォームと送信されたフォームを表示します。このコンポーネントは、ログインしたユーザーに対してパーソナライズされたエクスペリエンスを提供します。

**リンク**：このコンポーネントを使用すると、ページの任意の場所にフォームへのリンクを作成できます。

## フォームポータルコンポーネントの有効化

IntelliJ を起動し、[以前の手順](./getting-started.md)で作成した BankingApplication プロジェクトを開きます。ui.appssrc／main／content／jcr_root／apps.bankingapplication／components を展開します。

Adobe Experience Manager（AEM）サイトで任意のコアコンポーネント（標準のポータルコンポーネントを含む）を使用するには、プロキシコンポーネントを作成して、サイトに対してそれを有効にする必要があります。新しく作成したプロキシコンポーネントは、すべてをそのコンポーネントから継承するように、標準のフォームコンポーネントを指す必要があります。 これは、プロキシコンポーネントの content.xml 内の resourceSuperType を変更することで行われます。 content.xml では、タイトルとコンポーネントグループも指定します。
>[!NOTE]
>
> [ここで公開されている各コンポーネント](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)のリソーススーパータイプを作成できます。


### ドラフトと送信

既存のコンポーネント（例：`button`）のコピーを作成して、名前を _draftsandssubmits_ とします。
![draftsandssubmits](assets/forms-portal-components2.png)
`.content.xml` の内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 検索とリスター

ボタンコンポーネントのコピーを作成し、名前を _searchandlister_ に変更します。
`.content.xml` の内容を次の XML に置き換えます。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### リンクコンポーネント

ボタンコンポーネントのコピーを作成し、名前を _link_ に変更します。 
`.content.xml` の内容を次の XML に置き換えます。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

プロジェクトがデプロイされると、AEM ページでこれらのコンポーネントを使用してフォームポータルを作成できるようになります。

## 次の手順

[クラウドサービス設定を含める](./azure-storage-fdm.md)
