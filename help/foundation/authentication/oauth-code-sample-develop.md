---
title: AEM での OAuth 範囲の開発
description: Adobe Experience Manager の拡張可能な OAuth 範囲を使用すると、エンドユーザーによって許可されたクライアントアプリケーションからのリソースに対するアクセス制御が可能になります。 次の図は、AEM のコンテキストでのリクエストフローを示しています。
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: ht
source-wordcount: '179'
ht-degree: 100%

---

# OAuth 範囲の開発

Adobe Experience Manager の拡張可能な OAuth 範囲を使用すると、エンドユーザーによって許可されたクライアントアプリケーションからのリソースに対するアクセス制御が可能になります。 次の図は、AEM のコンテキストでのリクエストフローを示しています。

![OAuth 範囲フロー](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM には次の 3 つの範囲があります。

* プロファイル
* オフラインアクセス
* レプリケーション

AEM の拡張可能な OAuth 範囲を使用すると、他のカスタム範囲を定義できます。 例えば、カスタム範囲を開発して AEM にデプロイすると、OAuth 経由で承認されたモバイルアプリが読み込みに制限され、アセットへの書き込みは許可されなくなります。

OAuth は、クライアントアプリケーションに対して AEM ユーザーの資格情報を提供する代わりにアクセストークンを使用するので、クライアントアプリケーションを認証する推奨される方法です。

* [コードを表示する](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
