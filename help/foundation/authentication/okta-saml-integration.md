---
title: AEMでの OKTA の設定
description: OKTA を使用したシングルサインオンの様々な設定を理解します。
version: 6.5
topic: Integrations, Security, Administration
feature: Integrations
role: Admin
level: Experienced
jira: KT-12305
last-substantial-update: 2023-03-01T00:00:00Z
source-git-commit: de9377236016066cc62819f1c307aac82331a0b6
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 3%

---

# OKTA を使用して AEM オーサーに対して認証します。

> 詳しくは、 [SAML 2.0 認証](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/authentication/saml-2-0.html?lang=ja) AEM as a Cloud Serviceでの OKTA の設定方法を参照してください。

最初の手順は、OKTA ポータルでアプリを設定することです。 OKTA 管理者がアプリを承認すると、IdP 証明書とシングルサインオン URL にアクセスできるようになります。 以下は、新しいアプリケーションの登録で一般的に使用される設定です。

* **アプリ名：** これはお使いのアプリケーション名です。 アプリケーションに一意の名前を付けてください。
* **SAML 受信者：** OKTA からの認証後、これは SAML 応答でAEMインスタンスにヒットされる URL です。 SAML 認証ハンドラーは通常、/ saml_login を使用してすべての URL を傍受しますが、アプリケーションのルートの後に追加する方がよいでしょう。
* **SAML オーディエンス**:これは、アプリケーションのドメイン URL です。 ドメイン URL にはプロトコル（http または https）を使用しないでください。
* **SAML 名 ID:** ドロップダウンリストから「電子メール」を選択します。
* **環境**:適切な環境を選択します。
* **属性**:SAML 応答でユーザーに関して取得する属性です。 必要に応じて指定します。


![okta アプリケーション](assets/okta-app-settings-blurred.PNG)


## OKTA(IdP) 証明書をAEM Trust Store に追加します。

SAML アサーションは暗号化されるので、AEMとAEM間の安全な通信を可能にするために、IdP(OKTA) 証明書を OKTA Trust Store に追加する必要があります。
[Trust Store を初期化](http://localhost:4502/libs/granite/security/content/truststore.html)（まだ初期化されていない場合）
トラストストアのパスワードを記憶します。 このプロセスの後半で、このパスワードを使用する必要があります。

* に移動します。 [グローバルトラストストア](http://localhost:4502/libs/granite/security/content/truststore.html).
* 「CER ファイルから証明書を追加」をクリックします。 OKTA から提供された IdP 証明書を追加し、「送信」をクリックします。

   >[!NOTE]
   >
   >証明書をどのユーザーにもマップしないでください

証明書を Trust Store に追加すると、以下のスクリーンショットに示すように、証明書エイリアスが取得されます。 別名の名前は、別の名前にする必要があります。

![証明書エイリアス](assets/cert-alias.PNG)

**証明書エイリアスをメモします。 これは、後の手順で必要になります。**

### SAML 認証ハンドラーの設定

に移動します。 [configMgr](http://localhost:4502/system/console/configMgr).
「AdobeGranite SAML 2.0 認証ハンドラー」を検索して開きます。
以下に指定したように、次のプロパティを指定します。次に、指定する必要がある主要なプロパティを示します。

* **パス**  — 認証ハンドラーがトリガーされるパス
* **IdP Url**:OKTA から提供される IdP URL です
* **IDP 証明書エイリアス**:IdP 証明書をAEM Trust Store に追加した際に取得したエイリアス
* **サービスプロバイダーエンティティ ID**:AEM Server の名前
* **キーストアのパスワード**：使用した Trust Store のパスワードです
* **デフォルトのリダイレクト**：認証に成功した場合にリダイレクト先の URL
* **ユーザー ID 属性**:uid
* **暗号化を使用**:false
* **CRX ユーザーを自動作成**:true
* **グループに追加**:true
* **デフォルトのグループ**:oktausers( これは、ユーザーが追加されるグループです。 AEM内の任意の既存のグループを指定できます )
* **NamedIDPolicy**:要求された件名を表すために使用する名前識別子に対する制約を指定します。 次のハイライト表示された文字列をコピーして貼り付けます。 **骨:oasis:名前:tc:SAML:2.0:nameidformat:emailAddress**
* **同期済み属性**  — これらは、AEMプロファイルの SAML アサーションから保存される属性です

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling Referrer Filter の設定

に移動します。 [configMgr](http://localhost:4502/system/console/configMgr).
「Apache Sling Referrer Filter」を検索して開きます。次のプロパティを以下に指定します。

* **空を許可**:false
* **ホストを許可**:IdP のホスト名（これは、ケースで異なります）
* **正規表現ホストを許可**:IdP のホスト名（実際のケースでは異なります） Sling Referrer Filter リファラープロパティのスクリーンショット

![referrer-filter](assets/okta-referrer.png)

#### OKTA 統合用の DEBUG Logging の設定

AEMで OKTA 統合を設定する場合、AEM SAML 認証ハンドラーのデバッグログを確認すると役立つ場合があります。 ログレベルを DEBUG に設定するには、AEM OSGi Web コンソールで新しい Sling Logger 設定を作成します。

ログノイズを減らすには、必ずステージング環境と実稼動環境でこのロガーを削除または無効にしてください。

AEMで OKTA 統合を設定する場合、AEM SAML 認証ハンドラーのデバッグログを確認すると役に立つことがあります。 ログレベルを DEBUG に設定するには、AEM OSGi Web コンソールで新しい Sling Logger 設定を作成します。
**ログノイズを減らすには、必ずステージング環境と実稼動環境でこのロガーを削除または無効にしてください。**
* に移動します。 [configMgr](http://localhost:4502/system/console/configMgr)

* 「Apache Sling Logging Logger Configuration」を検索して開きます。
* 次の設定でロガーを作成します。
   * **ログレベル**:デバッグ
   * **ログファイル**:logs/saml.log
   * **ロガー**:com.adobe.granite.auth.saml
* 「保存」をクリックして設定を保存します。

#### OKTA 設定をテストする

AEMインスタンスからログアウトします。 リンクにアクセスしてみてください。 OKTA SSO が動作しているのがわかります。
