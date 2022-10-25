---
title: AEMas a Cloud Serviceの SAML 2.0
description: AEMas a Cloud Serviceパブリッシュサービスで SAML 2.0 認証を設定する方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: 343040.jpeg
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '2815'
ht-degree: 2%

---

# SAML 2.0 認証{#saml-2-0-authentication}

任意の SAML 2.0 互換 IDP に対して (AEM作成者ではなく ) エンドユーザーを設定し、認証する方法について説明します。

## SAML for AEMas a Cloud Service

AEM Publish（または Preview）との SAML 2.0 統合により、AEMベースの Web エクスペリエンスのエンドユーザーは、Adobe以外の IDP（ID プロバイダー）に対する認証を受け、AEMにネームドで認証済みのユーザーとしてアクセスできます。

|  | コンテンツ作成者 | AEM パブリッシュ |
|-----------------------|:----------:|:-----------:|
| SAML 2.0 のサポート | ✘ | ✔ |

+++ AEMでの SAML 2.0 フローの理解

AEM パブリッシュ SAML 統合の一般的なフローは次のとおりです。

1. ユーザーが AEM パブリッシュにリクエストをおこなった場合、は認証が必要であることを示します。
   + ユーザーが CUG/ACL で保護されたリソースをリクエストします。
   + ユーザーは、認証要件の対象となるリソースを要求します。
   + ユーザーは、AEMログインエンドポイント ( `/system/sling/login`) を使用してログインアクションを明示的に要求することができます。
1. AEMは、IDP に認証プロセスの開始を要求して、IDP に AuthnRequest を送信します。
1. ユーザーが IDP に対して認証をおこないます。
   + IDP に認証情報の入力を求められます。
   + ユーザーは既に IDP で認証されているので、追加の資格情報を提供する必要はありません。
1. IDP は、ユーザーのデータを含む SAML アサーションを生成し、IDP のプライベート証明書を使用して署名します。
1. IDP は、ユーザーの Web ブラウザーを介して HTTPPOSTを介して SAML アサーションを AEM パブリッシュに送信します。
1. AEM Publish は SAML アサーションを受け取り、IDP 公開証明書を使用して SAML アサーションの整合性と信頼性を検証します。
1. AEM Publish は、SAML 2.0 OSGi 設定と SAML アサーションの内容に基づいてAEMユーザーレコードを管理します。
   + ユーザーを作成
   + ユーザー属性を同期
   + AEMユーザーグループのメンバーシップを更新します
1. AEM Publish がAEMを設定 `login-token` HTTP 応答の cookie。AEM パブリッシュに対する以降の要求を認証するために使用されます。
1. AEM Publish は、 `saml_request_path` cookie.

+++

## 設定の手順

