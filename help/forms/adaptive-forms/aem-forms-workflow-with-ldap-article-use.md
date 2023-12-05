---
title: AEM Forms Workflow での LDAP の使用
description: 送信者のマネージャーへのAEM Formsワークフロータスクの割り当て
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 149
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 1%

---

# AEM Forms Workflow での LDAP の使用

送信者のマネージャーへのAEM Formsワークフロータスクの割り当て

AEMワークフローでアダプティブフォームを使用する場合、フォーム送信者のマネージャーにタスクを動的に割り当てる必要があります。 この使用例を達成するには、AEMと LDAP を設定する必要があります。

LDAP を使用してAEMを設定するために必要な手順については、 [詳細はこちら。](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html?lang=ja)

この記事の目的で、AdobeLDAP を使用してAEMを設定する際に使用する設定ファイルを添付します。 これらのファイルはパッケージに含まれ、パッケージマネージャーを使用してインポートできます。

以下のスクリーンショットでは、特定のコストセンターに属するすべてのユーザーを取得しています。 LDAP のすべてのユーザーを取得する場合は、追加のフィルターを使用できません。

![LDAP 設定](assets/costcenterldap.gif)

以下のスクリーンショットでは、LDAP からAEMに取得したユーザーにグループを割り当てます。 読み込まれたユーザーに forms-users グループが割り当てられていることに注意してください。 AEM Formsとやり取りするには、ユーザーがこのグループのメンバーである必要があります。 また、manager プロパティはAEMの profile/manager ノードの下にも保存されます。

![Synchandler](assets/synchandler.gif)

LDAP を設定し、ユーザーをAEMに読み込んだら、送信者のマネージャーにタスクを割り当てるワークフローを作成できます。 この記事では、シンプルな 1 ステップの承認ワークフローを開発しました。

ワークフローの最初のステップで、initialstep の値を「いいえ」に設定します。 アダプティブフォーム内のビジネスルールでは、「送信者の詳細」パネルが無効になり、最初の手順の値に基づいて「承認者」パネルが表示されます。

2 番目の手順では、タスクを送信者のマネージャーに割り当てます。 カスタムコードを使用して、送信者のマネージャーを取得します。

![タスクを割り当て](assets/assigntask.gif)

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

コードスニペットは、マネージャー ID を取得し、タスクをマネージャーに割り当てる役割を持ちます。

ワークフローを開始した人物がわかります。 次に、 manager プロパティの値を取得します。

manager プロパティが LDAP にどのように保存されるかに応じて、manager id を取得するには、文字列操作が必要になる場合があります。

この記事を読んで、独自の [ParticipantChooser を参照してください。](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja&amp;CID=RedirectAEMCommunityKautuk)

これをシステムでテストする (Adobe従業員の場合は、このサンプルをすぐに使用できます )

* [setvalue バンドルをダウンロードしデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。マネージャーのプロパティを設定するためのカスタム OSGi バンドルです。
* [DevelopingWithServiceUserBundle のダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [パッケージマネージャーを使用して、この記事に関連付けられているアセットをAEMに読み込みます](assets/aem-forms-ldap.zip)このパッケージの一部として、LDAP 設定ファイル、ワークフロー、アダプティブフォームが含まれます。
* 適切な LDAP 資格情報を使用して、LDAP でAEMを設定します。
* LDAP 資格情報を使用してAEMにログインします。
* を開きます。 [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* フォームに入力して送信します。
* 送信者の管理者は、確認用のフォームを取得する必要があります。

>[!NOTE]
>
>マネージャ名を抽出するこのカスタムコードは、AdobeLDAP に対してテストされています。 別の LDAP に対してこのコードを実行する場合は、管理者の名前を取得するために、独自の getParticipant 実装を変更または作成する必要があります。
