---
title: コンテキスト対応の設定で、フォームデータモデルのサポートを上書きします
description: 環境に基づいて異なるエンドポイントと通信するフォームデータモデルを設定します。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 10%

---

# コンテキスト対応クラウド設定

ローカル環境でクラウド設定を作成し、成功したテストをおこなう場合、アップストリーム環境で同じクラウド設定を使用し、エンドポイント、秘密鍵/パスワード、ユーザー名を変更する必要はありません。 この使用例を実現するために、Cloud Service上のAEM Formsでは、コンテキスト対応クラウド設定を定義する機能が導入されました。
例えば、異なる接続文字列とキーを使用して、開発、ステージ、実稼動環境で Azure ストレージアカウントのクラウド設定を再利用できます。

コンテキスト対応クラウド設定を作成するには、次の手順が必要です

## 環境変数の作成

標準環境変数は、Cloud Manager を介して設定および管理できます。これらは実行時環境に提供され、OSGi 設定で使用できます。[環境変数には、環境固有の値または環境シークレットを変更内容に応じて指定できます。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



次のスクリーンショットは、定義された azure_key と azure_connection_string の環境変数を示しています
![environment_variables](assets/environment-variables.png)

その後、これらの環境変数は、適切な環境で使用する設定ファイルで指定できます。例えば、すべてのオーサーインスタンスでこれらの環境変数を使用する場合は、config.author フォルダーで次に示すように設定ファイルを定義します

## 設定ファイルを作成

IntelliJ でプロジェクトを開きます。 config.author に移動し、という名前のファイルを作成します。

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

前の手順で作成したファイルに、次のテキストをコピーします。 このファイル内のコードは、 accountName プロパティと accountKey プロパティの値を環境変数で上書きしています **azure_connection_string** および **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>この設定は、クラウドサービスインスタンス内のすべてのオーサー環境に適用されます。 パブリッシュ環境に設定を適用するには、同じ設定ファイルを intelliJ プロジェクトの config.publish フォルダーに配置する必要があります
>[!NOTE]
> 上書きされるプロパティがクラウド設定の有効なプロパティであることを確認してください。 クラウド設定に移動して、次に示すように、上書きするプロパティを見つけます。

![cloud-config-property](assets/cloud-config-properties.png)

基本的な認証を使用する REST ベースのクラウド設定の場合、通常は serviceEndPoint、userName、および password プロパティの環境変数を作成します。