>[!VIDEO](https://video.tv.adobe.com/v/343040/?quality=12&learn=on)

このビデオでは、AEMas a Cloud Serviceのパブリッシュサービスとの SAML 2.0 統合の設定、および Okta を IDP として使用する方法について説明します。

## 前提条件

SAML 2.0 認証を設定する場合は、次の操作が必要です。

+ Cloud Manager への Deployment Manager アクセス
+ AEM AEM as a Cloud Service環境への管理者アクセス
+ IDP への管理者アクセス
+ オプションで、SAML ペイロードの暗号化に使用する公開鍵/秘密鍵のペアへのアクセス

SAML 2.0 は、AEM パブリッシュまたはプレビューに対するユーザーの認証にのみサポートされます。 と IDP を使用して AEM オーサーの認証を管理するには、次の手順を実行します。 [IDP とAdobe IMS](https://helpx.adobe.com/jp/enterprise/using/set-up-identity.html).


## AEMでの IDP 公開証明書のインストール

IDP の公開証明書がAEM Global Trust Store に追加され、IDP から送信される SAML アサーションが有効であることを検証するために使用されます。

+++SAML アサーション署名フロー

![SAML 2.0 - IDP SAML Assertion 署名](./assets/saml-2-0/idp-signing-diagram.png)

1. ユーザーが IDP に対して認証をおこないます。
1. IDP は、ユーザーのデータを含む SAML アサーションを生成します。
1. IDP は IDP のプライベート証明書を使用して SAML アサーションに署名します。
1. IDP が AEM パブリッシュの SAML エンドポイント (`.../saml_login`) に署名された SAML アサーションを含めます。
1. AEM Publish は、署名済み SAML アサーションを含む HTTPPOSTを受け取り、IDP 公開証明書を使用して署名を検証できます。

+++

![IDP 公開証明書をグローバルトラストストアに追加する](./assets/saml-2-0/global-trust-store.png)

1. の取得 __公開証明書__ ファイルを IDP から取得します。 この証明書により、AEMは IDP によってAEMに提供される SAML アサーションを検証できます。

   証明書は PEM 形式で、次のようになります。

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. AEM オーサーにAEM管理者としてログインします。
1. __ツール／セキュリティ／Trust Store__ に移動します。
1. グローバルトラストストアを作成するか、開きます。 グローバルトラストストアを作成する場合は、パスワードを安全な場所に保存します。
1. 展開 __CER ファイルから証明書を追加__.
1. 選択 __証明書ファイルを選択__、および IDP から提供される証明書ファイルをアップロードします。
1. 終了 __証明書をユーザーにマッピング__ 空白。
1. __送信__&#x200B;を選択します。
1. 新しく追加された証明書は、 __CRT ファイルから証明書を追加__ 」セクションに入力します。
1. をメモして __エイリアス__&#x200B;の場合は、この値が [SAML 2.0 認証ハンドラー OSGi 設定](#saml-2-0-authentication-handler-osgi-configuration).
1. 「__保存して閉じる__」を選択します。

グローバルトラストストアは、AEM オーサー上で IDP の公開証明書を使用して設定されますが、SAML は AEM パブリッシュでのみ使用されるので、IDP 公開証明書を AEM パブリッシュにアクセスするには、グローバルトラストストアを AEM パブリッシュにレプリケートする必要があります。

![AEM パブリッシュにグローバルトラストストアをレプリケート](./assets/saml-2-0/global-trust-store-replicate.png)

1. に移動します。 __ツール/導入/パッケージ__.
1. パッケージの作成
   + パッケージ名： `Global Trust Store`
   + バージョン: `1.0.0`
   + グループ：`com.your.company`
1. 新しい __グローバルトラストストア__ パッケージ。
1. を選択します。 __フィルター__ 」タブをクリックし、ルートパスのフィルターを追加します。 `/etc/truststore`.
1. 選択 __完了__ その後 __保存__.
1. を選択します。 __ビルド__ ボタン __グローバルトラストストア__ パッケージ。
1. ビルドが完了したら、「 」を選択します。 __詳細__ > __複製__ グローバルトラストストアノードをアクティベートするには、次の手順に従います (`/etc/truststore`) を AEM パブリッシュに追加します。

## AEMの公開鍵と秘密鍵のペアのインストール{#install-aem-public-private-key-pair}

_AEM公開鍵と秘密鍵のペアのインストールはオプションです_

AEM Publish は、AuthnRequests(IDP) に署名し、(AEMに )SAML アサーションを暗号化するように設定できます。 これは、AEM Publish に秘密鍵を提供することで実現され、公開鍵と IDP が一致します。

+++ AuthnRequest 署名フローについて（オプション）

AuthnRequest（ログインプロセスを開始する AEM Publish から IDP に対する要求）は、AEM Publish によって署名できます。 これをおこなうには、AEM Publish は秘密鍵を使用して AuthnRequest に署名します。IDP は公開鍵を使用して署名を検証します。 これにより、IDP に対して AuthnRequest が開始され、AEM Publish から要求されたことが保証され、悪意のあるサードパーティではなくなります。

![SAML 2.0 - SP AuthnRequest 署名](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. AEM パブリッシュに対して HTTP リクエストを実行すると、IDP に対する SAML 認証リクエストが実行されます。
1. AEM パブリッシュは、IDP に送信する SAML リクエストを生成します。
1. AEM パブリッシュは、AEM秘密鍵を使用して SAML リクエストに署名します。
1. AEM Publish が AuthnRequest を開始します。これは、署名済み SAML リクエストを含む IDP への HTTP クライアント側のリダイレクトです。
1. IDP は AuthnRequest を受け取り、AEM公開鍵を使用して署名を検証し、AEM Publish が AuthnRequest を開始することを保証します。
1. 次に、AEM Publish は、IDP 公開証明書を使用して、復号化された SAML アサーションの整合性と信頼性を検証します。

+++

+++ SAML アサーション暗号化フローの理解（オプション）

IDP と AEM Publish 間のすべての HTTP 通信は HTTPS 経由で行う必要があるので、デフォルトではセキュリティで保護されています。 ただし、必要に応じて、SAML アサーションは、HTTPS で提供される機密性の上に必要なイベントで暗号化できます。 これをおこなうには、IDP が秘密鍵を使用して SAML アサーションデータを暗号化し、AEM Publish が秘密鍵を使用して SAML アサーションを復号化します。

![SAML 2.0 - SP SAML アサーションの暗号化](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. ユーザーが IDP に対して認証をおこないます。
1. IDP は、ユーザーのデータを含む SAML アサーションを生成し、IDP のプライベート証明書を使用して署名します。
1. 次に、IDP はAEM公開鍵で SAML アサーションを暗号化します。この場合、AEM秘密鍵を復号化する必要があります。
1. 暗号化された SAML アサーションは、ユーザーの Web ブラウザーを通じて AEM パブリッシュに送信されます。
1. AEM Publish は SAML アサーションを受け取り、AEM秘密鍵を使用して復号化します。
1. IDP はユーザーに認証を求めます。

+++

AuthnRequest 署名と SAML アサーション暗号化はオプションですが、両方とも有効になっています。 [SAML 2.0 認証ハンドラーの OSGi 設定プロパティ `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)の場合は、両方を使用できるか、両方を使用できないかを示します。

![AEM authentication-service キーストア](./assets/saml-2-0/authentication-service-key-store.png)

1. AuthnRequest への署名と SAML アサーションの暗号化に使用する公開鍵、秘密鍵（PKCS#8 形式）、および証明書チェーンファイル（公開鍵の場合もあります）を取得します。 キーは、通常、IT 組織のセキュリティチームが提供します。

   + 自己署名キーペアは、 __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 公開鍵を IDP にアップロードします。
   + の使用 `openssl` 上記のメソッドの場合、公開鍵は `aem-public.crt` ファイル。
1. AEM オーサーにAEM Administrator としてログインし、秘密鍵をアップロードします。
1. に移動します。 __ツール/セキュリティ/Trust Store__&#x200B;を選択し、 __authentication-service__ ユーザーと選択 __プロパティ__ をクリックします。
1. に移動します。 __ツール/セキュリティ/ユーザー__&#x200B;を選択し、 __authentication-service__ ユーザーと選択 __プロパティ__ をクリックします。
1. を選択します。 __キーストア__ タブをクリックします。
1. キーストアを作成するか、開きます。 キーストアを作成する場合は、パスワードを安全に保ちます。
1. 選択 __DER ファイルから秘密鍵を追加__&#x200B;をクリックし、秘密鍵とチェーンファイルをAEMに追加します。
   + __エイリアス__:意味のある名前を指定します。多くの場合、IDP の名前を指定します。
   + __秘密鍵ファイル__:秘密鍵ファイル（DER 形式の PKCS#8）をアップロードします。
      + の使用 `openssl` 上のメソッド、これは `aem-private-pkcs8.der` ファイル
   + __証明書チェーンファイルを選択__:付属のチェーンファイル（公開鍵も含む）をアップロードします。
      + の使用 `openssl` 上のメソッド、これは `aem-public.crt` ファイル
   + 選択 __送信__
1. 新しく追加された証明書は、 __CRT ファイルから証明書を追加__ 」セクションに入力します。
   + をメモして __エイリアス__ これは [SAML 2.0 認証ハンドラー OSGi 設定](#saml-20-authentication-handler-osgi-configuration)
1. 「__保存して閉じる__」を選択します。
1. 選択 __authentication-service__ ユーザーと選択 __有効化__ をクリックします。

## SAML 2.0 認証ハンドラーの設定{#configure-saml-2-0-authentication-handler}

AEM SAML 設定は、 __AdobeGranite SAML 2.0 認証ハンドラー__ OSGi 設定。
設定は OSGi ファクトリ設定です。つまり、1 つのAEMas a Cloud Serviceパブリッシュサービスがリポジトリの個別のリソースツリーをカバーする複数の SAML 設定を持つ場合があります。これは、マルチサイトAEMのデプロイメントに役立ちます。

+++ SAML 2.0 認証ハンドラー OSGi 設定用語集

### AdobeGranite SAML 2.0 認証ハンドラー OSGi 設定{#configure-saml-2-0-authentication-handler-osgi-configuration}

|  | OSGi のプロパティ | 必須 | 値の形式 | デフォルト値 | 説明 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| パス | `path` | ✔ | 文字列配列 | `/` | AEMは、この認証ハンドラーが使用するパスを指定します。 |
| IDP URL | `idpUrl` | ✔ | String |  | SAML 認証リクエストが送信される IDP URL。 |
| IDP 証明書エイリアス | `idpCertAlias` | ✔ | 文字列 |  | AEM Global Trust Store で見つかった IDP 証明書のエイリアス |
| IDP HTTP redirect | `idpHttpRedirect` | ✘ | ブール値 | `false` | AuthnRequest を送信する代わりに IDP URL に HTTP リダイレクトするかどうかを示します。 に設定 `true` （IDP が開始した認証用） |
| IDP 識別子 | `idpIdentifier` | ✘ | 文字列 |  | AEMのユーザーとグループの一意性を確保する一意の IDP ID。 空の場合、 `serviceProviderEntityId` が代わりに使用されます。 |
| アサーションコンシューマーサービスの URL | `assertionConsumerServiceURL` | ✘ | 文字列 |  | この `AssertionConsumerServiceURL` AuthnRequest の URL 属性で、 `<Response>` メッセージをAEMに送信する必要があります。 |
| SP エンティティ ID | `serviceProviderEntityId` | ✔ | 文字列 |  | IDP に対してAEMを一意に識別する。通常はAEMホスト名です。 |
| SP の暗号化 | `useEncryption` | ✘ | ブール値 | `true` | IDP が SAML アサーションを暗号化するかどうかを示します。 必要 `spPrivateKeyAlias` および `keyStorePassword` を設定します。 |
| SP 秘密鍵のエイリアス | `spPrivateKeyAlias` | ✘ | 文字列 |  | の秘密鍵のエイリアス `authentication-service` ユーザーのキーストア。 必須 `useEncryption` が `true`. |
| SP キーストアのパスワード | `keyStorePassword` | ✘ | 文字列 |  | 「authentication-service」ユーザーのキーのパスワードが保存されます。 必須 `useEncryption` が `true`. |
| デフォルトのリダイレクト | `defaultRedirectUrl` | ✘ | 文字列 | `/` | 認証成功後のデフォルトのリダイレクト URL。 AEMホストに対する相対パスを指定できます ( 例： `/content/wknd/us/en/html`) をクリックします。 |
| ユーザー ID 属性 | `userIDAttribute` | ✘ | 文字列 | `uid` | AEMユーザーのユーザー ID を含む SAML アサーション属性の名前。 空のままにして `Subject:NameId`. |
| AEMユーザーの自動作成 | `createUser` | ✘ | ブール値 | `true` | 正常な認証でAEMユーザーが作成されるかどうかを示します。 |
| AEM user 中間パス | `userIntermediatePath` | ✘ | 文字列 |  | AEMユーザーを作成する場合、この値が中間パスとして使用されます ( 例： `/home/users/<userIntermediatePath>/jane@wknd.com`) をクリックします。 必要 `createUser` 設定 `true`. |
| AEMユーザー属性 | `synchronizeAttributes` | ✘ | 文字列配列 |  | AEMユーザーに保存する SAML 属性マッピングのリスト（形式） `[ "saml-attribute-name=path/relative/to/user/node" ]` ( 例： `[ "firstName=profile/givenName" ]`) をクリックします。 詳しくは、 [ネイティブAEM属性の完全なリスト](#aem-user-attributes). |
| ユーザーをAEMグループに追加 | `addGroupMemberships` | ✘ | ブール値 | `true` | 認証に成功した後、AEMユーザーがAEMユーザーグループに自動的に追加されるかどうかを示します。 |
| AEM group メンバーシップ属性 | `groupMembershipAttribute` | ✘ | 文字列 | `groupMembership` | ユーザーの追加先となるAEMユーザーグループのリストを含む SAML アサーション属性の名前。 必要 `addGroupMemberships` 設定 `true`. |
| デフォルトのAEMグループ | `defaultGroups` | ✘ | 文字列配列 |  | 認証済みユーザーのAEMユーザーグループのリストが常に追加されます ( 例： `[ "wknd-user" ]`) をクリックします。 必要 `addGroupMemberships` 設定 `true`. |
| NameIDPolicy 形式 | `nameIdFormat` | ✘ | 文字列 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | AuthnRequest メッセージで送信する NameIDPolicy 形式パラメーターの値。 |
| SAML 応答を保存 | `storeSAMLResponse` | ✘ | ブール値 | `false` | が `samlResponse` の値がAEMに保存される `cq:User` ノード。 |
| ログアウトを処理 | `handleLogout` | ✘ | ブール値 | `false` | ログアウトリクエストがこの SAML 認証ハンドラーによって処理されるかどうかを示します。 必要 `logoutUrl` を設定します。 |
| ログアウト URL | `logoutUrl` | ✘ | 文字列 |  | SAML ログアウトリクエストの送信先となる IDP の URL。 必須 `handleLogout` が `true`. |
| クロック許容値 | `clockTolerance` | ✘ | 整数値（Integer） | `60` | SAML アサーションを検証する際の IDP およびAEM (SP) クロックの歪み許容値。 |
| Digest メソッド | `digestMethod` | ✘ | 文字列 | `http://www.w3.org/2001/04/xmlenc#sha256` | SAML メッセージへの署名時に IDP が使用するダイジェストアルゴリズムです。 |
| 署名メソッド | `signatureMethod` | ✘ | 文字列 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | SAML メッセージの署名時に IDP が使用する署名アルゴリズムです。 |
| ID 同期タイプ | `identitySyncType` | ✘ | `default` か `idp` のどちらかにする必要があります。 | `default` | 変更しない `from` AEM as a Cloud Serviceのデフォルトです。 |
| サービスのランキング | `service.ranking` | ✘ | 整数値（Integer） | `5002` | 同じに対しては、上位のランク設定をお勧めします `path`. |

### AEMユーザー属性{#aem-user-attributes}

AEMは、次のユーザー属性を使用します。この属性は、 `synchronizeAttributes` プロパティ (AdobeGranite SAML 2.0 Authentication Handler OSGi 設定 )  任意の IDP 属性は任意のAEMユーザープロパティに同期できますが、AEMの属性プロパティ（以下に示す）を使用するようにマッピングすると、AEMで自然に使用できます。

| ユーザー属性 | からの相対プロパティパス `rep:User` ノード |
|--------------------------------|--------------------------|
| タイトル（例： ） `Mrs`) | `profile/title` |
| 名（名） | `profile/givenName` |
| 姓（姓） | `profile/familyName` |
| 役職 | `profile/jobTitle` |
| メールアドレス | `profile/email` |
| 住所 | `profile/street` |
| City（市区町村） | `profile/city` |
| 郵便番号 | `profile/postalCode` |
| 国 | `profile/country` |
| 電話番号 | `profile/phoneNumber` |
| 会社情報 | `profile/aboutMe` |

+++

1. 次の場所にあるプロジェクトで OSGi 設定ファイルを作成します。 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` を開き、IDE でを開きます。
   + 変更 `/wknd-examples/` を `/<project name>/`
   + の後の識別子 `~` ファイル名でこの設定を一意に識別する必要があるので、IDP の名前を次のように指定できます。 `...~okta.cfg.json`. 値は英数字でハイフンを使用する必要があります。
1. 次の JSON を `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` ファイルを作成し、 `wknd` 必要に応じて参照します。

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. プロジェクトの必要に応じて値を更新します。 詳しくは、 __SAML 2.0 認証ハンドラー OSGi 設定用語集__ 上記の設定プロパティの説明
1. OSGi 環境変数とシークレットの使用、リリースサイクルとの値の同期が解除される場合、または類似の環境タイプ/サービス層で値が異なる場合に、OSGi 環境変数とシークレットを使用することをお勧めしますが、必須ではありません。 デフォルト値は、 `$[env:..;default=the-default-value]"` 構文を使用します。

環境ごとの OSGi 設定 (`config.publish.dev`, `config.publish.stage`、および `config.publish.prod`) は、SAML 設定が環境によって異なる場合に、特定の属性を使用して定義できます。

### 暗号化を使用

条件 [AuthnRequest と SAML アサーションの暗号化](#encrypting-the-authnrequest-and-saml-assertion)に設定する場合は、次のプロパティが必要です。 `useEncryption`, `spPrivateKeyAlias`、および `keyStorePassword`. この `keyStorePassword` にはパスワードが含まれているので、値は OSGi 設定ファイルに格納せず、を使用して挿入します。 [シークレットの設定値](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++（オプション）暗号化を使用するように OSGi 設定を更新します

1. 開く `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` を IDE に追加します。
1. 3 つのプロパティを追加します。 `useEncryption`, `spPrivateKeyAlias`、および `keyStorePassword` 以下に示すように。

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. 暗号化に必要な 3 つの OSGi 設定プロパティは次のとおりです。

+ `useEncryption` を `true` に設定
+ `spPrivateKeyAlias` には、SAML 統合で使用される秘密鍵のキーストアエントリエイリアスが含まれます。
+ `keyStorePassword` 次を含む [OSGi シークレットの設定変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) を含む `authentication-service` ユーザーキーストアのパスワード。

+++

## リファラーフィルターの設定

SAML 認証プロセス中に、IDP が AEM パブリッシュの `.../saml_login` 終点。 IDP と AEM Publish が異なる接触チャネルに存在する場合、AEM Publish の __リファラーフィルター__ は、OSGi 設定を介して IDP のオリジンからの HTTP POST を許可するように設定されます。

1. 次の場所にあるプロジェクトで OSGi 設定ファイルを作成（または編集）します。 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + 変更 `/wknd-examples/` を `/<project name>/`
1. 次を確認します。 `allow.empty` の値は次の値に設定されます。 `true`、 `allow.hosts` ( または、 `allow.hosts.regexp`) は IDP の起源を含み、 `filter.methods` 次を含む `POST`. OSGi の設定は次のようになります。

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish は、1 つのリファラーフィルター設定をサポートしているので、SAML 設定要件と既存の設定を結合します。

環境ごとの OSGi 設定 (`config.publish.dev`, `config.publish.stage`、および `config.publish.prod`) は、特定の属性で定義できます ( `allow.hosts` ( または `allow.hosts.regex`) は環境間で異なります。

## クロスオリジンリソース共有 (CORS) の設定

SAML 認証プロセス中に、IDP が AEM パブリッシュの `.../saml_login` 終点。 IDP と AEM Publish が異なるホストやドメインに存在する場合、AEM Publish の __CRoss-Origin リソース共有 (CORS)__ は、IDP のホストまたはドメインからの HTTP POST を許可するように設定されている必要があります。

この HTTPPOSTリクエストの `Origin` ヘッダーの値は通常、AEM パブリッシュホストとは異なるので、CORS を設定する必要があります。

ローカルAEM SDK(`localhost:4503`) の場合、IDP は `Origin` ヘッダー `null`. その場合は、 `"null"` から `alloworigin` リスト。

1. 次の場所にあるプロジェクトで OSGi 設定ファイルを作成します。 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + 変更 `/wknd-examples/` プロジェクト名
   + の後の識別子 `~` ファイル名でこの設定を一意に識別する必要があるので、IDP の名前を次のように指定できます。 `...CORSPolicyImpl~okta.cfg.json`. 値は英数字でハイフンを使用する必要があります。
1. 次の JSON を `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` ファイル。

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

環境ごとの OSGi 設定 (`config.publish.dev`, `config.publish.stage`、および `config.publish.prod`) は、特定の属性で定義できます ( `alloworigin` および `allowedpaths` 環境によって異なります。

## AEM Dispatcher を設定して SAML HTTP POST を許可する

IDP への認証が正常に完了すると、IDP は HTTPPOSTを調整してAEMに登録します `/saml_login` エンドポイント（IDP で設定） この HTTPPOST `/saml_login` は、デフォルトで Dispatcher でブロックされているので、次の Dispatcher ルールを使用して明示的に許可する必要があります。

1. 開く `dispatcher/src/conf.dispatcher.d/filters/filters.any` を IDE に追加します。
1. ファイルの下部に、で終わる URL への HTTP POST の許可ルールを追加します。 `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

Apache Web サーバーで URL の書き換えが設定されている場合 (`dispatcher/src/conf.d/rewrites/rewrite.rules`) に設定され、 `.../saml_login` エンドポイントが誤ってマングされることはありません。

## データ同期の有効化とトークンのカプセル化

SAML 認証フローによって AEM パブリッシュにユーザーが作成されると、AEMユーザーノードは AEM パブリッシュサービス層全体で認証可能になります。
これには、 [データ同期](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#data-synchronization) および [カプセル化されたトークン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#sticky-sessions-and-encapsulated-tokens) を有効にします。

リクエストをAdobeカスタマーサポートに ( [AdminConsole](https://adminconsole.adobe.com) > サポート ) からのリクエスト：

> プログラム X と環境 Y の AEM パブリッシュサービスで、データの同期とカプセル化されたトークンを有効にします。

## SAML 設定のデプロイ

OSGi 設定は、Git にコミットし、Cloud Manager を使用してAEM as a Cloud Serviceにデプロイする必要があります。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

ターゲットの Cloud Manager Git ブランチ ( この例では `develop`) で、フルスタックデプロイメントパイプラインを使用します。
