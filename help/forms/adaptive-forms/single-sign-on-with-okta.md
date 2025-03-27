---
title: AEM での OKTA の設定
description: Okta を使用したシングルサインオンを使用するための様々な設定を理解する
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
duration: 153
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '733'
ht-degree: 100%

---

# OKTA を使用した AEM オーサーの認証

最初の手順は、OKTA ポータルでアプリを設定することです。OKTA 管理者がアプリを承認すると、IdP 証明書とシングルサインオン URL にアクセスできるようになります。以下は、新しいアプリケーションの登録で一般的に使用される設定です。

* **アプリ名：**&#x200B;これはアプリケーション名です。アプリケーションには必ず一意の名前を付けてください。
* **SAML 受信者：** OKTA からの認証後、これは SAML 応答で AEM インスタンスにヒットする URL です。SAML 認証ハンドラーは通常、/ saml_login ですべての URL をインターセプトしますが、アプリケーションルートの後に追加することをお勧めします。
* **SAML オーディエンス**：これは、アプリケーションのドメイン URL です。ドメイン URL にはプロトコル（http または https）を使用しないでください。
* **SAML 名 ID：**&#x200B;ドロップダウンリストから「メール」を選択します。
* **環境**：適切な環境を選択します。
* **属性**：SAML 応答で取得するユーザーに関する属性です。必要に応じて指定します。


![okta-application](assets/okta-app-settings-blurred.PNG)


## AEM Trust Store への OKTA（IdP）証明書の追加

SAML アサーションは暗号化されるため、OKTA と AEM の間の安全な通信を可能にするために、IdP（OKTA）証明書を AEM Trust Store に追加する必要があります。
まだ初期化されていない場合は、[Trust Store を初期化](http://localhost:4502/libs/granite/security/content/truststore.html)します。
Trust Store のパスワードは覚えておいてください。このパスワードは、このプロセスの後半で使用する必要があります。

* [Global Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html) に移動します。
* 「CER ファイルから証明書を追加」をクリックします。OKTA から提供された IdP 証明書を追加し、「送信」をクリックします。

  >[!NOTE]
  >
  >証明書はどのユーザーにもマッピングしないでください

Trust Store に証明書を追加すると、以下のスクリーンショットに示すように証明書エイリアスが取得されます。エイリアス名は異なる場合があります。

![証明書エイリアス](assets/cert-alias.PNG)

**証明書エイリアスをメモします。これは、後の手順で必要になります。**

### SAML 認証ハンドラーの設定

[configMgr](http://localhost:4502/system/console/configMgr) に移動します。 
「Adobe Granite SAML 2.0 認証ハンドラー」を検索して開きます。
以下のように、次のプロパティを指定します。
指定する必要がある主要なプロパティは次のとおりです。

* **パス** - 認証ハンドラーがトリガーされるパス
* **IdP URL**：OKTA から提供される IdP URL
* **IDP 証明書エイリアス**：IdP 証明書を AEM Trust Store に追加した際に取得したエイリアス
* **サービスプロバイダーエンティティ ID**：AEM サーバーの名前
* **キーストアのパスワード**：使用した Trust Store のパスワード
* **デフォルトのリダイレクト**：認証が成功したときにリダイレクトする URL
* **ユーザー ID 属性**：UID
* **暗号化を使用**：false
* **CRX ユーザーの自動作成**：true
* **グループに追加**：true
* **デフォルトのグループ**：oktausers（これは、ユーザーが追加されるグループです。 AEM 内の任意の既存のグループを指定できます）
* **NamedIDPolicy**：リクエストされたサブジェクトを表すために使用される名前識別子に関する制約を指定します。次のハイライト表示された文字列をコピーして貼り付けます。**urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **同期された属性** - AEM プロファイルの SAML アサーションから保存される属性です

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling リファラーフィルターを設定する

[configMgr](http://localhost:4502/system/console/configMgr) に移動します。
「Apache Sling リファラーフィルター」を検索して開きます。以下に指定されているように、次のプロパティを設定します。

* **Allow Empty**：false
* **Allow Hosts**：IdP のホスト名（これは、ケースで異なります）
* **Allow Regexp Host**：IdP のホスト名（これは、ケースで異なります）
Sling リファラーフィルターのリファラープロパティのスクリーンショット

![referrer-filter](assets/okta-referrer.png)

#### OKTA 統合用のデバッグログの設定

AEMで OKTA 統合を設定する場合、AEM の SAML 認証ハンドラーのデバッグログを確認すると役立つ場合があります。ログレベルをデバッグに設定するには、AEM OSGi web コンソールで新しい Sling Logger 設定を作成します。

ログノイズを減らすには、ステージング環境と実稼動環境で、このロガーを必ず削除または無効にしてください。

AEM で OKTA 統合を設定する場合、AEM の SAML 認証ハンドラーのデバッグログを確認すると役立つ場合があります。ログレベルをデバッグに設定するには、AEM OSGi web コンソールで新しい Sling Logger 設定を作成します。
**ログノイズを減らすには、ステージング環境と実稼動環境で、このロガーを必ず削除または無効にしてください。**
* [configMgr](http://localhost:4502/system/console/configMgr) に移動します。

* 「Apache Sling Logging Logger Configuration」を検索して開きます。
* 次の設定でロガーを作成します。
   * **Log Level**：Debug
   * **Log File**：logs/saml.log
   * **Logger**：com.adobe.granite.auth.saml
* 「保存」をクリックして、設定を保存します。

#### OKTA 設定のテスト

AEM インスタンスからログアウトします。 リンクにアクセスしてみてください。 OKTA SSO が動作しているのがわかります。
