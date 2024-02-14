---
title: 外部アプリケーションからの AEM as a Cloud Service の認証
description: ローカル開発のアクセストークンとサービス資格情報を使用して、外部アプリケーションで HTTP を通じて AEM as a Cloud Service をプログラムで認証し、やり取りする方法を調べます。
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 288
source-git-commit: 0400242f6a99bc5209a8b483469d5fd88eac077e
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 100%

---

# AEM as a Cloud Service に対するトークンベースの認証

AEM は、GraphQL、AEM コンテンツサービスから Assets HTTP API まで、ヘッドレスな方法で操作できる様々な HTTP エンドポイントを公開します。多くの場合、これらのヘッドレスを使用するお客様は、保護されたコンテンツやアクションにアクセスするために、AEM に対する認証を行う必要があります。これを容易にするために、AEM は、外部のアプリケーション、サービス、システムからの HTTP リクエストのトークンベースの認証をサポートしています。

このチュートリアルでは、外部アプリケーションがアクセストークンを使用して、HTTP 経由で AEM as a Cloud Service に対してプログラムで認証とやり取りを行う方法について詳しく説明します。

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 前提条件

このチュートリアルを開始する前に、次の情報と環境が整っていることを確認します。

1. AEM as a Cloud Service 環境（開発環境またはサンドボックスプログラムが望ましい）へのアクセス
1. AEM as a Cloud Service 環境におけるオーサーサービス AEM 管理者製品プロファイルのメンバーシップ
1. Adobe IMS 組織管理者のメンバーシップまたはアクセス権（[サービス資格情報](./service-credentials.md)の初期化を 1 回のみ実行する必要があります）
1. Cloud Service 環境にデプロイされた最新の [WKND サイト](https://github.com/adobe/aem-guides-wknd)

## 外部アプリケーションの概要

このチュートリアルでは、コマンドラインから実行される[単純な Node.js アプリケーション](./assets/aem-guides_token-authentication-external-application.zip)を使用して、[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ja) で AEM as a Cloud Service のアセットメタデータを更新します。

Node.js アプリケーションの実行フローは次のとおりです。

![外部アプリケーション](./assets/overview/external-application.png)

1. Node.js アプリケーションは、コマンドラインから呼び出されます
1. コマンドラインパラメーターの定義：
   + 接続先の AEM as a Cloud Service オーサーサービスホスト（`aem`）
   + アセットが更新される AEM アセットフォルダー（`folder`）
   + 更新するメタデータのプロパティと値（`propertyName` と `propertyValue`）
   + AEM as a Cloud Service へのアクセスに必要な資格情報を提供するファイルへのローカルパス（`file`）
1. AEM への認証に使用されるアクセストークンは、コマンドラインパラメーター `file` で指定された JSON ファイルから取得します

   a. 非ローカル開発用のサービス資格情報が JSON ファイル（`file`）で提供されている場合、アクセストークンは Adobe IMS API から取得します
1. アプリケーションはアクセストークンを使用して AEM にアクセスし、コマンドラインパラメーター `folder` で指定されたフォルダー内のすべてのアセットをリスト表示します
1. フォルダー内の各アセットのメタデータは、コマンドラインパラメーター `propertyName` と `propertyValue` で指定されたプロパティ名と値に基づいてアプリケーションで更新されます

このサンプルアプリケーションは Node.js ですが、これらのやり取りは様々なプログラミング言語で開発し、その他の外部システムから実行できます。

## ローカル開発アクセストークン

ローカル開発アクセストークンは、特定の AEM as a Cloud Service 環境のために生成され、オーサーサービスとパブリッシュサービスへのアクセスを提供します。これらのアクセストークンは一時的で、HTTP 経由で AEM とやり取りする外部アプリケーションやシステムを開発するときにのみ使用されます。開発者が正規のサービス資格情報を取得して管理する代わりに、一時的なアクセストークンを迅速かつ簡単に自動生成し、統合を開発できます。

+ [ローカル開発アクセストークンの使用方法](./local-development-access-token.md)

## サービス資格情報

サービス資格情報は、非開発シナリオ（ほとんどの場合は実稼動環境）で使用される正式な資格情報で、HTTP 経由で AEM を認証し、操作する外部アプリケーションまたはシステムの機能を促進します。サービス資格情報自体は認証時に AEM に送信されず、外部アプリケーションがこれらの情報を使用して JWT を生成します。これは、Adobe IMS の API __ で、AEM as a Cloud Service への HTTP リクエストの認証に使用できるアクセストークンと交換されます。

+ [サービス資格情報の使用方法](./service-credentials.md)

## その他のリソース

+ [サンプルアプリケーションのダウンロード](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT の作成と交換に関するその他のコードサンプル
   + [Node.js、Java、Python、C#.NET、PHP のコードサンプル](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [JavaScript/Axios ベースのコードサンプル](https://github.com/adobe/aemcs-api-client-lib)
