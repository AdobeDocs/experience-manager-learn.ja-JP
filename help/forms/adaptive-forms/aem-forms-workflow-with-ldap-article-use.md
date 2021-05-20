---
title: LDAPとAemのForms Workflow
seo-title: LDAPとAemのForms Workflow
description: 送信者のマネージャーへのAEM Formsワークフロータスクの割り当て
seo-description: 送信者のマネージャーへのAEM Formsワークフロータスクの割り当て
feature: アダプティブForms，ワークフロー
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
topic: 開発
role: Administrator
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 1%

---


# AEM Forms WorkflowでのLDAPの使用

送信者のマネージャーへのAEM Formsワークフロータスクの割り当て

AEMワークフローでアダプティブフォームを使用する場合、フォーム送信者のマネージャーにタスクを動的に割り当てる必要があります。 この使用例を実現するには、AEMとLDAPを設定する必要があります。

LDAPを使用してAEMを設定するために必要な手順については、[詳細を参照してください。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

この記事の目的で、AEMとLDAPの連携を設定する際に使用する設定ファイルを添付します。AdobeLDAP これらのファイルは、パッケージマネージャーを使用してインポートできるパッケージに含まれます。

次のスクリーンショットでは、特定のコストセンターに属するすべてのユーザーを取得しています。 LDAP内のすべてのユーザーを取得する場合は、追加のフィルターを使用できません。

![LDAP の設定](assets/costcenterldap.gif)

以下のスクリーンショットでは、LDAPからAEMに取得したユーザーにグループを割り当てます。 読み込まれたユーザーにforms-usersグループが割り当てられていることに注意してください。 AEM Formsとやり取りするには、ユーザーがこのグループのメンバーである必要があります。 また、managerプロパティは、AEMのprofile/managerノードの下にも保存されます。

![Synchandler](assets/synchandler.gif)

LDAPを設定し、AEMにユーザーを読み込むと、送信者のマネージャーにタスクを割り当てるワークフローを作成できます。 この記事では、シンプルな1ステップの承認ワークフローを開発しました。

ワークフローの最初の手順で、 initialstepの値を「いいえ」に設定します。 アダプティブフォーム内のビジネスルールでは、「送信者の詳細」パネルが無効になり、初期手順の値に基づいて「承認者」パネルが表示されます。

2番目の手順では、送信者のマネージャーにタスクを割り当てます。 カスタムコードを使用して、送信者のマネージャーを取得します。

![Assign Task](assets/assigntask.gif)

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

コードスニペットは、マネージャーIDを取得し、タスクをマネージャーに割り当てる役割を持ちます。

ワークフローを開始したユーザーを把握できます。 次に、 managerプロパティの値を取得します。

managerプロパティをLDAPに保存する方法に応じて、マネージャーIDを取得するには、文字列操作が必要になる場合があります。

この記事を読んで、独自の[ ParticipantChooser .](https://helpx.adobe.com/jp/experience-manager/using/dynamic-steps.html)を実装してください。

(Adobe従業員の場合は、このサンプルをすぐに使用できます)

* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、マネージャーのプロパティを設定するためのカスタムOSGIバンドルです。
* [DevelopingWithServiceUserBundleをダウンロードしてインストールする](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [パッケージマネージャーを使用して、この記事に関連付けられたアセットをAEMに読み込みます](assets/aem-forms-ldap.zip)。このパッケージの一部として、LDAP設定ファイル、ワークフロー、およびアダプティブフォームが含まれます。
* 適切なLDAP資格情報を使用して、LDAPでAEMを設定します。
* LDAP資格情報を使用してAEMにログインします。
* [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開きます。
* フォームに入力して送信します。
* 送信者の管理者は、確認用のフォームを取得する必要があります。

>[!NOTE]
>
>マネージャー名を抽出するこのカスタムコードは、AdobeLDAPに対してテストされています。 このコードを別のLDAPに対して実行する場合は、独自のgetParticipant実装を変更または記述して、マネージャーの名前を取得する必要があります。
