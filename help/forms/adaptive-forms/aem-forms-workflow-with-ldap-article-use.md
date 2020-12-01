---
title: AemForms WorkflowでのLDAPの使用
seo-title: AemForms WorkflowでのLDAPの使用
description: 送信者の管理者へのAEM Formsワークフロータスクの割り当て
seo-description: 送信者の管理者へのAEM Formsワークフロータスクの割り当て
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 1%

---


# LDAPとAEM Formsワークフローの使用

送信者の管理者にAEM Formsワークフロータスクを割り当てています。

AEMワークフローでアダプティブフォームを使用する場合、タスクをフォーム送信者の管理者に動的に割り当てる必要があります。 この使用例を達成するには、AEMをLdapと設定する必要があります。

LDAPを使用してAEMを設定するために必要な手順は、[詳細](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)で説明します。

この記事の目的で、AdobeLDAPを使用したAEMの設定で使用する設定ファイルを添付します。 これらのファイルはパッケージに含まれ、パッケージマネージャーを使用して読み込むことができます。

下のスクリーンショットでは、特定のコストセンターに属するすべてのユーザーを取得しています。 LDAP内のすべてのユーザーを取得する場合は、追加のフィルターを使用できません。

![LDAP の設定](assets/costcenterldap.gif)

以下のスクリーンショットでは、LDAPからAEMに取り込んだユーザーにグループを割り当てます。 読み込まれたユーザーに割り当てられているforms-usersグループに注目してください。 ユーザーは、AEM Formsとのやり取りのために、このグループのメンバーである必要があります。 また、AEMのプロファイル/マネージャノードの下にmanagerプロパティを格納します。

![シンチャンドラー](assets/synchandler.gif)

LDAPを設定し、ユーザーをAEMに読み込むと、タスクを送信者の管理者に割り当てるワークフローを作成できます。 この記事の目的で、承認ワークフローを1ステップで簡単に作成しました。

ワークフローの最初の手順では、initialstepの値をNoに設定します。 アダプティブフォーム内のビジネスルールは、「送信者の詳細」パネルを無効にし、初期手順の値に基づいて「承認者」パネルを表示します。

2番目の手順では、タスクを送信者の管理者に割り当てます。 カスタムコードを使用して、送信者の管理者に連絡します。

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

コードスニペットでは、マネージャーIDを取得し、タスクをマネージャーに割り当てます。

ワークフローを開始したユーザーを入手できます。 次に、managerプロパティの値を取得します。

managerプロパティがLDAPにどのように保存されるかに応じて、manager idを取得するために、文字列操作が必要になる場合があります。

この記事を読んで、独自の[ ParticipantChooser .](https://helpx.adobe.com/jp/experience-manager/using/dynamic-steps.html)を実装してください。

これをお使いのシステムでテストするには(Adobeの従業員の場合は、このサンプルをすぐに使用できます)

* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、マネージャーのプロパティを設定するためのカスタムOSGIバンドルです。
* [DevelopingWithServiceUserBundleのダウンロードとインストール](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [パッケージマネージャーを使用して、この記事に関連付けられたアセットをAEMに読み込みます](assets/aem-forms-ldap.zip)。このパッケージの一部には、LDAP設定ファイル、ワークフロー、およびアダプティブフォームが含まれます。
* 適切なLDAP秘密鍵証明書を使用して、AEMをLDAPで設定します。
* LDAP資格情報を使用してAEMにログインします。
* [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開きます。
* フォームに入力し、送信します。
* 送信者の管理者は、レビュー用のフォームを取得する必要があります。

>[!NOTE]
>
>AdobeのLDAPに対して、マネージャー名を抽出するためのこのカスタムコードがテストされています。 このコードを別のLDAPに対して実行する場合は、独自のgetParticipant実装を変更または作成して管理者の名前を取得する必要があります。
