---
title: コアコンポーネントベースのフォームの送信を処理するためのカスタム送信を作成
description: 添付ファイル付きのフォームデータを Azure に保存するためのカスタム送信を作成
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: 6.5
topic: Integrations
jira: KT-14794
exl-id: c4257567-76bd-417e-a1a2-38a0647767d9
duration: 147
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '187'
ht-degree: 100%

---

# フォームの送信を処理するためのカスタム送信を作成

このユースケースを満たすために、送信されたデータと添付ファイルを Azure に保存するカスタム送信サービスが作成されました。コアコンポーネントベースのフォームが送信されるとき、データは次の形式になります。

```json
{
    "Name": "Gloria Rios",
    "EmailID": "grios@email.com",
    "ExistingCustomer": [
        "Yes"
    ],
    "PhoneNumber": "1234567890",
    "Solution": "Forms",
    "contractcopy": {
        "name": "annualcontract.pdf",
        "size": 80194,
        "mediaType": "application/pdf",
        "data": "[object File]"
    },
    "Message": "We would like to renew our annual contract "
}
```

_**contractcopy**_ 要素は添付ファイルコンポーネントを表し、フォームと共に送信された添付ファイルをキャプチャするために使用されます。
アダプティブフォームにデータとその添付ファイルを事前入力できるように、送信された添付ファイルが Azure ポータルに保存され、送信されたデータ内の contractcopy オブジェクトのデータ要素が、保存された添付ファイルの URL で更新されます。
カスタム送信サービスは、添付ファイルを抽出して Azure Portal に保存します。更新された送信済みデータは次のようになります。


```json
{
    "Name": "Gloria Rios",
    "EmailID": "grios@email.com",
    "ExistingCustomer": [
        "Yes"
    ],
    "PhoneNumber": "1234567890",
    "Solution": "Forms",
    "contractcopy": {
        "name": "annualcontract.pdf",
        "size": 80194,
        "mediaType": "application/pdf",
        "data": "https://aemformstutorial.blob.core.windows.net/benefiitsenrollment/69d5a2?sv=2022-11-02&ss=bfqt&srt=KiHs0%3D"
    },
    "Message": "We would like to renew our annual contract "
}
``
```


