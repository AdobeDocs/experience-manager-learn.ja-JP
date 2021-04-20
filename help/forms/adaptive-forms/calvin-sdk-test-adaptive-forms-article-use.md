---
title: 'AEMアダプティブFormsでの自動テストの使用 '
seo-title: 'AEMアダプティブFormsでの自動テストの使用 '
description: Calvin SDKを使用したアダプティブFormsの自動テスト
seo-description: Calvin SDKを使用したアダプティブFormsの自動テスト
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 6%

---


# AEMアダプティブFormsでの自動テストの使用{#using-automated-tests-with-aem-adaptive-forms}

Calvin SDKを使用したアダプティブFormsの自動テスト

Calvin SDK は、アダプティブフォームをテストするためのアダプティブフォーム開発者用のユーティリティ API です。Calvin SDKは、[Hobbes.jsのテストフレームワーク](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)の上に構築されています。 Calvin SDKは、AEM Forms6.3以降で利用可能です。

このチュートリアルでは、次を作成します。

* テストスイート
* テストスイートには1つ以上のテストケースが含まれます
* テストケースには1つ以上のアクションが含まれます

## 概要 {#getting-started}

[Package ](assets/testingadaptiveformsusingcalvinsdk1.zip)Managerを使用したアセットのダウンロードとインストールこのパッケージには、サンプルスクリプトといくつかのアダプティブFormsが含まれています。これらのアダプティブFormsは、AEM Forms6.3バージョンを使用して構築されています。AEM Forms6.4以降でテストする場合は、ご使用のバージョンのAEM Forms専用の新しいフォームを作成することをお勧めします。 サンプルスクリプトは、アダプティブFormsをテストするために利用可能な様々なCalvin SDK APIをデモします。 AEMアダプティブFormsのテストの一般的な手順は次のとおりです。

* テストする必要があるフォームに移動します
* フィールドの値を設定する
* アダプティブフォームの送信
* エラーメッセージの確認

パッケージのサンプルスクリプトでは、上記のすべての操作について説明しています。
`mortgageForm.js`のコードを調べます。

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上記のコードは新しいテストスイートを作成します。

* この場合のTestSuiteの名前は&#39; `Mortgage Form Test` &#39;です。
* が、テストスイートを含むjsファイルへのAEM内の絶対パスです。
* 「 `true` 」に設定した場合、レジスタパラメータがテストスイートをテストUIで使用できるようにします。

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>この機能をAEM Forms6.4以降でテストする場合は、新しいアダプティブフォームを作成し、それを使用してテストを行ってください。パッケージに付属のアダプティブフォームの使用はお勧めしません。

テストケースをテストスイートに追加して、アダプティブフォームに対して実行できます。

* テストケースをテストスイートに追加するには、TestSuiteオブジェクトの`addTestCase`メソッドを使用します。
* `addTestCase`メソッドは、TestCaseオブジェクトをパラメーターとして受け取ります。
* TestCaseを作成するには、`hobs.TestCase(..)`メソッドを使用します。
* 注意：最初のパラメーターは、UIに表示されるテストケースの名前です。
* テストケースを作成したら、テストケースにアクションを追加できます。
* `navigateTo`、`asserts.isTrue`などのアクションをアクションとしてテストケースに追加できます。

## 自動テストの実行{#running-the-automated-tests}

[testsuiteを](http://localhost:4502/libs/granite/testing/hobbes.html)開き、テストスイートを展開してテストを実行します。すべてが正常に実行されると、次の出力が表示されます。

![calvinsdk](assets/calvinimage.png)

## サンプルテストスイート{#try-out-the-sample-test-suites}を試す

サンプルパッケージには、さらに3つのテストスイートが含まれています。 次に示すように、clientlibraryのjs.txtファイルに適切なファイルを含めることで、ファイルを試すことができます。

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
