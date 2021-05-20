---
title: 第7章 — モバイルアプリからのAEM Content Servicesの使用 — Content Services
description: チュートリアルの第7章では、AEM Content Servicesから作成したコンテンツを使用するためにAndroidモバイルアプリを実行します。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1414'
ht-degree: 1%

---


# 第7章 — モバイルアプリからのAEM Content Servicesの使用

チュートリアルの第7章では、ネイティブのAndroidモバイルアプリを使用して、AEM Content Servicesのコンテンツを使用します。

## Androidモバイルアプリ

このチュートリアルでは、**シンプルなネイティブAndroidモバイルアプリ**&#x200B;を使用して、AEM Content Servicesによって公開されたイベントコンテンツを使用および表示します。

[Android](https://developer.android.com/)の使用はほとんど重要ではありません。消費するモバイルアプリは、iOSなど、あらゆるモバイルプラットフォーム向けのあらゆるフレームワークで記述できます。

Androidは、Windows、macOSおよびLinux上でAndroidエミュレーターを実行する機能と、その人気、およびAEM開発者がよく理解できる言語であるJavaとして記述できるので、チュートリアルに使用されます。

*このチュートリアルのAndroidモバイルアプリは、Androidモバイルアプリの作成方法やAndroid開発のベストプラクティスを伝える方法を示すものではなく、モバイルアプリケーションからAEM Content Servicesを利用する方法を示すものです。*****

### AEM Content Servicesがモバイルアプリのエクスペリエンスを促進する仕組み

![モバイルアプリとコンテンツサービスのマッピング](assets/chapter-7/content-services-mapping.png)

1. [!DNL Events API]ページの&#x200B;**画像コンポーネント**&#x200B;で定義される&#x200B;**logo**。
1. [!DNL Events API]ページの&#x200B;**テキストコンポーネント**&#x200B;で定義された&#x200B;**タグ行**。
1. この&#x200B;**イベントリスト**&#x200B;は、設定済みの&#x200B;**コンテンツフラグメントリストコンポーネント**&#x200B;で公開される、イベントコンテンツフラグメントのシリアル化から派生します。

## モバイルアプリのデモ

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### ローカルホスト以外で使用するためのモバイルアプリの設定

AEMパブリッシュが&#x200B;**http://localhost:4503**&#x200B;で実行されていない場合、ホストとポートをモバイルアプリの[!DNL Settings]で更新して、AEMパブリッシュホスト/ポートのプロパティを指すことができます。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## モバイルアプリのローカルでの実行

1. [Android Studio](https://developer.android.com/studio/install)をダウンロードしてインストールし、Androidエミュレーターをインストールします。
1. **** Androidファイル [!DNL APK] の [ダウンロードGitHub /アセット/ wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. **Android Studio**&#x200B;を開きます。
   * Android Studioの初回起動時に、[!DNL Android SDK]をインストールするよう求めるプロンプトが表示されます。 デフォルトを受け入れ、インストールを終了します。
1. Android Studioを開き、「**Profile」または「Debug APK**」を選択します。
1. 手順2でダウンロードしたAPKファイル(**wknd-mobile.x.x.x.apk**)を選択し、「**OK**」をクリックします。
   * 「**新しいフォルダーを作成**」または「**既存の**&#x200B;を使用」というプロンプトが表示されたら、「**既存の**&#x200B;を使用」を選択します。
1. Android Studioの初回起動時に、「プロジェクト」リストの&#x200B;**wknd-mobile.x.x**&#x200B;を右クリックし、「**モジュール設定を開く**」を選択します。
   * **モジュール/ wknd-mobile.x.x.x /依存関係タブ**&#x200B;で、**Android API 29プラットフォーム**&#x200B;を選択します。 「 OK 」をタップして、変更を閉じ、保存します。
   * これをおこなわないと、エミュレーターを起動しようとすると、「Android SDKを選択してください」というエラーが表示されます。
1. **AVDマネージャー**&#x200B;を開くには、**ツール/AVDマネージャー**&#x200B;を選択するか、上部のバーにある&#x200B;**AVDマネージャー**&#x200B;アイコンをタップします。
1. **AVD Manager**&#x200B;ウィンドウで、「**+仮想デバイスを作成…」をクリックします。**&#x200B;を返します。
   1. 左側で、「**電話**」カテゴリを選択します。
   1. **ピクセル2**&#x200B;を選択します。
   1. 「**次へ**」ボタンをクリックします。
   1. **Q**&#x200B;を&#x200B;**APIレベル29**&#x200B;で選択します。
      * AVD Managerの初回起動時に、バージョン管理されたAPIをダウンロードするよう求められます。 「Q」リリースの横にある「ダウンロード」リンクをクリックし、ダウンロードとインストールを完了します。
   1. 「**次へ**」ボタンをクリックします。
   1. 「**完了**」ボタンをクリックします。
1. **AVDマネージャ**&#x200B;ウィンドウを閉じます。
1. 上部のメニューバーで、「**設定を実行/編集**」ドロップダウンから&#x200B;**wknd-mobile.x.x**&#x200B;を選択します。
1. 選択した&#x200B;**設定を実行/編集**&#x200B;の横にある&#x200B;**実行**&#x200B;ボタンをタップします
1. ポップアップで、新しく作成した&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;仮想デバイスを選択し、「**OK**」をタップします
1. [!DNL WKND Mobile]アプリがすぐに読み込まれない場合は、エミュレーターのAndroidホーム画面で&#x200B;**[!DNL WKND]**&#x200B;アイコンを探してタップします。
   * エミュレーターが起動してもエミュレーターの画面が黒のままの場合は、エミュレーターウィンドウの横にあるエミュレーターのツールウィンドウで&#x200B;**power**&#x200B;ボタンをタップします。
   * 仮想デバイス内をスクロールするには、クリック&amp;ホールドしてドラッグします。
   * AEMのコンテンツを更新するには、上部から更新アイコンまでプルダウンします。
が表示され、リリースされます。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## モバイルアプリコード

この節では、最もやり取りが多く、AEM Content ServicesとそのJSON出力に依存するAndroidモバイルアプリコードについて説明します。

読み込み時に、モバイルアプリは、モバイルアプリを駆動するコンテンツを提供するように設定されたAEM Content Servicesエンドポイントである`HTTP GET`を`/content/wknd-mobile/en/api/events.model.json`にします。

イベントAPIの編集可能なテンプレート(`/content/wknd-mobile/en/api/events.model.json`)はロックされているので、モバイルアプリはコード化して、JSON応答内の特定の場所にある特定の情報を探すことができます。

### 概要レベルのコードフロー

1. [!DNL WKND Mobile]アプリを開くと、`/content/wknd-mobile/en/api/events.model.json`にあるAEMパブリッシュに対して`HTTP GET`リクエストが呼び出され、モバイルアプリのUIに入力するコンテンツが収集されます。
2. AEMからコンテンツを受け取ると、モバイルアプリの3つのビュー要素（**ロゴ、タグ行、イベントリスト**）のそれぞれが、AEMのコンテンツで初期化されます。
   * AEMコンテンツをモバイルアプリのビュー要素にバインドするために、各AEMコンポーネントを表すJSONは、Java POJOにオブジェクトがマッピングされ、Androidビュー要素にバインドされます。
      * 画像コンポーネントJSON →ロゴPOJO →ロゴImageView
      * テキストコンポーネントJSON → TagLine POJO →テキストImageView
      * コンテンツフラグメントリストJSON →イベントPOJO →イベントリサイクラービュー
   * *モバイルアプリコードは、より大きなJSON応答内の既知の場所が原因で、JSONをPOJOにマッピングできます。「image」、「text」および「contentfragmentlist」のJSONキーは、バッキングAEMコンポーネントのノード名によって決まります。 これらのノード名が変更されると、JSONデータから必要なコンテンツをソースする方法が分からなくなるので、モバイルアプリは動作しなくなります。*

#### AEM Content Servicesエンドポイントの呼び出し

以下は、モバイルアプリの`MainActivity`で、AEM Content Servicesを呼び出して、モバイルアプリエクスペリエンスを推進するコンテンツを収集する役割を果たすコードの要約です。

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

`onCreate(..)` はモバイルアプリの初期化フックで、JSONを解析して値を要素にバ `ViewBinders` インドする3つのカスタムを登録し `View` ます。

`initApp(...)` が呼び出され、AEMパブリッシュ上のAEM Content Servicesエンドポイントに対して、コンテンツを収集するためのHTTPGETリクエストがおこなわれます。有効なJSON応答を受け取ると、JSON応答が各`ViewBinder`に渡されます。このは、JSONを解析してモバイル`View`要素にバインドします。

#### JSON応答の解析

次に、`LogoViewBinder`を見てみましょう。これは簡単ですが、いくつかの重要な考慮事項に焦点を当てています。

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

`bind(...)`の最初の行は、コンポーネントが追加されたAEM Layout Containerを表すキー&#x200B;**:items → root → :items**&#x200B;を使用してJSON応答を下に移動します。

ここから、画像コンポーネントを表す&#x200B;**image**&#x200B;という名前のキーをチェックします(このノード名→ JSONキーは安定していることが重要です)。 このオブジェクトが存在する場合は、 Jackson `ObjectMapper`ライブラリを介して[カスタム画像POJO](#image-pojo)に読み取り、マッピングされます。 画像POJOについては、以下で説明します。

最後に、ロゴの`src`が[!DNL Glide]ヘルパーライブラリを使用してAndroid ImageViewに読み込まれます。

AEM Content ServicesはJCRパス(例： `aemHost``/content/dam/wknd-mobile/images/wknd-logo.png`)を参照先のコンテンツに追加します。

#### 画像POJO{#image-pojo}

オプションですが、Gsonなどの他のライブラリで提供される[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)や類似の機能を使用すると、複雑なJSON構造をJava POJOにマッピングするのに役立ち、ネイティブのJSONオブジェクト自体を直接処理する必要はありません。 この単純なケースでは、 `image` JSONオブジェクトの`src`キーを、 `@JSONProperty`注釈を使用して、画像POJOの`src`属性に直接マッピングします。

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

JSONオブジェクトから多くのデータポイントを選択する必要があるイベントPOJOは、単純な画像よりもこの手法のメリットがあります（`src`が必要です）。

## モバイルアプリのエクスペリエンスの参照

これで、AEM Content Servicesがネイティブのモバイルエクスペリエンスを引き出す方法を理解できたので、学習した内容を使用して、次の手順を実行し、変更がモバイルアプリに反映されていることを確認できます。

各手順の後、モバイルアプリをプルして更新し、モバイルエクスペリエンスの更新を確認します。

1. **新しい[!DNL Event]コンテンツフラグメント**&#x200B;を作成して公開します。
1. 既存の&#x200B;**コンテンツフラグメント**&#x200B;を非公開にする[!DNL Event]
1. **タグ行**&#x200B;に更新を発行します

## これで完了です

**AEMヘッドレスチュートリアルを完了しました。**

AEMコンテンツサービスとAEM as a Headless CMSの詳細については、Adobeの他のドキュメントおよびイネーブルメントに関する資料を参照してください。

* [コンテンツフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCMコアコンポーネントユーザーガイド](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンポーネントコンポーネントライブラリ](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCMコアコンポーネントGitHubプロジェクト](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCMコアコンポーネント — エキスパートへの質問](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [コンポーネントエクスポーターのコードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
