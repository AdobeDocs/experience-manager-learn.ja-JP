---
title: AEM Assets でのメタデータ駆動型の権限
description: メタデータ駆動型の権限は、フォルダー構造ではなくアセットのメタデータプロパティに基づいてアクセスを制限するのに使用される機能です。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
duration: 158
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 54%

---

# メタデータ駆動型の権限{#metadata-driven-permissions}

メタデータ駆動型の権限は、フォルダー構造ではなくアセットのメタデータプロパティに基づいて、AEM Assets オーサーに対するアクセス制御の決定を可能にするのに使用される機能です。この機能を使用すると、アセットのステータス、タイプまたは定義したカスタムメタデータプロパティなどの属性を評価するアクセス制御ポリシーを定義できます。

例を見てみましょう。クリエイティブは、自分の作品を AEM Assets のキャンペーン関連フォルダーにアップロードします。これは、使用が承認されていない処理中のアセットである可能性があります。マーケターには、このキャンペーンで承認されたアセットのみが表示されていることを確認します。メタデータプロパティを利用すると、アセットが承認され、マーケターが使用できることを示すことができます。

## 仕組み

メタデータ駆動型の権限を有効にするには、「ステータス」や「ブランド」など、アクセス制限を推進するアセットメタデータプロパティを定義する必要があります。これらのプロパティを使用すると、特定のプロパティ値を持つアセットにアクセスできるユーザーグループを指定するアクセス制御エントリを作成できます。

## 前提条件

メタデータ駆動型の権限を設定するには、最新バージョンに更新された AEM as a Cloud Service 環境へのアクセスが必要です。

## OSGi 設定 {#configure-permissionable-properties}

メタデータ駆動権限を実装するには、開発者は OSGi 設定をAEM as a Cloud Serviceにデプロイする必要があります。これにより、特定のアセットメタデータプロパティで、メタデータ駆動権限を強化できます。

1. アクセス制御に使用するアセットメタデータプロパティを決定します。プロパティ名は、アセットの `jcr:content/metadata` リソース上の JCR プロパティ名です。 ここでは、`status` というプロパティにします。
1. AEM Maven プロジェクトに OSGi 設定 `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` を作成します。
1. 作成したファイルに次の JSON を貼り付けます。

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. プロパティ名を必要な値に置き換えます。

## 基本アセット権限のリセット

制限ベースのアクセス制御エントリを追加する前に、新しいト上位レベルのエントリを追加して、アセットの権限評価の対象となるすべてのグループ（「寄稿者」など）への読み取りアクセスを最初に拒否する必要があります。

1. __ツール→セキュリティ →権限__ 画面に移動します
1. __投稿者__ グループ（またはすべてのユーザーグループが属する他のカスタムグループ）を選択します
1. 画面の右上隅にある「__ACE を追加__」をクリックします。
1. __パス__ の `/content/dam` を選択
1. __権限__ の `jcr:read` を入力
1. __権限タイプ__ の `Deny` を選択
1. 「制限」で「`rep:ntNames`」を選択し、「__制限値__」として「`dam:Asset`」と入力します
1. 「__保存__」をクリックします。

![アクセスを拒否](./assets/metadata-driven-permissions/deny-access.png)

## メタデータによるアセットへのアクセス権の付与

アクセス制御エントリを追加して、[ 設定されたアセットメタデータプロパティ値 ](#configure-permissionable-properties) に基づいてユーザーグループに読み取りアクセス権を付与できるようになりました。

1. __ツール→セキュリティ →権限__ 画面に移動します
1. アセットにアクセスできるユーザーグループを選択します
1. 画面の右上隅にある「__ACE を追加__」をクリックします。
1. __Path__ の `/content/dam` （またはサブフォルダー）を選択
1. __権限__ の `jcr:read` を入力
1. __権限タイプ__ の `Allow` を選択
1. __Restrictions__ で、OSGi 設定で [ 設定されたアセットメタデータプロパティ名の 1 つを選択します ](#configure-permissionable-properties)
1. 必須のメタデータプロパティ値を「__制限値__」フィールドに入力します
1. __+__ アイコンをクリックして、アクセス制御エントリに制限を追加します
1. 「__保存__」をクリックします。

![アクセスを許可](./assets/metadata-driven-permissions/allow-access.png)

## 有効なメタデータ駆動権限

サンプルフォルダーには、複数のアセットが含まれます。

![管理者のビュー](./assets/metadata-driven-permissions/admin-view.png)

権限を設定し、それに応じてアセットメタデータプロパティを設定すると、ユーザー（ここでは、マーケターユーザー）には、承認済みアセットのみが表示されます。

![マーケターのビュー](./assets/metadata-driven-permissions/marketeer-view.png)

## メリットと考慮事項

メタデータ駆動権限の利点は次のとおりです。

- 特定の属性に基づいて、アセットへのアクセスをきめ細かく制御します。
- アクセス制御ポリシーをフォルダー構造から切り離すことで、より柔軟なアセット編成が可能になります。
- 複数のメタデータプロパティに基づいて複雑なアクセス制御ルールを定義できます。

>[!NOTE]
>
> 次に注意することが重要です。
> 
> - メタデータプロパティは、__文字列等価__ （`=`）を使用して、制限に照らして評価されます（他のデータ型や演算子は、より大きい（`>`）プロパティや日付プロパティではサポートされていません）
> - 1 つの制限プロパティに対して複数の値を許可するには、「タイプを選択」ドロップダウンから同じプロパティを選択し、新しい制限値（例：`status=approved`、`status=wip`）を入力し、「+」をクリックして制限をエントリに追加することで、アクセス制御エントリに追加の制限を追加できます。
> ![複数の値を許可](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __AND 制限__ は、異なるプロパティ名（例：`status=approved`、`brand=Adobe`）の 1 つのアクセス制御エントリ内の複数の制限を介してサポートされます。つまり、選択されたユーザーグループには、`status=approved AND brand=Adobe` のアセットに対する読み取りアクセス権が付与され、AND 条件として評価されます
> ![複数の制限を許可](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __OR 制限__ は、メタデータプロパティ制限を持つ新しいアクセス制御エントリを追加することでサポートされ、エントリに OR 条件を確立します。例えば、制限 `status=approved` を持つ単一のエントリと、`brand=Adobe` を持つ単一のエントリが、`status=approved OR brand=Adobe` として評価されます
> ![複数の制限を許可](./assets/metadata-driven-permissions/allow-multiple-aces.png)
