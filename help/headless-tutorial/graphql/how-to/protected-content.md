---
title: AEMヘッドレスで保護されたコンテンツ
description: AEMヘッドレスでコンテンツを保護する方法を説明します。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-04-01T00:00:00Z
source-git-commit: c498783aceaf3bb389baaeaeefbe9d8d0125a82e
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 0%

---


# AEMヘッドレスでのコンテンツの保護

AEM Publish からAEMヘッドレスコンテンツを提供する場合、機密コンテンツを提供する際には、データの整合性とセキュリティを確保することが重要です。 この手順では、AEMヘッドレスGraphQL API エンドポイントで提供されるコンテンツの保護に関する手順を説明します。

以下のハウツーは対象としません。

- エンドポイントを直接保護する代わりに、エンドポイントが提供するコンテンツの保護に注力します。
- AEM Publish またはログイントークンの取得に対する認証。 認証方法と資格情報の受け渡しは、個々の使用例と実装に応じて異なります。

## ユーザーグループ

まず、 [ユーザーグループ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) 保護されたコンテンツへのアクセス権を持つユーザーを含む

![AEMヘッドレスで保護されたコンテンツユーザーグループ](./assets/protected-content/user-groups.png){align="center"}

ユーザーグループは、コンテンツフラグメントや参照元の他のアセットを含む、AEMヘッドレスコンテンツへのアクセスを割り当てます。

1. AEM Author に as a としてログインします。 **ユーザー管理者**.
1. に移動します。 **ツール** > **セキュリティ** > **グループ**.
1. 選択 **作成** をクリックします。
1. Adobe Analytics の **詳細** タブで、 **グループ ID** および **グループ名**.
   - グループ ID とグループ名は任意の名前にすることができますが、この例では名前が使用されます **AEMヘッドレス API ユーザー**.
1. 「**保存して閉じる**」を選択します。
1. 新しく作成したグループを選択し、「 」を選択します。 **有効化** をクリックします。

様々なレベルのアクセスが必要な場合は、様々なコンテンツに関連付けることができる複数のユーザーグループを作成します。

### ユーザーグループへのユーザーの追加

保護されたコンテンツへのAEMヘッドレスGraphQL API リクエストのアクセスを許可するには、ヘッドレスリクエストを特定のユーザーグループに属するユーザーに関連付けます。 次に、2 つの一般的な方法を示します。

