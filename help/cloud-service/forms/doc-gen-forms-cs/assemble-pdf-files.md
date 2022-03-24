---
title: アセンブリPDFファイル
description: invokeDDX 操作を使用して pdf ファイルを操作します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9958
thumbnail: 332439.jpg
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# DDXPDFを呼び出しエンドポイントを使用したエンドポイントファイルの操作


次の手順では、必要なパラメーターを使用して、エンドポイントに対する HTTPPOST呼び出しをおこないます。 テンプレートとデータファイルは、リソースファイルとして提供されます。 生成された PDF のプロパティは、リクエストのオプションのパラメーターで指定されます。embedFonts プロパティは、生成された PDF にカスタムフォントを埋め込むために使用されます。 後に続いてください [このドキュメント](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html) Formsクラウドインスタンスにカスタムフォントをデプロイする場合。 プロパティは options.json リソースファイルで指定します。 エンドポイントにはトークンベースの認証があるので、リクエストヘッダーでアクセストークンを渡します。

次のコードは、データをテンプレートと結合して pdf を生成する場合に使用しました

```java
public class DocumentGeneration
{
        public String SAVE_LOCATION = "c:\\aspire1";
        public void mergeDataWithXdpTemplate(String postURL)
        {
                HttpPost httpPost = new HttpPost(postURL);
                CredentialUtilites cu = new CredentialUtilites();
                String accessToken = cu.getAccessToken();
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
                URL templateFile = classLoader.getResource("templates/custom_fonts.xdp");
                File xdpTemplate = new File(templateFile.getPath());
                URL url = classLoader.getResource("datafiles");
                System.out.println(url.getPath());
                File files[] = new File(url.getPath()).listFiles();
                for (int i = 0; i < files.length; i++) {
                        MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                        ContentType strContent = ContentType.create("text/plain", Charset.forName("UTF-8"));
                        builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
                        builder.addBinaryBody("data", files[i]);
                        builder.addBinaryBody("template", xdpTemplate);
                        builder.addBinaryBody("options",GetOptions.getPDFOptions().getBytes(),ContentType.APPLICATION_JSON,"options"
                        try {
                                HttpEntity entity = builder.build();
                                httpPost.setEntity(entity);
                                CloseableHttpClient httpclient = HttpClients.createDefault();
                                CloseableHttpResponse response = httpclient.execute(httpPost);
                                InputStream generatedPDF = response.getEntity().getContent();
                                byte[] bytes = IOUtils.toByteArray(generatedPDF);
                                File saveLocation = new File(SAVE_LOCATION);
                                if (!saveLocation.exists()) {
                                        saveLocation.mkdirs();
                                }
                                File outputFile = new File(SAVE_LOCATION+File.separator+files[i].getName().replace("xml", "pdf"));
                                try (FileOutputStream outputStream = new FileOutputStream(outputFile)) {
                                        outputStream.write(bytes);
                                }
                        } catch (Exception e) {
                                System.out.println("The error is " + e.getMessage());
                        }

                }
                System.out.println("Done generating " + files.length + " files");

        }

}
```
