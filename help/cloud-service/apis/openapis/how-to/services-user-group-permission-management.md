---
title: 製品プロファイルとサービスユーザーグループの権限管理
description: AEM as a Cloud Serviceで製品プロファイルとサービスユーザーグループの権限を管理する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17429
thumbnail: KT-17429.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 3230a8e7-6342-4497-9163-1898700f29a4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 2%

---

# 製品プロファイルとサービスユーザーグループの権限管理

AEM as a Cloud Serviceで製品プロファイルとサービスユーザーグループの権限を管理する方法を説明します。

このチュートリアルでは、次の内容について説明します。

- 製品プロファイルとサービスとの関連付け。
- サービスユーザーグループの権限を更新しています。

## 背景

AEM API を使用する場合は、_製品プロファイル_ をAdobe Developer Console（または ADC）プロジェクトの _資格情報_ に割り当てる必要があります。 _製品プロファイル_ （および関連するサービス）は、AEM リソースにアクセスするための資格情報に _権限または認証_ を提供します。 次のスクリーンショットでは、AEM Assets オーサー API の _資格情報_ および _製品プロファイル_ を確認できます。

![ 資格情報と製品プロファイル ](../assets/how-to/API-Credentials-Product-Profile.png)

製品プロファイルは、1 つ以上の _サービス_ に関連付けられています。 AEM as a Cloud Serviceでは、_サービス_ は、リポジトリノード用に事前定義されたアクセス制御リスト（ACL）を持つユーザーグループを表し、詳細な権限管理を可能にします。

![ テクニカルアカウントユーザー製品プロファイル ](../assets/s2s/technical-account-user-product-profile.png)

API が正常に呼び出されると、ADC プロジェクトの資格情報を表すユーザーが、製品プロファイルおよびサービス設定に一致するユーザーグループと共に、AEM オーサーサービスに作成されます。

![ テクニカルアカウントユーザーメンバーシップ ](../assets/s2s/technical-account-user-membership.png)

上記のシナリオでは、ユーザー `1323d2...` はAEM オーサーサービスで作成され、ユーザーグループ `AEM Assets Collaborator Users - Service` および `AEM Assets Collaborator Users - author - Program XXX - Environment XXX` のメンバーです。

## Update Services ユーザーグループの権限

ほとんどの _サービス_ は、_サービス_ と同じ名前を持つAEM インスタンスのユーザーグループを介して、AEM リソースに _読み取り_ 権限を提供します。

資格情報（別名テクニカルアカウントユーザー）には、AEM リソースの _作成、更新、削除_ （CUD））などの追加の権限が必要な場合があります。 その場合は、AEM インスタンス内の _Services_ ユーザーグループの権限を更新できます。

例えば、AEM Assets オーサー API の呼び出しでGET以外のリクエストに対して [403 エラー ](../use-cases/invoke-api-using-oauth-s2s.md#403-error-for-non-get-requests) が発生した場合は、AEM インスタンスの _AEM Assets Collaborator Users - Service_ ユーザーグループの権限を更新できます。

権限ユーザーインターフェイスまたは [Sling リポジトリの初期化 ](https://sling.apache.org/documentation/bundles/repository-initialization.html) スクリプトを使用すると、AEM インスタンス内の標準のユーザーグループの権限を更新できます。

### 権限ユーザーインターフェイスを使用した権限の更新

権限ユーザーインターフェイスを使用してサービスユーザーグループ（`AEM Assets Collaborator Users - Service` など）の権限を更新するには、次の手順に従います。

- AEM インスタンスで **ツール**/**セキュリティ**/**権限** に移動します。

- サービスユーザーグループを検索します（例：`AEM Assets Collaborator Users - Service`）。

  ![ユーザーグループの検索](../assets/how-to/search-user-group.png)

- **ACE を追加** をクリックして、ユーザーグループに新しいアクセス制御エントリ（ACE）を追加します。

  ![ACE の追加 ](../assets/how-to/add-ace.png)

### リポジトリ初期化スクリプトを使用した権限の更新

リポジトリ初期化スクリプトを使用してサービスユーザーグループ（`AEM Assets Collaborator Users - Service` など）の権限を更新するには、次の手順に従います。

- お気に入りの IDE でAEM プロジェクトを開きます。

- `ui.config` モジュールに移動します

- 次の内容を含んだ `org.apache.sling.jcr.repoinit.RepositoryInitializer-services-group-acl-update.cfg.json` という名前 `ui.config/src/main/content/jcr_root/apps/<PROJECT-NAME>/osgiconfig/config.author` ファイルをに作成します。

  ```json
  {
      "scripts": [
          "set ACL for \"AEM Assets Collaborator Users - Service\" (ACLOptions=ignoreMissingPrincipal)",
          "    allow jcr:read,jcr:versionManagement,crx:replicate,rep:write on /content/dam",
          "end"
      ]
  }
  ```

- 変更をコミットしてリポジトリにプッシュします。

- [AEM フルスタックパイプライン ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline) を使用して、変更内容をCloud Manager インスタンスにデプロイします。

- また、**権限** ビューを使用して、ユーザーグループの権限を確認することもできます。 AEM インスタンスで **ツール**/**セキュリティ**/**権限** に移動します。

  ![ 権限ビュー ](../assets/how-to/permissions-view.png)

### 権限の確認

上記の方法のいずれかを使用して権限を更新した後、アセットメタデータを更新するPATCH リクエストが問題なく機能するようになりました。

![PATCH リクエスト ](../assets/how-to/patch-request.png)

## 概要

AEM as a Cloud Serviceの製品プロファイルとサービスユーザーグループに対する権限を管理する方法を学びました。 権限ユーザーインターフェイスまたはリポジトリ初期化スクリプトを使用して、AEM インスタンス内のサービスユーザーグループの権限を更新できます。
