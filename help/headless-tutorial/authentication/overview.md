---
title: 外部アプリケーションからのCloud ServiceとしてのAEMへの認証
description: ローカル開発アクセストークンとサービス資格情報を使用して、HTTP経由のCloud Serviceとして、外部アプリケーションがAEMをプログラムで認証し、やり取りする方法を調べます。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: 「ヘッドレス、統合」
role: デベロッパー
level: 「中級、経験豊富」
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 2%

---


# Cloud ServiceとしてのAEMへのトークンベースの認証

このチュートリアルでは、外部アプリケーションがアクセストークンを使用して、HTTP経由のCloud ServiceとしてAEMをプログラム的に認証し、やり取りする方法について詳しく説明します。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 前提条件

このチュートリアルに従う前に、次の準備が整っていることを確認してください。

1. Cloud Service環境としてのam AEMへのアクセス(開発環境またはSandboxプログラムを使用することを推奨)
1. Cloud Service環境の作成者サービスAEM管理者製品プロファイルとしてのAEMのメンバーシップ
1. AdobeのIMS組織管理者のメンバーシップ（アクセス）（[サービス資格情報](./service-credentials.md)の1回限りの初期化を実行する必要があります）
1. お使いのCloud Service環境に展開された最新の[WKNDサイト](https://github.com/adobe/aem-guides-wknd)

## 外部アプリケーションの概要

このチュートリアルでは、コマンドラインから[単純なNode.jsアプリケーション](./assets/aem-guides_token-authentication-external-application.zip)を実行し、[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ja)を使用してAEMのアセットメタデータをCloud Serviceとして更新します。

Node.jsアプリケーションの実行フローは次のとおりです。

![外部アプリケーション](./assets/overview/external-application.png)

1. Node.jsアプリケーションは、コマンドラインから呼び出されます
1. コマンドラインパラメータの定義：
   + 接続先のCloud Service作成者サービスホストとしてのAEM (`aem`)
   + アセットが更新されるAEMアセットフォルダー(`folder`)
   + 更新するメタデータプロパティと値（`propertyName`と`propertyValue`）
   + AEMにCloud Serviceとしてアクセスするために必要な資格情報を提供するファイルへのローカルパス(`file`)
1. AEMへの認証に使用されるアクセストークンは、コマンドラインパラメータ`file`で指定されたJSONファイルから得られます

   a.ローカルでない開発に使用されたサービス資格情報がJSONファイル(`file`)に含まれている場合、アクセストークンはAdobeIMS APIから取得されます
1. このアクセストークンを使用してAEMにアクセスし、コマンドラインパラメータ`folder`で指定されたリスト内のすべてのアセットにします
1. フォルダー内の各アセットに対して、アプリケーションは、コマンドラインパラメーター`propertyName`と`propertyValue`で指定されたプロパティ名と値に基づいてメタデータを更新します

この例のアプリケーションはNode.jsですが、これらのインタラクションは異なるプログラミング言語を使用して開発され、他の外部システムから実行できます。

## 現地開発アクセストークン

特定のAEM用に、Cloud Service環境としてローカル開発アクセストークンが生成され、作成者および発行サービスへのアクセスが提供されます。  これらのアクセストークンは一時的なもので、AEMとHTTPを介してやり取りする外部アプリケーションまたはシステムの開発時にのみ使用されます。 開発者は、適切なサービス資格情報を取得して管理する必要がなく、一時的なアクセストークンをすばやく簡単に自己生成して、統合を開発できます。

+ [ローカル開発アクセストークンの使用方法](./local-development-access-token.md)

## サービス資格情報

サービス資格情報は、開発環境以外のあらゆるシナリオ（ほとんどの場合は明らかに実稼働環境）で使用される、堅牢な資格情報です。これにより、外部アプリケーションやシステムがHTTP経由のCloud ServiceとしてAEMを認証し、やり取りする機能を促進します。 サービス資格情報自体は、認証のためにAEMに送信されず、外部アプリケーションはJWTを使用してJWTを生成します。これはAdobeIMSのAPI _で_&#x200B;に交換され、AEMに対するHTTP要求をCloud Serviceとして認証するために使用できます。

+ [サービス資格情報の使用方法](./service-credentials.md)

## その他のリソース

+ [サンプルアプリケーションのダウンロード](./assets/aem-guides_token-authentication-external-application.zip)
+ JWTの作成および交換の他のコードサンプル
   + [Node.js、Java、Python、C#.NET、PHPのコードサンプル](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axiosベースのコードサンプル](https://github.com/adobe/aemcs-api-client-lib)
