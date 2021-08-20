---
title: フォームデータモデルを使用したキャンペーンプロファイルの作成
description: AEM Formsフォームデータモデルを使用したAdobe Campaign Standardプロファイルの作成に関する手順
feature: アダプティブフォーム
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 4%

---


# フォームデータモデルを使用したキャンペーンプロファイルの作成 {#create-campaign-profile-using-form-data-model}

AEM Formsフォームデータモデルを使用したAdobe Campaign Standardプロファイルの作成に関する手順

## カスタム認証の作成 {#create-custom-authentication}

Swaggerファイルを使用してデータソースを作成する場合、AEM Formsは次の種類の認証をサポートします

* なし
* OAuth 2.0
* 基本認証
* API キー
* カスタム認証

![campaignfdm](assets/campaignfdm.gif)

Adobe Campaign StandardへのREST呼び出しをおこなうには、カスタム認証を使用する必要があります。

カスタム認証を使用するには、IAuthenticationインターフェイスを実装するOSGiコンポーネントを開発する必要があります

getAuthDetailsメソッドを実装する必要があります。 このメソッドは、 AuthenticationDetailsオブジェクトを返します。 このAuthenticationDetailsオブジェクトには、Adobe CampaignへのREST API呼び出しをおこなうために必要なHTTPヘッダーが設定されます。

次に、カスタム認証の作成に使用したコードを示します。 getAuthDetailsメソッドは、すべての処理を実行します。 AuthenticationDetailsオブジェクトを作成します。 次に、適切なHttpHeadersをこのオブジェクトに追加し、このオブジェクトを返します。

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## データソースの作成 {#create-data-source}

最初の手順は、Swaggerファイルを作成することです。 Swaggerファイルは、Adobe Campaign Standardでのプロファイルの作成に使用されるREST APIを定義します。 Swaggerファイルは、REST APIの入力パラメーターと出力パラメーターを定義します。

データソースはSwaggerファイルを使用して作成されます。 データソースの作成時に、認証タイプを指定できます。 この場合、Adobe Campaignに対する認証にカスタム認証を使用します。上記のコードは、Adobe Campaignに対する認証に使用されました。

この記事に関連するアセットの一部として、サンプルのSwaggerファイルが提供されます。**ACSインスタンスに合わせて、SwaggerファイルのhostとbasePathを必ず変更してください。**

## ソリューションのテスト {#test-the-solution}

ソリューションをテストするには、次の手順に従います。
* [ここで説明する手順に従っていることを確認します。](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [このファイルをダウンロードして解凍し、Swaggerファイルを取得します](assets/create-acs-profile-swagger-file.zip)
* Swaggerファイルを使用したデータソースの作成
フォームデータモデルを作成し、前の手順で作成したデータソースをベースにします。
* 前の手順で作成したフォームデータモデルに基づいてアダプティブフォームを作成します。
* 次の要素を「データソース」タブからアダプティブフォームにドラッグ&amp;ドロップします

   * 電子メール
   * firstName
   * 姓
   * 携帯電話

* 送信アクションを「フォームデータモデルを使用して送信」に設定します。
* 適切に送信するようにデータモデルを設定します。
* フォームをプレビューする. フィールドに入力し、を送信します。
* プロファイルがAdobe Campaign Standardで作成されたことを確認します。
