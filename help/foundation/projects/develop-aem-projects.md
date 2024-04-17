---
title: AEM でのプロジェクトの開発
description: AEM プロジェクトの開発方法を説明する開発チュートリアルです。 このチュートリアルでは、AEM 内で新規プロジェクトを作成して、コンテンツオーサリングのワークフローとタスクの管理に使用できるカスタムプロジェクトテンプレートを作成します。
version: 6.4, 6.5
feature: Projects, Workflow
doc-type: Tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
duration: 1603
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: ht
source-wordcount: '4441'
ht-degree: 100%

---

# AEM でのプロジェクトの開発

これは、[!DNL AEM Projects] の開発方法を説明する開発チュートリアルです。 このチュートリアルでは、AEM 内でプロジェクトを作成して、コンテンツオーサリングのワークフローとタスクの管理に使用できるカスタムプロジェクトテンプレートを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*このビデオでは、以下のチュートリアルで作成する完成済みワークフローの簡単なデモを紹介します。*

## はじめに {#introduction}

[[!DNL AEM Projects]](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/projects/projects) は AEM の機能で、AEM Sites または Assets の実装の一環として、コンテンツ作成に関連するすべてのワークフローとタスクを簡単に管理およびグループ化できるように設計されています。

AEM プロジェクトには、複数の [OOTB プロジェクトテンプレート](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/projects/projects)が付属しています。 プロジェクトを作成する際に、作成者はこれらの使用可能なテンプレートから選択できます。 独自のビジネス要件を持つ大規模な AEM 実装では、ニーズに合わせてカスタマイズされたカスタムプロジェクトテンプレートを作成する必要があります。 カスタムプロジェクトテンプレートを作成することで、開発者はプロジェクトダッシュボードを設定し、カスタムワークフローに関連付けて、ビジネス上の役割をプロジェクト用に追加作成できます。 ここでは、プロジェクトテンプレートの構造について学び、サンプルテンプレートを作成します。

![カスタムプロジェクトカード](./assets/develop-aem-projects/custom-project-card.png)

## セットアップ

このチュートリアルでは、カスタムプロジェクトテンプレートの作成に必要なコードを順を追って説明します。 [添付パッケージ](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)をダウンロードしてローカル環境にインストールし、チュートリアルに従ってください。 また、[GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide) でホストされている完全な Maven プロジェクトにアクセスすることもできます。

