---
title: フォームデータモデルを使用した Campaign プロファイルの作成
description: AEM Forms のフォームデータモデルを使用した Adobe Campaign Standard プロファイルの作成に関する手順
feature: Adaptive Forms
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 110
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '431'
ht-degree: 100%

---

# フォームデータモデルを使用した Campaign プロファイルの作成 {#create-campaign-profile-using-form-data-model}

AEM Forms のフォームデータモデルを使用した Adobe Campaign Standard プロファイルの作成に関する手順

## カスタム認証の作成 {#create-custom-authentication}

Swagger ファイルを使用してデータソースを作成する場合、AEM Forms は次の種類の認証タイプをサポートします

* なし
* OAuth 2.0
* 基本認証
* API キー
* カスタム認証

![campaignfdm](assets/campaignfdm.gif)

Adobe Campaign Standard への REST 呼び出しを行うには、カスタム認証を使用する必要があります。

カスタム認証を使用するには、IAuthentication インターフェイスを実装する OSGi コンポーネントを開発する必要があります

メソッド getAuthDetails を実装する必要があります。このメソッドは AuthenticationDetails オブジェクトを返します。この AuthenticationDetails オブジェクトには、Adobe Campaign への REST API 呼び出しを行うために必要な、必須 HTTP ヘッダーが設定されます。

次に、カスタム認証の作成で使用したコードを示します。getAuthDetails メソッドがすべての作業を行います。AuthenticationDetails オブジェクトを作成します。次に、このオブジェクトに適切な HttpHeaders を追加し、このオブジェクトを返します。

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

最初の手順は、Swagger ファイルを作成することです。Swagger ファイルは、Adobe Campaign Standard でのプロファイルの作成に使用される REST API を定義します。Swagger ファイルは、REST API の入力パラメーターと出力パラメーターを定義します。

データソースは、Swagger ファイルを使用して作成されます。データソースを作成する際に、認証タイプを指定できます。この場合、Adobe Campaign に対する認証にカスタム認証を使用します。上記のコードは、Adobe Campaign に対する認証に使用されていました。

サンプルの Swagger ファイルは、この記事の関連アセットの一部として提供されています。**Swagger ファイルの host と basePath を、自身の ACS インスタンスに合わせて必ず変更してください。**

## ソリューションのテスト {#test-the-solution}

ソリューションをテストするには、次の手順に従います。
* [ここで説明する手順に従っていることを確認します](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [このファイルをダウンロードして展開し、Swagger ファイルを取得します](assets/create-acs-profile-swagger-file.zip)
* Swagger ファイルを使用してデータソースを作成します
フォームデータモデルを作成し、前の手順で作成したデータソースをベースにします
* 以前の手順で作成したフォームデータモデルに基づいてアダプティブフォームを作成します。
* 次の要素を「データソース」タブからドラッグし、アダプティブフォームにドロップします

   * メール
   * 名前（名）
   * 名前（姓）
   * 携帯電話

* 送信アクションを「フォームデータモデルを使用して送信」に設定します。
* 適切に送信するようにデータモデルを設定します。
* フォームをプレビューします。フィールドに入力して送信します。
* プロファイルが Adobe Campaign Standard で作成されたことを確認します。