[コアコンポーネントベースのアダプティブフォーム用のサンプルのカスタム送信ハンドラーはこちらから利用できます](https://github.com/adobe/aem-core-forms-components/blob/master/it/core/src/main/java/com/adobe/cq/forms/core/components/it/service/CustomAFSubmitService.java#L56)。フォーム送信を処理するために、次のカスタム送信が記述されました。

```java
package com.azuredemo.core;
import com.adobe.aemds.guide.common.GuideValidationResult;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.adobe.forms.common.service.FileAttachmentWrapper;
import com.azuredemo.core.SaveAndFetchFromAzure.StoreAndFetchDataFromAzureStorage;
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

@Component(
    service = FormSubmitActionService.class,
    immediate = true
)
public class StoreFormDataWithAttachments implements FormSubmitActionService {
    private static final String serviceName = "Store Submitted Form Data in Azure";
    private static transient Logger logger = LoggerFactory.getLogger(StoreFormDataWithAttachments.class);

    @Reference
    DataManager dataManager;
    @Reference
    StoreAndFetchDataFromAzureStorage storeAndFetchDataFromAzureStorage;

    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map < String, Object > submit(FormSubmitInfo formSubmitInfo) {
        logger.debug("@@@@in my custom submit!!!");
        Map < String, Object > result = new HashMap < > ();
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.FALSE);
        try {

            String guideContainerPath = formSubmitInfo.getFormContainerPath();

            String submittedFormName = formSubmitInfo.getFormContainerResource().getParent().getParent().getName();
            logger.debug("The submitted form name is " + submittedFormName);
            String data = formSubmitInfo.getData();
            logger.debug("The submitted data is " + data);
            JsonObject jsonData = new Gson().fromJson(data, JsonObject.class);
            if (formSubmitInfo.getFileAttachments() != null && formSubmitInfo.getFileAttachments().size() > 0) {
                logger.debug("#####The form submitted has file attachments");
                for (int i = 0; i < formSubmitInfo.getFileAttachments().size(); i++) {
                    FileAttachmentWrapper fileAttachment = formSubmitInfo.getFileAttachments().get(i);

                    logger.debug("The url of the file attachment is " + fileAttachment.getUri());
                    logger.debug("The data ref of the file attachment is " + fileAttachment.getDataRef());
                    String fileName = fileAttachment.getFileName();
                    logger.debug("The fileName is  " + fileName);
                    logger.debug("#### Writing  " + fileName + " to azure");


                    String dataUrl = storeAndFetchDataFromAzureStorage.saveFormAttachmentinAzure(fileAttachment.getInputStream(), fileName, fileAttachment.getContentType());
                    jsonData.getAsJsonObject(fileAttachment.getDataRef()).addProperty("data", dataUrl);
                    logger.debug(jsonData.toString());

                }
            }


            String uniqueID = UUID.randomUUID().toString();

            logger.debug(storeAndFetchDataFromAzureStorage.saveFormDatainAzure(jsonData.toString(), "text/plain"));

            if (dataManager != null) {
                dataManager.put(uniqueID, data);

            }
            logger.info("AF Submission successful using custom submit service for: {}", guideContainerPath);
            result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
            result.put(DataManager.UNIQUE_ID, uniqueID);
            // adding id here so that this available in redirect parameters in final thank you page
            Map < String, Object > redirectParamMap = new HashMap < String, Object > () {
                {
                    put(DataManager.UNIQUE_ID, uniqueID);
                }
            };
            
            result.put("fd:redirectParameters", redirectParamMap);
        } catch (Exception ex) {
            logger.error("Error while using the AF Submit service", ex);
            GuideValidationResult guideValidationResult = new GuideValidationResult();
            guideValidationResult.setOriginCode("500");
            guideValidationResult.setErrorMessage("Internal server error");
            result.put(GuideConstants.FORM_SUBMISSION_ERROR, guideValidationResult);
        }
        return result;
    }

}
```

## Azure にフォームの添付ファイルを保存

```java
@Override
// Save the submitted form attachment in azure and return its url
public String saveFormAttachmentinAzure(InputStream attachmentStream, String fileName, String contentType) {
    logger.debug("Saving " + fileName + "  form attachment in azure  ");
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    logger.debug("The SAS Token is " + sasToken);
    logger.debug("The Storage URL is " + storageURI);

    int timeout = 5;
    RequestConfig config = RequestConfig.custom()
        .setConnectTimeout(timeout * 1000)
        .setConnectionRequestTimeout(timeout)
        .setSocketTimeout(timeout * 1000).build();
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().setDefaultRequestConfig(config).build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    System.out.println("The put Request " + putRequestURL);
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");

    try {

        InputStreamEntity ies = new InputStreamEntity(attachmentStream, attachmentStream.available());

        httpPut.setEntity(ies);

        CloseableHttpResponse response = httpClient.execute(httpPut);
        logger.debug("Response code    " + response.getStatusLine().getStatusCode());
        logger.debug("Added file attachment  " + response.getStatusLine().getStatusCode() + "  uuid " + uuid.toString() + "   " + response.getStatusLine().getReasonPhrase());
        if (response.getStatusLine().getStatusCode() == 201) {
            return putRequestURL;
        }
    } catch (Exception e) {

        logger.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;
}
```

## Azure にフォームデータを保存

```java
@Override
 public String saveFormDatainAzure(String formData, String contentType) {
     String sasToken = azurePortalConfigurationService.getSASToken();
     String storageURI = azurePortalConfigurationService.getStorageURI();
     logger.debug("The SAS Token is " + sasToken);
     logger.debug("The Storage URL is " + storageURI);
     logger.debug("The form data is " + formData);
     int timeout = 5;
     RequestConfig config = RequestConfig.custom()
         .setConnectTimeout(timeout * 1000)
         .setConnectionRequestTimeout(timeout)
         .setSocketTimeout(timeout * 1000).build();
     org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().setDefaultRequestConfig(config).build();
     UUID uuid = UUID.randomUUID();
     String putRequestURL = storageURI + uuid.toString();
     putRequestURL = putRequestURL + sasToken;
     logger.debug("The put Request " + putRequestURL);
     HttpPut httpPut = new HttpPut(putRequestURL);
     httpPut.addHeader("x-ms-blob-type", "BlockBlob");
     httpPut.addHeader("Content-Type", contentType);
     try {
         httpPut.setEntity(new StringEntity(formData));
         CloseableHttpResponse response = httpClient.execute(httpPut);
         logger.debug("Response code " + response.getStatusLine().getStatusCode());
         System.out.println("The response code is " + response.getStatusLine().getStatusCode() + "uuid " + uuid.toString());
         if (response.getStatusLine().getStatusCode() == 201) {
             return uuid.toString();
         }
     } catch (IOException e) {
         logger.error("Error: " + e.getMessage());
         throw new RuntimeException(e);
     }
     return null;

 }
```

## 次の手順

[OSGi 設定を作成](./create-osgi-configuration.md)
