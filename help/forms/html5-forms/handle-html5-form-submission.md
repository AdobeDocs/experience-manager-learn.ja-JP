---
title: HTML5 フォーム送信の処理
description: HTML5 フォーム送信ハンドラーを作成します。
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 18%

---


# HTML5 フォームの送信を処理

HTML5 forms は、AEMでホストされているサーブレットに送信できます。 送信されたデータは、サーブレットで入力ストリームとしてアクセスできます。HTML5 フォームを送信するには、AEM Forms Designerを使用してフォームテンプレートに「HTTP 送信ボタン」を追加します。

## 送信ハンドラーの作成

シンプルなサーブレットでHTML5 フォーム送信を処理できます。 次のコードスニペットを使用して送信されたデータを抽出します。 このチュートリアルで提供される [ サーブレット ](assets/html5-submit-handler.zip) をダウンロードします。 [ パッケージマネージャー ](assets/html5-submit-handler.zip) を使用して [servlet](http://localhost:4502/crx/packmgr/index.jsp) をインストールします。

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

コードを使用して J2EE プロセスを呼び出す予定の場合は ](https://helpx.adobe.com/jp/aem-forms/6/submit-form-data-livecycle-process.html)[Adobe LiveCycle クライアントSDK設定が完了していることを確認してください。

## HTML5 フォームの送信 URL の設定

![ 送信 URL](assets/submit-url.PNG)

- xdp を開き、_プロパティ_->_詳細_ に移動します。
- http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.htmlをコピーし、「送信 URL」テキストフィールドに貼り付けます。
- 「_保存して閉じる_ ボタンをクリックします。

### 除外パスへのエントリの追加

- [configMgr](http://localhost:4502/system/console/configMgr) に移動します。
- _Adobe Granite CSRF フィルター_ を検索します。
- 「除外パス」セクションに、_/content/AemFormsSamples/handlehml5formsubmission_ のエントリを追加します。
- 変更を保存します。

### フォームのテスト

- xdp テンプレートを開きます。
- _プレビュー_/HTMLとしてプレビューをクリックします。
- フォームにデータを入力し、「送信」をクリックします。
- 送信されたデータについて、サーバーの stdout.log ファイルを確認します。

### 追加情報

HTML5 フォームの送信から PDF を生成する方法について詳しくは、この [ 記事 ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=ja) を参照してください。

