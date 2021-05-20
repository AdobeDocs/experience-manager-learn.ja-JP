---
title: AEMを使用したOKTAの設定
description: oktaを使用したシングルサインオンの様々な設定について
feature: アダプティブフォーム
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
topic: 管理
role: Administrator
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 3%

---


# OKTAを使用したAEMオーサーへの認証

最初の手順は、OKTAポータルでアプリを設定することです。 OKTA管理者がアプリを承認すると、IdP証明書とシングルサインオンURLにアクセスできるようになります。 以下は、新しいアプリケーションの登録で一般的に使用される設定です。

* **アプリ名：** アプリ名です。アプリケーションに一意の名前を付けてください。
* **SAML Recipient:** OKTAからの認証後、SAML応答でAEMインスタンスにヒットされるURLです。通常、SAML認証ハンドラーは、/saml_loginを使用してすべてのURLを傍受しますが、アプリケーションのルートの後に追加する方が良いでしょう。
* **SAMLオーディエンス**:これは、アプリケーションのドメインURLです。ドメインURLにプロトコル（httpまたはhttps）を使用しないでください。
* **SAML Name ID:** ドロップダウンリストから「Email」を選択します。
* **環境**:適切な環境を選択します。
* **属性**:SAML応答でユーザーに関して取得する属性です。必要に応じて指定します。


![oktaアプリケーション](assets/okta-app-settings-blurred.PNG)


## OKTA(IdP)証明書をAEM Trust Storeに追加します

SAMLアサーションは暗号化されるので、OKTAとAEM間の安全な通信を可能にするために、AEM Trust StoreにIdP(OKTA)証明書を追加する必要があります。
[Trust Storeを初期化します](http://localhost:4502/libs/granite/security/content/truststore.html)（まだ初期化されていない場合）。Trust Storeのパスワードを記憶します。 このパスワードは後で使用する必要があります。

* [グローバルTrust Store](http://localhost:4502/libs/granite/security/content/truststore.html)に移動します。
* 「CERファイルから証明書を追加」をクリックします。 OKTAから提供されたIdP証明書を追加し、「送信」をクリックします。

   >[!NOTE]
   >
   >証明書をどのユーザーにもマップしないでください

Trust Storeに証明書を追加すると、以下のスクリーンショットに示すように証明書エイリアスが取得されます。 別のエイリアス名を使用する場合もあります。

![証明書エイリアス](assets/cert-alias.PNG)

**証明書エイリアスをメモします。後の手順でこれを行う必要があります。**

### SAML認証ハンドラーの設定

[configMgr](http://localhost:4502/system/console/configMgr)に移動します。
「AdobeGranite SAML 2.0 Authentication Handler」を検索して開きます。
以下に指定するプロパティを指定します
次に、指定する必要がある主要なプロパティを示します。

* **path**  — 認証ハンドラーがトリガーされるパスです
* **IdP Url**:OKTAから提供されるIdP URLです。
* **IDP Certificate Alias**:IdP証明書をAEM Trust Storeに追加したときに取得したエイリアス
* **Service Provider Entity Id**：これは、AEM Serverの名前です。
* **キーストアのパスワード**：使用したTrust Storeのパスワードです。
* **デフォルトのリダイレクト**：認証が成功した場合にリダイレクト先のURL
* **UserID属性**:uid
* **Use Encryption**:false
* **CRXユーザーの自動作成**:true
* **Add to Groups**:true
* **Default Groups**:oktausers(これは、ユーザーが追加されるグループです。AEM内に任意の既存のグループを指定できます)。
* **NamedIDPolicy**:要求された件名を表す名前識別子に対する制約を指定します。次のハイライト表示された文字列&#x200B;**urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**&#x200B;をコピーして貼り付けます。
* **同期済み属性**  - AEMプロファイルのSAMLアサーションから保存される属性です

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling Referrer Filterの設定

[configMgr](http://localhost:4502/system/console/configMgr)に移動します。
「Apache Sling Referrer Filter」を検索して開きます。以下に指定するプロパティを設定します。

* **空を許可**:true
* **ホストを許可**:IdPのホスト名（この場合は異なります）
* **正規表現ホストを許可**:IdPのホスト名（この場合は異なります） Sling Referrer Filterリファラープロパティのスクリーンショット

![referrer-filter](assets/sling-referrer-filter.PNG)

#### OKTA統合用のDEBUGログの設定

AEMでOKTA統合を設定する場合、AEM SAML認証ハンドラーのデバッグログを確認すると役立つ場合があります。 ログレベルをDEBUGに設定するには、AEM OSGi Webコンソールを使用して新しいSling Logger設定を作成します。

ログノイズを減らすには、必ずステージングと実稼動環境でこのロガーを削除または無効にしてください。

AEMでOKTA統合を設定する場合、AEM SAML認証ハンドラーのデバッグログを確認すると役立つ場合があります。 ログレベルをDEBUGに設定するには、AEM OSGi Webコンソールを使用して新しいSling Logger設定を作成します。
**ログノイズを減らすには、必ずステージングと実稼動環境でこのロガーを削除または無効にしてください。**
* [configMgr](http://localhost:4502/system/console/configMgr)に移動します。

* 「Apache Sling Logging Logger Configuration」を検索して開きます。
* 次の設定でロガーを作成します。
   * **ログレベル**:デバッグ
   * **ログファイル**:logs/saml.log
   * **ロガー**:com.adobe.granite.auth.saml
* 「保存」をクリックして設定を保存します。



#### OKTA設定のテスト

AEMインスタンスからログアウトします。 リンクにアクセスしてみてください。 OKTA SSOが動作していることを確認します。
