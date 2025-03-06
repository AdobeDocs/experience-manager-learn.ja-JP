---
title: Usagerights API の使用
description: 提供されたPDFに使用権限を適用するサンプルコード
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 3%

---

# API 呼び出しを行う

## 使用権限の適用

アクセストークンを取得したら、次に、指定したPDFに使用権限を適用する API リクエストを実行します。 これには、呼び出しを認証するためのリクエストヘッダーにアクセストークンを含め、ドキュメントの安全で承認された処理を確保することが含まれます。

次の関数は、使用権限を適用します

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



* **API エンドポイントとペイロードの設定**
   * 指定された `endPoint` と事前定義済みの `BUCKET` を使用して API URL を作成します。
   * 適用する権限を指定する JSON 文字列（`usageRights`）を定義します。例えば、次のようなものです。
      * コメント
      * 埋め込みファイル
      * フォームの入力
      * フォームデータのエクスポート

* **PDF ファイルを読み込む**
   * `pdffiles` ディレクトリから `withoutusagerights.pdf` ファイルを取得します。
   * ファイルが見つからない場合、エラーをログに記録して終了します。

* **HTTP リクエストの準備**
   * PDF ファイルをバイト配列に読み取ります。
   * `MultipartEntityBuilder` を使用して、以下を含むマルチパートリクエストを作成します。
      * PDF ファイルをバイナリ本文として指定します。
      * テキスト本文として使用する `usageRights` JSON。
   * ヘッダー付きの HTTP `POST` リクエストを設定します。
      * 認証用に `Authorization: Bearer <accessToken>` します。
      * `X-Adobe-Accept-Experimental: 1` （API の互換性のために必要になる場合があります）。

* **リクエストの送信と応答の処理**
   * `httpClient.execute(httpPost)` を使用して HTTP リクエストを実行します。
   * 応答を読み取ります（使用権限が適用された、更新されたPDFであると想定）。
   * 受け取ったPDF コンテンツを `SAVE_LOCATION` の **&quot;ReaderExtended.pdf&quot;** に書き込みます。

* **エラーの処理とクリーンアップ**
   * `IOException` のエラーをキャッチしてログに記録します。
   * すべてのリソース（ストリーム、HTTP クライアント、応答）が `finally` ブロックで適切に閉じられることを確認します。

applyUsageRights 関数を呼び出す main.java コードを次に示します

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

`main` メソッドは、`AccessTokenService` から `getAccessToken()` を呼び出すことにより初期化します。このメソッドは有効なトークンを返すと想定されます。

* その後、`DocumentGeneration` クラスから `applyUsageRights()` を呼び出して、次を渡します。
   * 取得した `accessToken`
   * 使用権限を適用するための API エンドポイント。


## 次の手順

[サンプルプロジェクトのデプロイ](sample-project.md)
