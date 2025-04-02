---
title: AEM Forms ワークフローでの LDAP の使用
description: 送信者のマネージャーへの AEM Forms ワークフロータスクの割り当て
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: Experience Manager 6.4, Experience Manager 6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 111
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '508'
ht-degree: 100%

---

# AEM Forms ワークフローでの LDAP の使用

送信者のマネージャーへの AEM Forms ワークフロータスクの割り当て。

AEMワークフローでアダプティブフォームを使用する場合、フォーム送信者のマネージャーにタスクを動的に割り当てる必要があります。この使用例を実現するには、LDAP で AEM を設定する必要があります。

LDAP で AEM を設定するために必要な手順の[詳細についてはこちら](https://helpx.adobe.com/ja/experience-manager/6-5/sites/administering/using/ldap-config.html)を参照してください。

この記事のために、Adobe LDAP で AEM を設定する際に使用する設定ファイルを添付します。これらのファイルは、パッケージマネージャーを使用してインポートできるパッケージに含まれています。

以下のスクリーンショットでは、特定のコストセンターに属するすべてのユーザーを取得しています。LDAP のすべてのユーザーを取得する場合は、追加のフィルターを使用できません。

![LDAP の設定](assets/costcenterldap.gif)

以下のスクリーンショットでは、LDAP から AEM に取得したユーザーに、グループを割り当てます。読み込まれたユーザーに forms-users グループが割り当てられていることに注意してください。AEM Forms を操作するには、ユーザーがこのグループのメンバーである必要があります。また、manager プロパティは AEM の profile／manager ノードの下にも保存されます。

![Synchandler](assets/synchandler.gif)

LDAP を設定し、ユーザーを AEM に読み込んだら、送信者のマネージャーにタスクを割り当てるワークフローを作成できます。この記事では、シンプルな 1 ステップの承認ワークフローを開発しました。

ワークフローの最初のステップで、initialstep の値を「いいえ」に設定します。アダプティブフォーム内のビジネスルールでは、「送信者の詳細」パネルが無効になり、initialstep 値に基づいて「承認者」パネルが表示されます。

2 番目のステップでは、タスクを送信者のマネージャーに割り当てます。カスタムコードを使用して、送信者のマネージャーを取得します。

![タスクの割り当て](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

コードスニペットには、マネージャー ID を取得し、タスクをマネージャーに割り当てる役割があります。

ワークフローを開始した人物がわかります。次に、 manager プロパティの値を取得します。

manager プロパティが LDAP にどのように保存されるかに応じて、manager ID を取得するのに文字列操作が必要になる場合があります。

この記事を読んで、独自の [ParticipantChooser](https://helpx.adobe.com/ja/experience-manager/using/dynamic-steps.html) を実装してください。

これをシステムでテストする（アドビ従業員の場合は、このサンプルをすぐに使用できます）

* [setvalue バンドルをダウンロードしデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、マネージャーのプロパティを設定するためのカスタム OSGi バンドルです。
* [DevelopingWithServiceUserBundle のダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [パッケージマネージャーを使用して、この記事に関連付けられているアセットを AEM に読み込みます](assets/aem-forms-ldap.zip)。このパッケージの一部として、LDAP 設定ファイル、ワークフロー、アダプティブフォームが含まれます。
* 適切な LDAP 資格情報を使用して、LDAP で AEM を設定します。
* LDAP 資格情報を使用して AEM にログインします。
* [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled) を開く
* フォームに入力して送信します。
* 送信者のマネージャーは、レビュー用にフォームを取得する必要があります。

>[!NOTE]
>
>マネージャー名を抽出するこのカスタムコードは、Adobe LDAP に対してテスト済みです。別の LDAP に対してこのコードを実行する場合は、マネージャーの名前を取得するために、独自の getParticipant 実装を変更または作成する必要があります。
