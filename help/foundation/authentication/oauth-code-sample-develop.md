---
title: AEMでの OAuth 範囲の開発
description: Adobe Experience Managerの拡張可能な OAuth 範囲を使用すると、エンドユーザーによって許可されたクライアントアプリケーションからのリソースに対するアクセス制御が可能になります。 次の図は、AEMのコンテキストでのリクエストフローを示しています。
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# OAuth 範囲の開発

Adobe Experience Managerの拡張可能な OAuth スコープを使用すると、エンドユーザーによって許可されたクライアントアプリケーションからのリソースに対するアクセス制御が可能になります。 次の図は、AEMのコンテキストでのリクエストフローを示しています。

![OAuth スコープフロー](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEMには 3 つのスコープがあります。

* プロファイル
* オフラインアクセス
* レプリケーション

AEM拡張可能な OAuth スコープを使用すると、他のカスタムスコープを定義できます。 例えば、カスタムスコープを開発し、AEMにデプロイすると、OAuth 経由で承認されたモバイルアプリが読み取りに制限され、アセットの書き込みに制限されなくなります。

OAuth は、クライアントアプリケーションに対してAEMユーザーの資格情報を提供する代わりにアクセストークンを使用するので、クライアントアプリケーションを認証する推奨される方法です。

* [コードを表示する](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
