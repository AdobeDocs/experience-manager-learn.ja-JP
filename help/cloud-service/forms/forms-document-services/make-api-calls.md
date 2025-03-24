---
title: 使用権限 API の使用
description: 提供された PDF に使用権限を適用するサンプルコード
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a4e2132b-3cfd-4377-8998-6944365edec5
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 100%

---

# API 呼び出しの実行

## 使用権限の適用

アクセストークンを取得したら、次に、指定された PDF に使用権限を適用する API リクエストを実行します。それには、呼び出しを認証するためのアクセストークンをリクエストヘッダーに含めて、ドキュメントの安全で承認された処理が確実に行われるようにする必要があります。

次の関数は使用権限を適用します

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## 関数の分類：



* **API エンドポイントおよびペイロードの設定**
   * 指定された `endPoint` と定義済みの `BUCKET` を使用して API URL を作成します。
   * 適用する権限を指定する JSON 文字列（`usageRights`）を定義します。例えば、次のような権限です。
      * コメント
      * 埋め込みファイル
      * フォーム入力
      * フォームデータの書き出し

* **PDF ファイルの読み込み**
   * `pdffiles` ディレクトリから `withoutusagerights.pdf` ファイルを取得します。
   * ファイルが見つからない場合、エラーをログに記録して終了します。

* **HTTP リクエストの準備**
   * PDF ファイルを読み取ってバイト配列に格納します。
   * `MultipartEntityBuilder` を使用して、以下を含んだマルチパートリクエストを作成します。
      * PDF ファイル（バイナリ本文）
      * `usageRights` JSON（テキスト本文）
   * 次のヘッダーを含んだ HTTP `POST` リクエストを設定します。
      * `Authorization: Bearer <accessToken>`（認証用）
      * `X-Adobe-Accept-Experimental: 1`（API の互換性のために必要になる可能性があります）

* **リクエストの送信と応答の処理**
   * `httpClient.execute(httpPost)` を使用して HTTP リクエストを実行します。
   * 応答（使用権限が適用された更新済みの PDF となる見込み）を読み取ります。
   * 受け取った PDF コンテンツを `SAVE_LOCATION` の **ReaderExtended.pdf** に書き込みます。

* **エラーの処理とクリーンアップ**
   * `IOException` エラーを取得してログに記録します。
   * すべてのリソース（ストリーム、HTTP クライアント、応答）が必ず `finally` ブロックで適切に閉じられるようにします。

applyUsageRights 関数を呼び出す main.java コードを次に示します。

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

`main` メソッドは、`AccessTokenService` から `getAccessToken()` を呼び出すことにより初期化を行います。この呼び出しで有効なトークンが返されるはずです。

* その後、`DocumentGeneration` クラスの `applyUsageRights()` を呼び出して、次のものを渡します。
   * 取得した `accessToken`
   * 使用権限を適用するための API エンドポイント


## 次の手順

[サンプルプロジェクトのデプロイ](sample-project.md)