* [完成済みのチュートリアルパッケージ](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub のフルコードリポジトリ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

このチュートリアルでは、読者が [AEM の開発手法](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/developing/introduction/the-basics)に関する基礎知識を多少なりとも持ち、[AEM Maven プロジェクトのセットアップ](https://docs.adobe.com/content/help/ja/experience-manager-65/developing/devtools/ht-projects-maven.html)についてある程度理解していることを前提としています。 取り上げられているすべてのコードは、参考として使用することを目的としており、[ローカル開発 AEM インスタンス](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/deploying/deploying/deploy)にのみデプロイするようにしてください。

## プロジェクトテンプレートの構造

プロジェクトテンプレートはソース管理下に置き、/apps 下のアプリケーションフォルダーの直下に配置してください。 **&#42;/projects/templates/**&lt;my-template> という命名規則に沿ったサブフォルダーに置くのが理想です。 この命名規則に従って配置することで、プロジェクトの作成時に作成者が新しいカスタムテンプレートを自動的に使用できるようになります。 使用可能なプロジェクトテンプレートの設定は、**cq:allowedTemplates** プロパティによって **/content/projects/jcr:content** ノードに設定されます。 デフォルトでは、次のような正規表現になります。**/(apps|libs)/.&#42;/projects/templates/.&#42;**

プロジェクトテンプレートのルートノードには、**cq:Template** の **jcr:primaryType** があります。 そのルートノードの直下に、**gadgets**、**roles**、**workflows** の 3 つのノードがあります。 これらのノードはすべて **nt:unstructured** です。 また、ルートノードの直下に、「プロジェクトを作成」ウィザードでテンプレートを選択するときに表示される thumbnail.png ファイルを配置することもできます。

完全なノード構造は次のとおりです。

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### プロジェクトテンプレートルート

プロジェクトテンプレートのルートノードは **cq:Template** タイプです。 このノードでは、プロジェクトを作成ウィザードに表示されるプロパティ **jcr:title** と **jcr:description** を設定することができます。 また、**wizard**&#x200B;というプロパティもあり、これはプロジェクトのプロパティに値を入力するフォームを指しています。 **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** のデフォルト値は、ほとんどの場合にうまく機能します。ユーザーが基本的なプロジェクトプロパティに値を入力したり、グループメンバーを追加したりできるからです。

*&#42;なお、プロジェクトを作成ウィザードでは Sling POST サーブレットを使用しません。 代わりに、値はカスタムサーブレット&#x200B;**com.adobe.cq.projects.impl.servlet.ProjectServlet**にポストされます。 これについては、カスタムフィールドを追加する際に考慮してください。*

翻訳プロジェクトテンプレートのカスタムウィザードの例は **/libs/cq/core/content/projects/wizard/translationproject/defaultproject** にあります。

### gadgets {#gadgets}

このノードには追加のプロパティはありませんが、gadgets ノードの子は、新しいプロジェクトの作成時にプロジェクトのダッシュボードに表示されるプロジェクトタイルを制御します。 [プロジェクトタイル](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/projects/projects) （ガジェットやポッドとも呼ばれます）は、プロジェクトのワークプレースに表示されるシンプルなカードです。 OOTB タイルの完全なリストは、**/libs/cq/gui/components/projects/admin/pod にあります。 **プロジェクト所有者は、プロジェクトの作成後、いつでもタイルの追加や削除を行うことができます。

### roles {#roles}

すべてのプロジェクトには、[デフォルトの役割](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/projects/projects)として、**監視者**、**編集者**、**所有者**&#x200B;の 3 つがあります。 roles ノードの直下に子ノードを追加すると、ビジネス固有のプロジェクトの役割をテンプレートに追加できます。 そして、これらの役割を、プロジェクトに関連付けされている特定のワークフローに結び付けることができます。

### workflows {#workflows}

カスタムプロジェクトテンプレートを作成する最も魅力的な理由の 1 つは、プロジェクトで使用可能なワークフローを設定できる点です。 これらは OOTB ワークフローでもカスタムワークフローでも構いません。 **workflows** ノードの直下には **models** ノード（これも `nt:unstructured`）が必要で、その直下の子ノードは使用可能なワークフローモデルを指定します。 プロパティ**modelId **は、/etc/workflow 下のワークフローモデルとプロパティを指し、**wizard** は、ワークフローの開始時に使用されるダイアログを指します。 プロジェクトの大きな利点は、ワークフローの開始時にビジネス固有のメタデータを取得するカスタムダイアログ（ウィザード）を追加できることで、ワークフロー内でさらに多くのアクションを実行できます。

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## プロジェクトテンプレートの作成 {#creating-project-template}

主にノードをコピー／設定しているので、CRXDE Lite を使用します。 ローカルの AEM インスタンスで、[CRXDE Lite](http://localhost:4502/crx/de/index.jsp) を開きます。

1. まず、`/apps/&lt;your-app-folder&gt;` の直下に新しいフォルダーを作成して、名前を `projects` とします。 その直下に別のフォルダーを作成して、名前を `templates` とします。

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 作業を容易にするために、既存のシンプルなプロジェクトテンプレートからカスタムテンプレートの作成を開始します。

   1. ノード **/libs/cq/core/content/projects/templates/default** をコピーして、手順 1 で作成した *templates* フォルダーの下にペーストします。

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. これで、パスは **/apps/aem-guides/projects-tasks/projects/templates/authoring-project** のようになります。

   1. author-project ノードの **jcr:title** および **jcr:description** プロパティを編集して、カスタムタイトルと説明の値に変更します。

      1. **wizard** プロパティは、デフォルトのプロジェクトプロパティを指したままにしておきます。

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. このプロジェクトテンプレートでは、タスクを利用します。
   1. 新しい **nt:unstructured** ノードを authoring-project/gadgets の直下に追加して、名前を **tasks** にします。
   1. String プロパティを tasks ノードに追加して、**cardWeight**=&quot;100&quot;、**jcr:title**=&quot;Tasks&quot;、**sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot; とします。

   これで、新しいプロジェクトが作成される際に [Tasks タイル](https://experienceleague.adobe.com/docs/?lang=ja#Tasks)がデフォルトで表示されます。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. プロジェクトテンプレートにカスタムの「承認者」役割を追加します。

   1. プロジェクトテンプレート（authoring-project）ノードの直下に新しい **nt:unstructured** ノードを追加して、**roles** というラベルを付けます。
   1. 別の **nt:unstructured** ノードを、approvers というラベルを付けて、roles ノードの子として追加します。
   1. String プロパティ **jcr:title**=&quot;**Approvers**&quot;、**roleclass**=&quot;**owner**&quot;、**roleid**=&quot;**approvers**&quot; を追加します。
      1. approvers ノードの名前、および jcr:title と roleid には、（roleid が一意である限り）任意の文字列値を指定できます。
      1. **roleclass** は、**owner**、**editor**、**observer** の [3 つの OOTB の役割](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/projects/projects)に基づいて、その役割に適用される権限を管理します。
      1. 一般に、カスタムの役割が管理の役割の色彩が強い場合、roleclass は **owner** にできます。フォトグラファーやデザイナーなど、オーサリング上のより具体的な役割の場合は、**editor** の roleclass で十分です。 **owner** と **editor** の大きな違いは、プロジェクトの所有者はプロジェクトのプロパティを更新したり、新しいユーザーをプロジェクトに追加したりできる点です。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 「シンプルなプロジェクト」テンプレートをコピーすると、4 つの OOTB ワークフローが設定されます。 workflows/models の直下の各ノードは、特定のワークフローと、そのワークフローの開始ダイアログウィザードを指しています。 このチュートリアルの後半では、このプロジェクトのカスタムワークフローを作成します。 ここでは、workflow/models の下のノードを削除します。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. コンテンツ作成者がプロジェクトテンプレートを容易に識別できるように、カスタムサムネールを追加できます。 推奨サイズは 319 x 319 ピクセルです。
   1. CRXDE Lite で、ガジェット、役割、ワークフローのノードの兄弟として、**thumbnail.png** という名前の新しいファイルを作成します。
   1. 保存して、`jcr:content` ノードに移動し、`jcr:data` プロパティをダブルクリックします（「表示」をクリックしないでください）。
      1. これにより、`jcr:data` ファイルを編集するダイアログが表示され、カスタムサムネールをアップロードできます。

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

プロジェクトテンプレートの XML 表示域が完了しました。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## カスタムプロジェクトテンプレートのテスト

プロジェクトを作成して、プロジェクトテンプレートをテストできるようになりました。

1. カスタムテンプレートが、プロジェクト作成用のオプションの 1 つとして表示されます。

   ![テンプレートを選択](./assets/develop-aem-projects/choose-template.png)

1. カスタムテンプレートを選択したら、「次へ」をクリックし、プロジェクトメンバーに入力する際に、承認者の役割として追加できることに注意してください。

   ![承認](./assets/develop-aem-projects/user-approver.png)

1. 「作成」をクリックして、カスタムテンプレートに基づくプロジェクトの作成を終了します。 プロジェクトダッシュボードに、タスクタイルと、ガジェットの下に設定された他のタイルが自動的に表示されます。

   ![タスクタイル](./assets/develop-aem-projects/tasks-tile.png)


## ワークフローを使用する理由

従来、承認プロセスを中心とする AEM ワークフローは、参加者ワークフローステップを使用していました。 AEM インボックスには、タスクとワークフローに関する詳細が含まれており、AEM プロジェクトとの統合が強化されています。 これらの機能により、プロジェクト作成タスクプロセス手順の使用がより魅力的なオプションになります。

### タスクを実行する理由

従来の参加者ステップに対してタスク作成ステップを使用すると、次のような利点があります。

* **開始日と期限** - 作成者が自分の時間を簡単に管理できるようにします。新しいカレンダー機能はこれらの日付を利用します。
* **優先度** - 低、標準、高の優先度が優先度に組み込まれているので、作成者は作業を優先順位付けすることができます
* **スレッドコメント** - 作成者がタスクに取り組むとき、コメントを残すことができるため、共同作業が促進されます
* **表示** - プロジェクトのタスクタイルと表示により、管理者はどのように時間が費やされているかを確認できます
* **プロジェクトの統合** - タスクは既にプロジェクトの役割およびダッシュボードと統合されています

参加者ステップと同様に、タスクを動的に割り当てたり、ルーティングしたりすることができます。 タイトル、優先度などのタスクメタデータは、次のチュートリアルで説明するように、以前のアクションに基づいて動的に設定することもできます。

タスクには参加者ステップよりもいくつかの利点がありますが、追加のオーバーヘッドが発生し、プロジェクトの外ではそれほど役に立ちません。 これに加えて、タスクのすべての動的な動作は、独自の制限を持つ ecma スクリプトを使用してコーディングする必要があります。

## サンプルユースケースの要件 {#goals-tutorial}

![ワークフロープロセスダイアグラム](./assets/develop-aem-projects/workflow-process-diagram.png)

上の図は、サンプルの承認ワークフローの全体的な要件の概要を示しています。

最初のステップは、コンテンツの一部の編集を終了するタスクを作成することです。 ワークフローイニシエーターがこの最初のタスクの担当者を選択できるようにします。

最初のタスクが完了すると、担当者はワークフローをルーティングする次の 3 つのオプションを使用できます。

**通常**- 通常のルーティングでは、プロジェクトの承認者グループに割り当てられたタスク（レビューと承認の実行用）を作成します。 タスクの優先順位は標準で、期限は作成日から 5 日間です。

**緊急** - 緊急ルーティングは、プロジェクトの承認者グループに割り当てられたタスクも作成します。 タスクの優先順位が高く、期限は 1 日だけです。

**スキップ** - このサンプルワークフローでは、最初の参加者には承認グループをスキップするオプションがあります。 （これが「承認」ワークフローの目的を覆す可能性がありますが、追加のルーティング機能を説明することができます）

承認者グループは、コンテンツを承認するか、再作業用に最初の担当者に返送することができます。 再作業用に返送された場合、新しいタスクが作成され、適切に「再作業用に返送」というラベルが付けられます。

ワークフローの最後のステップは、ページやアセットの ootb アクティベートプロセスステップを利用し、ペイロードを複製します。

## ワークフローモデルの作成

1. AEM スタートメニューでツール／ワークフロー／モデルに移動します。 右上隅の「作成」をクリックして、ワークフローモデルを作成します。

   新しいモデルに「コンテンツ承認ワークフロー」というタイトルを付け、URL 名に「content-approval-workflow」と入力します。

   ![ワークフロー作成ダイアログ](./assets/develop-aem-projects/workflow-create-dialog.png)

   [ワークフローの作成に関する詳細については、こちらをお読みください](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-models)。

1. ベストプラクティスとして、カスタムワークフローは、/etc/workflow/models の下の専用フォルダーにグループ化する必要があります。 CRXDE Lite で、/etc/workflow/models の下に、**「aem-guides」**&#x200B;という名前で、**「nt:folder」**&#x200B;を作成します。 サブフォルダーを追加すると、アップグレードまたはサービスパックのインストール中に、カスタムワークフローが誤って上書きされることがなくなります。

   &#42;サブフォルダー全体がアップグレードやサービスパックによって上書きされる可能性もあるため、/etc/workflow/models/dam や /etc/workflow/models/projects などの ootb サブフォルダーの下にフォルダーやカスタムワークフローを配置しないことが重要です。

   ![6.3 でのワークフローモデルの場所](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   6.3 でのワークフローモデルの場所

   >[!NOTE]
   >
   >AEM 6.4 以降を使用している場合、ワークフローの場所が変更されています。 詳しくは、[こちらを参照してください。](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   AEM 6.4 以降を使用している場合、ワークフローモデルは `/conf/global/settings/workflow/models` の下に作成されます。 /conf ディレクトリで上記の手順を繰り返し、`aem-guides` という名前のサブフォルダーを追加し、その下に `content-approval-workflow` を移動します。

   ![最新のワークフロー定義の場所](./assets/develop-aem-projects/modern-workflow-definition-location.png)
6.4 以降のワークフローモデルの場所

1. AEM 6.3 で導入されたのは、特定のワークフローにワークフローステージを追加する機能です。 ステージは、「ワークフロー情報」タブのインボックスからユーザーに表示されます。 ワークフローの現在のステージと、その前後のステージがユーザーに表示されます。

   ステージを設定するには、サイドキックからページのプロパティダイアログを開きます。 4 番目のタブには、「ステージ」というラベルが付いています。 このワークフローの 3 つのステージを設定するには、以下の値を追加します。

   1. コンテンツの編集
   1. 承認
   1. パブリッシュ

   ![ワークフローステージの設定](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   ページのプロパティダイアログで、ワークフローステージを設定します。

   ![ワークフローの進行状況バー](./assets/develop-aem-projects/workflow-info-progress.png)

   AEM インボックスから見たワークフローの進行状況バー。

   オプションで、**画像**&#x200B;をページのプロパティにアップロードできます。これは、ユーザーが選択した際にワークフローサムネールとして使用されます。 画像のサイズは、319 x 319 ピクセルにする必要があります。 ページのプロパティに&#x200B;**説明**&#x200B;を追加すると、ユーザーがワークフローを選択する際にも表示されます。

1. プロジェクトタスクを作成ワークフロープロセスは、ワークフローのステップとしてタスクを作成するように設計されています。 タスクを完了した後にのみ、ワークフローが前に進みます。 プロジェクトタスクを作成ステップの強力な側面は、ワークフローのメタデータ値を読み取り、それらを使用して動的にタスクを作成できる点です。

   最初に、デフォルトで作成される参加者ステップを削除します。 コンポーネントメニューのサイドキックから&#x200B;**「プロジェクト」**&#x200B;サブ見出しを展開し、**「プロジェクトタスクを作成」**&#x200B;をモデルにドラッグ＆ドロップします。

   「プロジェクトタスクを作成」ステップをダブルクリックして、ワークフローダイアログを開きます。 以下のプロパティを設定します。

   このタブは、すべてのワークフロープロセスステップで共通で、「タイトル」と「説明」を設定します（これらはエンドユーザーには表示されません）。 設定する重要なプロパティは、ワークフローステージをドロップダウンメニューから&#x200B;**「コンテンツを編集」**&#x200B;に設定することです。

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   プロジェクトタスクを作成ワークフロープロセスは、ワークフローのステップとしてタスクを作成するように設計されています。 「タスク」タブでは、タスクのすべての値を設定できます。 担当者を動的にする場合は、空白のままにします。 残りのプロパティ値は次のとおりです。

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   「ルーティング」タブは、ユーザーがタスクを完了するために使用できるアクションを指定できるオプションのダイアログです。 これらのアクションは単なる文字列値であり、ワークフローのメタデータに保存されます。 これらの値は、ワークフローの後半でスクリプトやプロセスステップで読み取り、ワークフローを動的に「ルーティング」することができます。 ワークフローの目標に基づいて、このタブに 3 つのアクションを追加します。

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   このタブでは、タスクの作成前にタスクの様々な値をプログラムで決定できるタスクを事前作成スクリプトを設定できます。 スクリプトを外部ファイルに指定するか、短いスクリプトをダイアログに直接埋め込むオプションがあります。 この例では、タスクを事前作成スクリプトを外部ファイルに指定します。 手順 5 で、そのスクリプトを作成します。

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 前の手順では、タスクを事前作成スクリプトを参照しました。 ここで、ワークフローメタデータ値「**assignee**」の値に基づいてタスクの担当者を設定するスクリプトを作成します。 **「assignee」**&#x200B;の値は、ワークフローがキックオフされたときに設定します。 また、ワークフローのメタデータの&#x200B;**「taskPriority」** 値と **&quot;taskDueDate&quot; ** を読み取って、最初のタスクの有効期限が切れたときに動的に設定することで、タスクの優先度を動的に選択するワークフローメタデータを読み取ります。

   組織のために、アプリフォルダーの下にフォルダーを作成して、すべてのプロジェクト関連スクリプト **/apps/aem-guides/projects-tasks/projects/scripts** を保持します。 このフォルダーの下に、**「start-task-config.ecma」**&#x200B;という名前の新しいファイルを作成します。 &#42;start-task-config.ecma ファイルへのパスが、手順 4 の「詳細設定」タブで設定したパスと一致するようにします。

   ファイルのコンテンツとして以下を追加します。

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. コンテンツ承認ワークフローに戻ります。 **OR 分岐**&#x200B;コンポーネント （「ワークフロー」カテゴリの下のサイドキックにあります）を&#x200B;**タスクを開始**&#x200B;ステップの下にドラッグ＆ドロップします。 共通ダイアログで、3 つのブランチのラジオボタンを選択します。 OR 分岐は、ワークフローメタデータ値&#x200B;**「lastTaskAction」**&#x200B;を読み取り、ワークフローのルートを決定します。 **「lastTaskAction」** プロパティは、手順 4 で設定した「ルーティング」タブの値の 1 つに設定されます。 「分岐」タブのそれぞれで、「**スクリプト**」テキスト領域に次の値を記入します。

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   &#42;ルートを決定するために文字列の直接照合を行っているので、分岐のスクリプトに設定された値が、手順 4 で設定されたルートの値と一致することが重要であることに注意してください。

1. 別の「**プロジェクトタスクを作成**」ステップをドラッグして、OR 分岐の下の左端のモデル（分岐 1）にドロップします。 ダイアログに次のプロパティを入力します。

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   これは「通常の承認」ルートなので、タスクの優先度は「中」に設定しています。 さらに、承認者グループにタスクを完了するための期間を 5 日間とします。 「詳細設定」タブで動的に割り当てるので、「タスク」タブの担当者は空白のままにします。 このタスクを完了する際に、承認者グループに 2 つのルートを提供します。「**承認して公開**」は、コンテンツを承認し、公開できる場合で、「**修正用に送り返す**」は、元の編集者による修正が必要な問題がある場合です。 承認者は、元の編集者に対して、ワークフローが元の編集者に返されたかどうかを確認するコメントを残すことができます。

このチュートリアルでは先に、承認者の役割を含むプロジェクトテンプレートを作成しました。 このテンプレートから新しいプロジェクトが作成されるたびに、承認者の役割用にプロジェクト固有のグループが作成されます。 「参加者」ステップと同様に、タスクはユーザーまたはグループにのみ割り当てることができます。 このタスクを、承認者グループに対応するプロジェクトグループに割り当てます。 プロジェクト内から起動されるすべてのワークフローには、プロジェクトの役割をプロジェクト固有のグループにマッピングするメタデータが含まれます。

次のコードをコピーして、「**詳細設定**」タブの「**スクリプト**」テキスト領域にペーストします。 このコードは、ワークフローのメタデータを読み取り、タスクをプロジェクトの承認者グループに割り当てます。 承認者グループの値が見つからない場合は、タスクの管理者グループへの割り当てに戻ります。

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 別の「**プロジェクトタスクを作成**」ステップをドラッグして、OR 分岐の下の中央のモデル（分岐 2）にドロップします。 ダイアログに次のプロパティを入力します。

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   これは「ラッシュ承認」ルートなので、タスクの優先度は「高」に設定しています。 さらに、承認者グループにタスクを完了するための期間として 1 日のみを与えます。 「詳細設定」タブで動的に割り当てるので、「タスク」タブの担当者は空白のままにします。

   手順 7 と同じスクリプトスニペットを再利用し、「**詳細設定**」タブの「**スクリプト**」テキスト領域 に入力します。 次のコードをコピーしてペーストします。

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 「**操作なし**」コンポーネントをドラッグして、右端の分岐（分岐 3）にドロップします。 「操作なし」コンポーネントは何のアクションも実行せず、直ちに次に進みます。承認ステップを省略したいという元の編集者の要求を表しています。 技術的には、この分岐をワークフローステップなしにしておくことが可能ですが、ベストプラクティスとして、「操作なし」のステップを追加します。 これにより、他の開発者に対して、分岐 3 の目的が明確になります。

   ワークフローステップをダブルクリックし、「タイトル」と「説明」を設定します。

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![ワークフローモデルの OR 分岐](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   OR 分岐の 3 つの分岐すべてが設定されたら、ワークフローモデルは次のようになります。

1. 承認者グループには、ワークフローを元の編集者に送り返して追加の修正を求めるオプションがあるので、最後に実行されたアクションを読み取り、ワークフローを最初にルーティングするのか、それとも続行させるのかは、「**移動**」ステップに依存します。

   「移動ステップ」コンポーネント（「ワークフロー」の下の「サイドキック」内）をドラッグし、OR 分岐の下の再結合する場所にドロップします。 ダブルクリックして、ダイアログで次のプロパティを設定します。

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   最後に設定するのは、「移動」プロセスステップの一部となるスクリプトです。 スクリプトの値は、ダイアログを使用して埋め込むことも、外部ファイルを指すように設定することもできます。 移動スクリプトには&#x200B;**関数 check()** を含め、ワークフローが指定された手順に進む必要がある場合に true を返すようにする必要があります。 false が返されると、ワークフローが次に進みます。

   承認者グループが「**修正用に送り返す**」アクション（手順 7 と 8 で設定）を選択した場合、ワークフローを「**タスク作成を開始** 」ステップに戻します。

   「プロセス」タブで、次のスニペットを「スクリプト」テキスト領域に追加します。

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. ペイロードを公開するには、OOTB の「**ページ/アセットをアクティベート**」プロセスステップを使用します。 このプロセスステップではほとんど設定が必要なく、アクティベーション用にワークフローのペイロードをレプリケーションキューに追加します。 「移動」ステップの下にステップを追加します。この方法では、承認者グループが公開用にコンテンツを承認した場合、または元の編集者が承認の省略ルートを選択した場合にのみ、このステップに到達できます。

   「**ページ/アセットをアクティベート**」プロセスステップ（「WCM ワークフロー」の下の「サイドキック」内）をドラッグし、モデル内の「移動」ステップの下にドロップします。

   ![ワークフローモデルの完成](assets/develop-aem-projects/workflow-model-final.png)

   「移動」ステップおよび「ページ/アセットをアクティベート」ステップを追加した後のワークフローモデルの表示内容。

1. 承認者グループが修正用にコンテンツを送り返した場合、元の編集者に通知します。 これを実現するには、タスク作成プロパティを動的に変更します。 「**修正用に送り返す**」の lastActionTaken プロパティ値のキーを設定します。 その値が存在する場合は、タイトルと説明を変更して、このタスクはコンテンツが修正用に送り返された結果であることを示します。 また、優先度を「**高**」に更新して、編集者が最初に作業する項目になるようにします。 最後に、タスクの期限を、ワークフローが修正用に送り返された日から 1 日に設定します。

   開始 `start-task-config.ecma` スクリプト（手順 5 で作成）を次のように置き換えます。

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## 「ワークフローを開始」ウィザードを作成する {#start-workflow-wizard}

プロジェクト内からワークフローを開始する場合は、ワークフローを開始するウィザードを指定する必要があります。 デフォルトのウィザードである `/libs/cq/core/content/projects/workflowwizards/default_workflow` では、ワークフローのタイトル、開始コメント、実行するワークフローのペイロードパスをユーザーが入力できます。 `/libs/cq/core/content/projects/workflowwizards` には、他にもいくつかの例があります。

カスタムウィザードを作成すると、ワークフローが開始する前に重要な情報を収集できるので、非常に強力です。 データはワークフローのメタデータの一部として保存されます。ワークフロープロセスでは、これを読み取り、入力された値に基づいて動作を動的に変更できます。 カスタムウィザードを作成し、開始ウィザードの値に基づいて、ワークフローの最初のタスクを動的に割り当てます。

1. CRXDE-Lite では、`/apps/aem-guides/projects-tasks/projects` フォルダーの下に「wizards」というサブフォルダーを作成します。 新しく作成されたウィザードフォルダーの下の `/libs/cq/core/content/projects/workflowwizards/default_workflow` からデフォルトのウィザードをコピーし、名前を「**content-approval-start**」に変更します。 フルパスは、`/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start` になります。

   デフォルトのウィザードは 2 列のウィザードで、最初の列には選択したワークフローモデルのタイトル、説明およびサムネールが表示されます。 2 番目の列には、ワークフロータイトル、コメントを開始およびペイロードパスのフィールドが含まれます。 ウィザードは、標準のタッチ UI フォームであり、標準の [Granite UI フォームコンポーネント](https://experienceleague.adobe.com/docs/?lang=ja)を使用してフィールドに値を入力します。

   ![コンテンツ承認ワークフローウィザード](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. ワークフローの最初のタスクの担当者を設定するために使用するウィザードにフィールドを追加します（[ワークフローモデルの作成](#create-workflow-model)：ステップ 5 を参照）。

   `../content-approval-start/jcr:content/items/column2/items` の下に、**「assign」**&#x200B;という名前の `nt:unstructured` タイプの新しいノードを作成します。 プロジェクトのユーザーピッカーコンポーネント（[Granite ユーザーピッカーコンポーネントに基づいています](https://experienceleague.adobe.com/docs/?lang=ja)）を使用します。 このフォームフィールドを使用すると、ユーザーとグループの選択を現在のプロジェクトに属するものだけに簡単に制限できます。

   **assign** ノードの XML 表現を以下に示します。

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. ワークフローの最初のタスクの優先度を決定する優先度選択フィールドも追加します（[ワークフローモデルの作成](#create-workflow-model)：ステップ 5 を参照）。

   `/content-approval-start/jcr:content/items/column2/items` の下に、**priority** という名前の `nt:unstructured` タイプの新しいノードを作成します。 [Granite UI 選択コンポーネント](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)を使用して、フォームフィールドに値を入力します。

   **priority** ノードの下に、**nt:unstructured** の **items** ノードを追加します。 **items** ノードの下にさらに 3 つのノードを追加して、高、中、低の選択オプションを入力します。 各ノードのタイプは **nt:unstructured** で、**text** と **value** のプロパティが必要です。 text と value の両方が同じ値である必要があります。

   1. 高
   1. 中
   1. 低

   「中」ノードの場合、**「選択済み」**&#x200B;という名前のブーリアンプロパティを追加し、値を **true** に設定します。 これにより、「中」が選択フィールドのデフォルト値になります。

   ノード構造とプロパティの XML 表現を以下に示します。

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. ワークフローの開始者が最初のタスクの期日を設定できるようにします。 [Granite UI DatePicker](https://experienceleague.adobe.com/docs/?lang=ja) フォームフィールドを使用して、この入力をキャプチャします。 また、[TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) を含む非表示フィールドを追加して、入力が Date タイプのプロパティとして JCR に格納されるようにします。

   以下の XML で表される次のプロパティを持つ 2 つの **nt:unstructured** ノードを追加します。

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. ウィザードを開始ダイアログの完全なコードは、[こちら](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml)で確認できます。

## ワークフローとプロジェクトテンプレートの接続 {#connecting-workflow-project}

最後に、ワークフローモデルをプロジェクトの 1 つから開始できるようにする必要があります。 これを行うには、このシリーズのパート 1 で作成したプロジェクトテンプレートに再度アクセスする必要があります。

ワークフロー設定は、そのプロジェクトで使用できるワークフローを指定するプロジェクトテンプレートの領域です。 また、この設定では、ワークフロー（[前の手順](#start-workflow-wizard)で作成したもの）を開始する際にワークフローを開始ウィザードも指定します。 プロジェクトテンプレートのワークフロー設定が「ライブ」になるという意味は、ワークフロー設定を更新すると、テンプレートを使用する既存のプロジェクトだけでなく、作成した新しいプロジェクトにも影響するということです。

1. CRXDE-Lite で、以前に `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models` で作成したプロジェクトの作成テンプレートに移動します。

   models ノードの下に、ノード タイプが **nt:unstructured** の **contentapproval** という名前の新しいノードを追加します。 ノードに次のプロパティを追加します。

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >AEM 6.4 を使用している場合、ワークフローの場所が変更されました。 `/var/workflow/models/aem-guides/content-approval-workflow` の下にあるランタイムワークフローモデルの場所を `modelId` プロパティに指定します。
   >
   >
   >[ワークフローの場所の変更について詳しくは、こちら](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)を参照してください。

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. コンテンツ承認ワークフローをプロジェクトテンプレートに追加したら、プロジェクトのワークフロータイルから開始できるようになります。 先に進んで、作成した様々なルーティングを起動して試してください。

## サポート資料

* [完成したチュートリアルパッケージのダウンロード](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub のフルコードリポジトリ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM プロジェクトのドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/projects/projects)
