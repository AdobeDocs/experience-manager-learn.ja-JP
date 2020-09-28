---
title: AEMヘッドレス — 第7章 — モバイルアプリからのAEM Content Servicesの利用
description: チュートリアルの第7章では、AEM Content Servicesで作成したコンテンツをAndroidモバイルアプリで使用する方法を説明します。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '1404'
ht-degree: 1%

---


# 第7章 — モバイルアプリからのAEM Content Servicesの利用

チュートリアルの第7章では、ネイティブのAndroidモバイルアプリを使用して、AEM Content Servicesのコンテンツを参照します。

## Androidモバイルアプリ

このチュートリアルでは、AEM Content Servicesで公開されているイベントコンテンツを利用して表示する **ための** 簡単なネイティブAndroidモバイルアプリを使用します。

[](https://developer.android.com/) Androidの使用はほとんど重要ではありません。消費するモバイルアプリは、iOSなど、どのモバイルプラットフォーム用のフレームワークでも記述できます。

Androidは、Windows、macOS、LinuxでAndroidエミュレーターを実行する機能と、その人気、AEM開発者による理解が深いJavaとして記述できることから、チュートリアルに使用されます。

*チュートリアルのAndroidモバイルアプリは&#x200B;****、Androidモバイルアプリの作成方法やAndroid開発のベストプラクティスを伝える方法を説明する目的ではなく、AEM Content Servicesをモバイルアプリケーションで利用する方法を説明する目的で使用されています。*

### AEM Content ServicesがMobile Appのエクスペリエンスを引き起こす仕組み

![モバイルアプリとContent Servicesのマッピング](assets/chapter-7/content-services-mapping.png)

1. **ページの** 画像コンポーネントで定義されるロゴ [!DNL Events API]****。
1. **ページの** Textコンポーネントで定義される [!DNL Events API] タグ行 ****。
1. この **イベントリスト** は、イベントコンテンツフラグメントのシリアル化から派生し、設定済みの **コンテンツフラグメントリストコンポーネントを介して公開されます**。

## モバイルアプリのデモ

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### ローカルホスト以外で使用するためのモバイルアプリの設定

AEM Publishがhttp://localhost:4503 **で実行されていない場合は、AEM Publishのプロパティを指すように、ホストとポートがモバイルアプリ**[!DNL Settings] のホストとポートで更新されます。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## モバイルアプリをローカルで実行する

1. Android Studio [をダウンロードしてインストールし](https://developer.android.com/studio/install) 、Androidエミュレーターをインストールします。
1. **Android** ファイル [!DNL APK][GitHub/Assets/wknd-mobile.x.x.xapkをダウンロードします。](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Android **Studioを開く**
   * Android Studioの初回起動時に、Android Studioのインストールを求めるプロンプトが表示され [!DNL Android SDK] ます。 デフォルトを受け入れ、インストールを終了します。
1. Android Studioを開き、 **プロファイルまたはデバッグAPKを選択します**
1. 手順2でダウンロードしたAPKファイル(**wknd-mobile.x.x.apk**)を選択し、「 **OK」をクリックします**
   * 「 **新しいフォルダの**&#x200B;作成」または「既存のフォルダを **使用」の指示に従った場合は、**「既存を **使用」を選択します**。
1. Android Studioの初回起動時に、プロジェクトリストーの **wknd-mobile.x.x** を右クリックし、「モジュール設定を **開く**」を選択します。
   * 「 **モジュール/wknd-mobile.x.x.x/依存関係」タブを選択し**、「 **Android API 29プラットフォーム**」を選択します。 「OK」をタップして、変更を閉じ、保存します。
   * この操作を行わないと、エミュレーターを起動しようとすると、「Android SDKを選択してください」というエラーが表示されます。
1. **AVDマネージャーを開くには、** ツール/AVD Managerを選択するか **、上部バーの** AVD Manager **** アイコンをタップします。
1. デバイスを登録していない場合は、 **AVD Manager** ( **AVDマネージャ)ウィンドウで、「** +仮想デバイスを作成…」をクリックします。
   1. 左側で、「 **Phone** 」カテゴリを選択します。
   1. 「 **ピクセル2**」を選択します。
   1. Click the **Next** button.
   1. 「 **Q** with **API Level 29**」を選択します。
      * AVDマネージャーの初回起動時に、「Download the versioned API」というメッセージが表示されます。 「Q」リリースの横の「ダウンロード」リンクをクリックし、ダウンロードとインストールを完了します。
   1. Click the **Next** button.
   1. Click the **Finish** button.
1. [ **AVDマネージャ** ]ウィンドウを閉じます。
1. 上部のメニューバーで、 **実行/** 編集の設定 **ドロップダウンからwknd-mobile.x.x** を選択します。
1. 選択した設定の **実行** / **編集の横にある「実行」ボタンをタップします**
1. ポップアップで、新しく作成した **[!DNL Pixel 2 API 29]** 仮想デバイスを選択し、「 **OK」をタップします**
1. アプリケーションがすぐに読み込まれない場合は、エミュレーターのAndroidホーム画面で [!DNL WKND Mobile]**[!DNL WKND]** アイコンを探してタップします。
   * エミュレータが起動してもエミュレータの画面が黒のままの場合は、エミュレータのツールウィンドウのエミュレータウィンドウの横にある **電源** ボタンをタップします。
   * 仮想デバイス内をスクロールするには、クリック&amp;ホールドしてドラッグします。
   * AEMでコンテンツを更新するには、上部から「更新」アイコンが表示されるまでプルダウンし、アイコンを離します。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## モバイルアプリコード

この節では、AEM Content ServicesとJSON出力に最も相互に作用し、依存するAndroidモバイルアプリコードについて説明します。

読み込み時に、Mobile Appは、ど `HTTP GET` のエンドポイントに対してAEM Content Services `/content/wknd-mobile/en/api/events.model.json` を実行し、Mobile Appを駆動するコンテンツを提供するかを決定します。

イベントAPI(`/content/wknd-mobile/en/api/events.model.json`)の編集可能なテンプレートはロックされているので、Mobile Appはコード化して、JSON応答内の特定の場所で特定の情報を探すことができます。

### ハイレベルコードフロー

1. アプリを開くと、AEM Publish()へのリクエストが呼び出され、モバ [!DNL WKND Mobile] イルアプリのUIに入力 `HTTP GET``/content/wknd-mobile/en/api/events.model.json` するためのコンテンツが収集されます。
2. AEMからコンテンツを受け取ると、モバイルアプリの3つの表示要素、 **ロゴ、タグ行、イベントリスト**、のそれぞれがAEMのコンテンツで初期化されます。
   * AEMコンテンツをモバイルアプリの表示要素に連結するために、各AEMコンポーネントを表すJSONはJava POJOにマッピングされ、Android表示要素に連結されます。
      * 画像コンポーネントJSON→ロゴPOJO→ロゴImageView
      * テキストコンポーネントJSON→ TagLine POJO→ Text ImageView
      * コンテンツフラグメントリストJSON→イベントPOJO→イベントリサイクラービュー
   * *Mobile Appコードは、より大きなJSON応答内の既知の場所があるので、JSONをPOJOにマッピングできます。 「image」、「text」および「contentfragmentlist」のJSONキーは、バッキングしているAEMコンポーネントのノード名によって決まります。 これらのノード名が変更されると、必要なコンテンツをJSONデータからソースする方法がわからないので、モバイルアプリが中断します。*

#### AEM Content Servicesエンドポイントの呼び出し

以下は、モバイルアプリのコードの要約です。このコードは、AEM Content Servicesを呼び出して、Mobile Appエクスペリエンスを駆動するコンテンツを収集する役割を `MainActivity` 持ちます。

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` はモバイルアプリの初期化フックで、JSONを解析し、値を `ViewBinders``View` 要素にバインドする3つのカスタムを登録します。

`initApp(...)` が呼び出され、AEM Publish上のAEM Content Servicesエンドポイントに対してHTTPGETリクエストが行われ、コンテンツが収集されます。 有効なJSON応答を受け取ると、JSON応答が各JSONに渡され、JSONの解析とモバイル `ViewBinder``View` 要素へのバインドを担当します。

#### JSON応答の解析

次に、簡単で重要な考慮事項をいくつ `LogoViewBinder`か示します。

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

の最初の行は、JSON応答のキー `bind(...)` :items→ルート→ :items **** (コンポーネントが追加されたAEMレイアウトコンテナを表す)を介して下に移動します。

ここから、 **image**&#x200B;という名前のキー（Imageコンポーネントを表す）に対してチェックが行われます（再び、このノード名→ JSONキーは安定していることが重要です）。 このオブジェクトが存在する場合、Jackson [ライブラリを介して読み取り、カスタム画像POJO](#image-pojo)`ObjectMapper` にマッピングされます。 画像POJOについては、以下に説明します。

最後に、ロゴのロゴ `src` は、 [!DNL Glide] ヘルパーライブラリを使用してAndroid ImageViewに読み込まれます。

AEM Content ServicesはJCRパス(すなわち、 `aemHost``/content/dam/wknd-mobile/images/wknd-logo.png`)を参照先コンテンツに追加します。

#### ザイメージポジョ{#image-pojo}

オプションですが、 [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) (またはGsonなどの他のライブラリで提供される同様の機能を使用すると、複雑なJSON構造をJava POJOにマッピングするのに役立ちます。ネイティブのJSONオブジェクト自体を直接処理する必要はありません。 この単純なケースでは、 `src` JSONオブジェクトの `image` キーを、注釈を介して直接画像POJOの `src``@JSONProperty` 属性にマップします。

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

イベントPOJOは、JSONオブジェクトから多くのデータポイントを選択する必要がありますが、この方法の利点は、単純なImageよりも多くあります。この方法の利点は、単にを選択するだけで `src`す。

## モバイルアプリのエクスペリエンスの調査

これで、AEM Content Servicesがどのようにネイティブのモバイルエクスペリエンスを引き起こすかを理解できたので、次の手順の実行方法を学習し、変更内容をモバイルアプリに反映させて確認できます。

各手順の後、モバイルアプリを引っ張って更新し、モバイルエクスペリエンスの更新を確認します。

1. **新しい[!DNL Event]コンテンツフラグメントの作成と発行**
1. **既存の[!DNL Event]コンテンツフラグメントの非公開**
1. 更新を **タグ行に発行**

## おめでとう

**AEM Headless Tutorialは終了です。**

ヘッドレスCMSとしてのAEM Content ServicesとAEMについて詳しくは、Adobeの他のドキュメントおよび有効化に関する資料を参照してください。

* [コンテンツフラグメントの使用](../sites/content-fragments/understand-content-fragments-and-experience-fragments.md)
* [AEM WCMコアコンポーネントユーザーガイド](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンポーネントコンポーネントライブラリ](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCMコアコンポーネントGitHubプロジェクト](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCMコアコンポーネント — エキスパートにお問い合わせ](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [コンポーネントエクスポーターのコードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
