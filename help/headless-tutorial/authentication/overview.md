---
title: 外部アプリケーションからas a Cloud ServiceのAEMを認証する
description: ローカル開発のアクセストークンとサービス資格情報を使用して、外部アプリケーションが HTTP 経由でas a Cloud Serviceをプログラムで認証し、やり取りする方法を調べます。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 2%

---

# AEMas a Cloud Serviceに対するトークンベースの認証

AEMは、GraphQL、AEM Content Services から Assets HTTP API に至るまで、ヘッドレスな方法でやり取りできる様々な HTTP エンドポイントを公開します。 多くの場合、ヘッドレスの消費者は、保護されたコンテンツやアクションにアクセスするために、AEMへの認証が必要になる場合があります。 これを容易にするために、AEMは、外部のアプリケーション、サービスまたはシステムからの HTTP リクエストのトークンベースの認証をサポートします。

このチュートリアルでは、外部アプリケーションが、アクセストークンを使用して、HTTP 経由でas a Cloud Serviceを介してプログラムによる認証およびやり取りをおこなう方法について詳しく説明します。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 前提条件

このチュートリアルに従う前に、次の設定を行ってください。

1. am AEMas a Cloud Service環境（できれば開発環境またはサンドボックスプログラム）へのアクセス
1. AEMas a Cloud Service環境のオーサーサービスのAEM管理者製品プロファイルのメンバーシップ
1. Adobe IMS組織管理者のメンバーシップ（アクセス）（1 回限りの初期化を実行する必要があります） [サービス資格情報](./service-credentials.md))
1. 最新 [WKND サイト](https://github.com/adobe/aem-guides-wknd) Cloud Service環境にデプロイ済み

## 外部アプリケーションの概要

このチュートリアルでは、 [単純な Node.js アプリケーション](./assets/aem-guides_token-authentication-external-application.zip) コマンドラインから実行し、as a Cloud Service上のアセットメタデータを更新するには、次を使用します。 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ja).

Node.js アプリケーションの実行フローは次のとおりです。

![外部アプリケーション](./assets/overview/external-application.png)

1. Node.js アプリケーションは、コマンドラインから呼び出されます。
1. コマンドラインパラメータの定義：
   + 接続先のAEMas a Cloud Serviceオーサーサービスホスト (`aem`)
   + アセットが更新されるAEMアセットフォルダー (`folder`)
   + 更新するメタデータのプロパティと値 (`propertyName` および `propertyValue`)
   + AEMas a Cloud Service(`file`)
1. AEMへの認証に使用されるアクセストークンは、コマンドラインパラメーターで指定された JSON ファイルから取得されます `file`

   a.非ローカル開発用のサービス資格情報が JSON ファイル (`file`) の場合、アクセストークンはAdobe IMSAPI から取得されます
1. アプリケーションはアクセストークンを使用してAEMにアクセスし、コマンドラインパラメーターで指定されたフォルダー内のすべてのアセットをリストします `folder`
1. フォルダー内の各アセットのメタデータは、コマンドラインパラメーターで指定されたプロパティ名と値に基づいて更新されます `propertyName` および `propertyValue`

このサンプルアプリケーションは Node.js ですが、これらのやり取りは、異なるプログラミング言語を使用して開発し、他の外部システムから実行できます。

## ローカル開発アクセストークン

ローカル開発アクセストークンは、特定のAEMas a Cloud Service環境用に生成され、オーサーサービスとパブリッシュサービスへのアクセスを提供します。  これらのアクセストークンは一時的で、HTTP 経由でAEMとやり取りする外部アプリケーションやシステムの開発時にのみ使用されます。 開発者が正規のサービス資格情報を取得して管理する代わりに、一時的なアクセストークンを迅速かつ簡単に自動生成し、統合を開発できます。

+ [ローカル開発アクセストークンの使用方法](./local-development-access-token.md)

## サービス資格情報

サービス資格情報は、非開発シナリオ（ほとんどの場合は実稼動環境）で使用される正式な資格情報で、HTTP 経由でAEMを認証し、操作する外部アプリケーションまたはシステムの機能を容易にします。 サービス資格情報自体は、認証のためにAEMに送信されるのではなく、外部Adobe IMSがこれらの情報を使用して JWT を生成し、外部アプリケーションは JWT を生成し、この API と交換します _対象_ アクセストークン。AEM as a Cloud Serviceに対する HTTP リクエストの認証に使用できます。

+ [サービス資格情報の使用方法](./service-credentials.md)

## その他のリソース

+ [サンプルアプリケーションをダウンロード](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT の作成および交換のその他のコードサンプル
   + [Node.js、Java、Python、C#.NET および PHP のコード例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axios ベースのコードサンプル](https://github.com/adobe/aemcs-api-client-lib)
