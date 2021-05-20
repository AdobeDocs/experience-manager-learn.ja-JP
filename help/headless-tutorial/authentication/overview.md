---
title: 外部アプリケーションからのCloud ServiceとしてのAEMの認証
description: ローカル開発のアクセストークンとサービス資格情報を使用して、外部アプリケーションがHTTP経由でAEMをCloud Serviceとしてプログラムで認証およびやり取りする方法を調べます。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: ヘッドレス、統合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 3%

---


# AEM as aCloud Serviceへのトークンベースの認証

このチュートリアルでは、外部アプリケーションが、アクセストークンを使用して、HTTP経由でCloud ServiceとしてAEMに対してプログラムによる認証とやり取りをおこなう方法をよく調べます。

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 前提条件

このチュートリアルに従う前に、次の点を確認してください。

1. am AEM as aCloud Service環境（できれば、開発環境またはサンドボックスプログラム）へのアクセス
1. AEM as a Cloud Service環境のオーサーサービスAEM Administrator製品プロファイルのメンバーシップ
1. AdobeのIMS Org Administratorのメンバーシップまたはアクセス（[サービス資格情報](./service-credentials.md)の初期化を1回だけ実行する必要があります）
1. Cloud Service環境にデプロイされた最新の[WKNDサイト](https://github.com/adobe/aem-guides-wknd)

## 外部アプリケーションの概要

このチュートリアルでは、コマンドラインから[単純なNode.jsアプリケーション](./assets/aem-guides_token-authentication-external-application.zip)を実行し、[Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ja)を使用してCloud ServiceとしてAEMのアセットメタデータを更新します。

Node.jsアプリケーションの実行フローは次のとおりです。

![外部アプリケーション](./assets/overview/external-application.png)

1. Node.jsアプリケーションは、コマンドラインから呼び出されます。
1. コマンドラインパラメータの定義：
   + 接続先のAEM as a  as aCloud Serviceオーサーサービスホスト(`aem`)
   + アセットが更新されるAEMアセットフォルダー(`folder`)
   + 更新するメタデータのプロパティと値（`propertyName`と`propertyValue`）
   + AEM as aCloud Service(`file`)へのアクセスに必要な資格情報を提供するファイルへのローカルパス
1. AEMへの認証に使用されるアクセストークンは、コマンドラインパラメーター`file`で提供されるJSONファイルから取得されます

   a.非ローカル開発に使用されるサービス資格情報がJSONファイル(`file`)で指定されている場合、アクセストークンはAdobeIMS APIから取得されます
1. アプリケーションはアクセストークンを使用してAEMにアクセスし、コマンドラインパラメーター`folder`で指定されたフォルダー内のすべてのアセットをリストします
1. フォルダー内のアセットごとに、コマンドラインパラメーター`propertyName`と`propertyValue`で指定されたプロパティ名と値に基づいてメタデータが更新されます

このサンプルアプリケーションはNode.jsですが、これらのインタラクションは、異なるプログラミング言語を使用して開発し、他の外部システムから実行できます。

## ローカル開発アクセストークン

ローカル開発アクセストークンは、特定のAEM as aCloud Service環境用に生成され、オーサーサービスとパブリッシュサービスへのアクセスを提供します。  これらのアクセストークンは一時的なもので、HTTP経由でAEMとやり取りする外部アプリケーションやシステムの開発時にのみ使用されます。 開発者が正式なサービス資格情報を取得して管理する代わりに、一時的なアクセストークンをすばやく簡単に自己生成して、統合を開発できます。

+ [ローカル開発アクセストークンの使用方法](./local-development-access-token.md)

## サービス資格情報

サービス資格情報は、非開発シナリオ（ほとんどの場合、実稼動環境で使用される）で使用される正式な資格情報で、HTTP経由のCloud ServiceとしてAEMを認証および操作する外部アプリケーションまたはシステムの機能を支援します。 サービス資格情報自体は認証のためにAEMに送信されるのではなく、外部アプリケーションがJWTを使用して生成します。JWTはAdobeIMSのAPI _で_&#x200B;用に交換され、AEMに対するHTTP要求をCloud Serviceとして認証するために使用できます。

+ [サービス資格情報の使用方法](./service-credentials.md)

## その他のリソース

+ [サンプルアプリケーションのダウンロード](./assets/aem-guides_token-authentication-external-application.zip)
+ JWTの作成および交換のその他のコードサンプル
   + [Node.js、Java、Python、C#.NET、PHPのコード例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axiosベースのコードサンプル](https://github.com/adobe/aemcs-api-client-lib)
