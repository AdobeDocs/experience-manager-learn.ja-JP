---
title: AEMでのOAuth範囲の開発
description: Adobe Experience Managerの拡張可能なOAuth範囲を使用すると、エンドユーザーによって許可されたクライアントアプリケーションからのリソースに対するアクセス制御が可能になります。 次の図は、AEMのコンテキストでのリクエストフローを示しています。
version: 6.3, 6.4, 6.5
feature: 'ユーザーとグループ '
topics: authentication, security
activity: develop
audience: developer
doc-type: code
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 3%

---


# OAuth範囲の開発

Adobe Experience Managerの拡張可能なOAuth範囲を使用すると、エンドユーザーによって許可されたクライアントアプリケーションからのリソースに対するアクセス制御が可能になります。 次の図は、AEMのコンテキストでのリクエストフローを示しています。

![Oauthスコープのフロー](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEMには3つのスコープがあります。

* プロファイル
* オフラインアクセス
* レプリケーション

AEM拡張可能なOAuthスコープを使用すると、他のカスタムスコープを定義できます。 例えば、カスタムスコープを開発してAEMにデプロイすると、OAuthを介して承認されたモバイルアプリで、アセットの読み取りは制限され、書き込みは制限されません。

OAuthは、クライアントアプリケーションにAEMユーザーの資格情報を提供する必要がなく、アクセストークンを使用するので、クライアントアプリケーションを認証する推奨される方法です。

* [コードを表示する](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
