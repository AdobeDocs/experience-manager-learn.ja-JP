---
title: フォームデータモデルを使用したキャンペーンプロファイルの作成
description: AEM Formsフォームデータモデルを使用したAdobe Campaign Standardプロファイルの作成に関する手順
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 3%

---

# フォームデータモデルを使用したキャンペーンプロファイルの作成 {#create-campaign-profile-using-form-data-model}

AEM Formsフォームデータモデルを使用したAdobe Campaign Standardプロファイルの作成に関する手順

## カスタム認証の作成 {#create-custom-authentication}

Swagger ファイルを使用してデータソースを作成する場合、AEM Formsは次の種類の認証をサポートします

* なし
* OAuth 2.0
* 基本認証
* API キー
* カスタム認証

![campaignfdm](assets/campaignfdm.gif)

Adobe Campaign Standardへの REST 呼び出しをおこなうには、カスタム認証を使用する必要があります。

カスタム認証を使用するには、IAuthentication インターフェイスを実装する OSGi コンポーネントを開発する必要があります

メソッド getAuthDetails を実装する必要があります。 このメソッドは AuthenticationDetails オブジェクトを返します。 この AuthenticationDetails オブジェクトには、Adobe Campaignへの REST API 呼び出しを行うために必要な必須 HTTP ヘッダーが設定されます。

次に、カスタム認証の作成で使用したコードを示します。 getAuthDetails メソッドはすべての動作を行います。 AuthenticationDetails オブジェクトを作成します。 次に、このオブジェクトに適切な HttpHeaders を追加し、このオブジェクトを返します。

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

## データソースを作成 {#create-data-source}

最初の手順は、Swagger ファイルを作成することです。 Swagger ファイルは、Adobe Campaign Standardでのプロファイルの作成に使用される REST API を定義します。 Swagger ファイルは、REST API の入力パラメーターと出力パラメーターを定義します。

データソースは Swagger ファイルを使用して作成されます。 データソースを作成する際に、認証タイプを指定できます。 この場合、Adobe Campaignに対する認証にカスタム認証を使用します。上記のコードは、Adobe Campaignに対する認証に使用されていました。

サンプルの Swagger ファイルは、この記事に関連するアセットの一部として提供されています。**Swagger ファイルの host と basePath を、ACS インスタンスに合わせて必ず変更してください。**

## ソリューションをテストする {#test-the-solution}

ソリューションをテストするには、次の手順に従います。
* [ここで説明する手順に従っていることを確認します。](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [このファイルをダウンロードして展開し、Swagger ファイルを取得します](assets/create-acs-profile-swagger-file.zip)
* Swagger ファイルを使用してデータソースを作成するフォームデータモデルを作成し、前の手順で作成したデータソースをベースにします。
* 前の手順で作成したフォームデータモデルに基づいてアダプティブフォームを作成します。
* 次の要素を「データソース」タブからアダプティブフォームにドラッグ&amp;ドロップします

   * 電子メール
   * 名前（名）
   * 名前（姓）
   * 携帯電話

* 送信アクションを「フォームデータモデルを使用して送信」に設定します。
* 適切に送信するようにデータモデルを設定します。
* フォームをプレビューする. フィールドに入力して送信します。
* プロファイルがAdobe Campaign Standardで作成されたことを確認します。
