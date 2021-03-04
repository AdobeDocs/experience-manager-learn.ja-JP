---
title: AEMでのOAuthスコープの開発
description: Adobe Experience Managerの拡張可能なOAuthスコープを使用すると、エンドユーザーによって承認されたクライアントアプリケーションからのリソースをアクセス制御できます。 次の図は、AEMのコンテキストでのリクエストフローを示しています。
version: 6.3, 6.4, 6.5
feature: 認証
topics: authentication, security
activity: develop
audience: developer
doc-type: code
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---


# OAuthスコープの開発

Adobe Experience Managerの拡張可能なOAuthスコープを使用すると、エンドユーザーによって承認されたクライアントアプリケーションからのリソースをアクセス制御できます。 次の図は、AEMのコンテキストでのリクエストフローを示しています。

![OAUTHスコープのフロー](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEMには3つのスコープが用意されています。

* プロファイル
* オフラインアクセス
* レプリケーション

AEM extensible OAuthスコープを使用すると、他のカスタムスコープを定義できます。 例えば、カスタムスコープを開発し、AEMにデプロイすると、OAuthを介して承認されたモバイルアプリで読み取りが制限され、アセットの書き込みは制限されません。

OAuthは、クライアントアプリケーションに対してAEMアクセストークンの資格情報を提供する必要がなく、ユーザーを使用するので、そのアプリケーションを承認する推奨される方法です。

* [コードの表示](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