1. **AEMas a Cloud Service [技術アカウント](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - AEM as a Cloud Service Developer Console でテクニカルアカウントを作成します。
   - テクニカルアカウントでAEM Author に 1 回ログインします。
   - を使用してテクニカルアカウントをユーザーグループに追加します。 **ツール/セキュリティ/グループ/ AEMヘッドレス API ユーザー/メンバー**.
   - **有効化** AEM Publish のテクニカルアカウントユーザーとユーザーグループの両方。
   - このメソッドでは、ヘッドレスクライアントが、特定のユーザーの資格情報なので、サービス資格情報をユーザーに公開しないようにする必要があり、共有しないでください。

   ![AEMテクニカルアカウントグループ管理](./assets/protected-content/group-membership.png){align="center"}

2. **特定ユーザー：**
   - ネームドユーザーを認証し、AEM Publish でユーザーグループに直接追加します。
   - このメソッドでは、ヘッドレスクライアントがAEM Publish でユーザー資格情報を認証し、AEMログインまたはアクセストークンを取得し、このトークンを使用してAEMへの後続の要求を実行する必要があります。 これを実現する方法の詳細については、このハウツーでは説明しません。また、実装によって異なります。

## コンテンツフラグメントの保護

コンテンツフラグメントの保護は、AEMヘッドレスコンテンツを保護するために不可欠で、コンテンツを閉じられたユーザーグループ (CUG) に関連付けることで実現します。 ユーザーがAEMヘッドレスGraphQL API に対してリクエストをおこなうと、返されるコンテンツはユーザーの CUG に基づいてフィルタリングされます。

![ヘッドレス CUG のAEM](./assets/protected-content/cugs.png){align="center"}

次の手順に従って、 [閉じられたユーザーグループ (CUG)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. AEM Author に as a としてログインします。 **DAM ユーザー**.
2. に移動します。 **アセット/ファイル** をクリックし、 **フォルダー** 保護するコンテンツフラグメントを含んでいます。 CUG は、別の CUG で上書きされない限り、階層的に、またエフェクトサブフォルダーに適用されます。
   - フォルダーのコンテンツを利用する他のチャネルに属するユーザーが、このユーザーグループに含まれていることを確認します。 または、CUG のリストに、これらのチャネルに関連付けられたユーザーグループを含めます。 そうでない場合、これらのチャネルからはコンテンツにアクセスできません。
3. フォルダーを選択し、「 」を選択します。 **プロパティ** をクリックします。
4. を選択します。 **権限** タブをクリックします。
5. 次に **グループ名** をクリックし、 **追加** ボタンをクリックして新しい CUG を追加します。
6. **保存** CUG を適用します。
7. **選択** アセットフォルダーを選択し、「 **公開** 適用された CUG を含むフォルダーをAEM Publish に送信します。このフォルダーは権限として評価されます。

保護する必要があるコンテンツフラグメントを含むすべてのフォルダーに対して、同じ手順を実行し、各フォルダーに正しい CUG を適用します。

現在は、AEMヘッドレスGraphQL API エンドポイントに対して HTTP リクエストがおこなわれた場合、リクエスト元ユーザーの指定された CUG からアクセス可能なコンテンツフラグメントのみが結果に含まれます。 ユーザーがコンテンツフラグメントへのアクセス権を持っていない場合、結果は空になりますが、200 HTTP ステータスコードが返されます。

### 参照元のコンテンツの保護

コンテンツフラグメントは、多くの場合、画像などの他のAEMコンテンツを参照します。 この参照元のコンテンツを保護するには、参照元のアセットが格納されているアセットフォルダーに CUG を適用します。 参照元のアセットは、通常、AEMヘッドレスGraphQL API とは異なるメソッドを使用してリクエストされます。 その結果、これらの参照元アセットへのリクエストでアクセストークンが渡される方法が異なる場合があります。

コンテンツのアーキテクチャによっては、参照されるすべてのコンテンツが確実に保護されるように、複数のフォルダーに CUG を適用する必要が生じる場合があります。

## 保護されたコンテンツのキャッシュを防ぐ

AEMas a Cloud Service [デフォルトで HTTP 応答をキャッシュ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) （パフォーマンスの強化のため） ただし、保護されたコンテンツの提供に問題が生じる可能性があります。 このようなコンテンツのキャッシュを防ぐには、 [特定のエンドポイントのキャッシュヘッダーを削除する](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) (AEMパブリッシュインスタンスの Apache 設定 )

次のルールを Dispatcher プロジェクトの Apache 設定ファイルに追加して、特定のエンドポイントのキャッシュヘッダーを削除します。

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Dispatcher や CDN によってコンテンツがキャッシュされないので、パフォーマンスの低下が生じます。 これは、パフォーマンスとセキュリティのトレードオフです。

## AEMヘッドレスGraphQL API エンドポイントの保護

このガイドでは、 [AEMヘッドレスGraphQL API エンドポイント](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint) それ自体に焦点を当てているのですが、むしろ、そのユーザーが提供するコンテンツのセキュリティ保護に重点を置いています。 匿名ユーザーを含むすべてのユーザーは、保護されたコンテンツを含むエンドポイントにアクセスできます。 ユーザーの閉じられたユーザーグループがアクセスできるコンテンツのみが返されます。 コンテンツにアクセスできない場合、AEMヘッドレス API 応答には 200 HTTP 応答のステータスコードが残りますが、結果は空になります。 通常、コンテンツの保護は十分です。エンドポイント自体は本質的に機密データを公開するわけではないので、この点で十分です。 エンドポイントを保護する必要がある場合は、を介してAEM Publish でエンドポイントに ACL を適用します。 [Sling リポジトリの初期化 (repoinit) スクリプト](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).

