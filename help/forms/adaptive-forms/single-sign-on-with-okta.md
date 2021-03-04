---
title: AEMでのOKTAの設定
description: oktaを使用したシングルサインオンを使用するための様々な設定の理解
feature: アダプティブフォーム
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
topic: Administration
role: Administrator
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# OKTAを使用してAEM作成者に認証する

最初の手順は、OKTAポータルでアプリを設定することです。 アプリがOKTA管理者によって承認されると、IdP証明書とシングルサインオンURLにアクセスできるようになります。 以下は、新しいアプリケーションの登録で一般に使用される設定です。

* **アプリケーション名：** これはアプリケーション名です。アプリには一意の名前を付けてください。
* **SAML受信者:OKTA** からの認証後、SAML応答を使用してAEMインスタンスにヒットされるURLです。SAML認証ハンドラーは通常、/saml_loginを使用してURLのすべてを傍受しますが、アプリケーションのルートの後にURLを追加することをお勧めします。
* **SAMLオーディエンス**:これは、アプリケーションのドメインURLです。ドメインURLには、プロトコル（httpまたはhttps）を使用しないでください。
* **SAML名ID：ドロップダウンリストから「電子メール」を** 選択します。
* **環境**:適切な環境を選択します。
* **属性**:SAML応答でユーザーに関して取得する属性です。必要に応じて指定します。


![okta応用](assets/okta-app-settings-blurred.PNG)


## AEM 追加 Trust StoreへのOKTA(IdP)証明書

SAMLアサーションは暗号化されているので、OKTAとAEMとの間の安全な通信を可能にするために、AEM Trust StoreにIdP(OKTA)証明書を追加する必要があります。
[Trust Storeを初期化します](http://localhost:4502/libs/granite/security/content/truststore.html)（既に初期化されていない場合）。Trust Storeのパスワードを記憶しておきます。 このプロセスの後半で、このパスワードを使用する必要があります。

* [グローバルTrust Store](http://localhost:4502/libs/granite/security/content/truststore.html)に移動します。
* [CERファイルから追加の証明書]をクリックします。 OKTA追加が提供するIdP証明書を確認し、「送信」をクリックします。

   >[!NOTE]
   >
   >証明書をどのユーザーにもマップしないでください

Trust Storeに証明書を追加したら、次のスクリーンショットに示すように、証明書エイリアスを取得する必要があります。 エイリアス名は異なる場合があります。

![証明書エイリアス](assets/cert-alias.PNG)

**証明書エイリアスをメモしておきます。後の手順で必要になります。**

### SAML認証ハンドラーの設定

[configMgr](http://localhost:4502/system/console/configMgr)に移動します。
「Adobe御影石SAML 2.0 Authentication Handler」を検索して開きます。
以下に指定するプロパティを指定します
次に、指定する必要がある主なプロパティを示します。

* **path**  — これは、認証ハンドラーがトリガーされるパスです。
* **IdP Url**：これはOKTAから提供されるIdP URLです
* **IDP証明書エイリアス**:AEM Trust StoreにIdP証明書を追加したときに取得したエイリアスです。
* **サービスプロバイダーエンティティID**：これは、AEMサーバーの名前です
* **キーストアのパスワード**：これは、使用したTrust Storeのパスワードです。
* **デフォルトのリダイレクト**：これは、認証が成功した場合にリダイレクトされるURLです
* **UserID Attribute**:uid
* **Use Encryption**:false
* **CRXユーザーを自動作成**:true
* **グループ追加へ：true**
* **Default Groups**:oktausers(これは、ユーザーを追加する先のグループです。AEM内の任意の既存のグループを指定できます)。
* **NamedIDPolicy**:要求された件名を表すために使用する名前識別子に関する制約事項を指定します。次の強調表示された文字列&#x200B;**urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**&#x200B;をコピー&amp;ペーストします。
* **同期済み属性** - AEMプロファイルのSAMLアサーションから保存される属性です。

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling転送者フィルターの設定

[configMgr](http://localhost:4502/system/console/configMgr)に移動します。
「Apache Sling転送者フィルター」を検索して開きます。次のプロパティを設定します。

* **空白を許可**:true
* **ホストを許可**:IdPのホスト名（使用する場合は異なります）
* **Regexpホストを許可**:IdPのホスト名（実際の場合は異なります） Sling転送者フィルター転送者のプロパティのスクリーンショット

![転送者フィルタ](assets/sling-referrer-filter.PNG)

#### OKTA統合用のDEBUGログの設定

AEMでOKTA統合を設定する場合、AEM SAML認証ハンドラーのデバッグログを確認すると便利です。 ログレベルをDEBUGに設定するには、AEM OSGi Webコンソールを使用して新しいSling Logger設定を作成します。

ログノイズを減らすために、ステージと実稼働環境でこのロガーを削除または無効にしてください。

AEMでOKTA統合を設定する場合、AEM SAML認証ハンドラーのデバッグログを確認すると便利です。 ログレベルをDEBUGに設定するには、AEM OSGi Webコンソールを使用して新しいSling Logger設定を作成します。
**ログノイズを減らすために、ステージと実稼働環境でこのロガーを削除または無効にしてください。**
* [configMgr](http://localhost:4502/system/console/configMgr)に移動します

* 「Apache Sling Logging Logger Configuration」を検索して開きます。
* 次の設定でロガーを作成します。
   * **Log Level**:デバッグ
   * **ログファイル**:logs/saml.log
   * **Logger**:com.adobe.granite.auth.saml
* 「保存」をクリックして設定を保存します



#### OKTAの設定をテストする

AEMインスタンスからログアウトします。 リンクにアクセスしてみてください。 OKTA SSOが動作していることを確認してください。
