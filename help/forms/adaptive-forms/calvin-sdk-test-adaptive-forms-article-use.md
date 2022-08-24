---
title: AEM Adaptive Formsでの自動テストの使用
description: Calvin SDK を使用したアダプティブFormsの自動テスト
feature: Adaptive Forms
doc-type: article
activity: develop
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 8%

---

# AEM Adaptive Formsでの自動テストの使用 {#using-automated-tests-with-aem-adaptive-forms}

Calvin SDK を使用したアダプティブFormsの自動テスト

Calvin SDK は、アダプティブフォームをテストするためのアダプティブフォーム開発者用のユーティリティ API です。Calvin SDK は [Hobbes.js テストフレームワーク](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)上に構築されます。Calvin SDK は、AEM Forms 6.3 以降で使用できます。

このチュートリアルでは、次のものを作成します。

* テストスイート
* テストスイートには 1 つ以上のテストケースが含まれます
* テストケースには 1 つ以上のアクションが含まれます

## 概要 {#getting-started}

[パッケージマネージャーを使用したアセットのダウンロードとインストール](assets/testingadaptiveformsusingcalvinsdk1.zip)パッケージには、サンプルスクリプトといくつかのアダプティブFormsが含まれています。これらのアダプティブFormsは、AEM Forms 6.3 バージョンを使用して構築されています。 AEM Forms 6.4 以降でテストする場合は、お使いのバージョンのAEM Formsに固有の新しいフォームを作成することをお勧めします。 このサンプルスクリプトは、Adaptive Formsのテストに使用できる様々な Calvin SDK API を示しています。 AEM Adaptive Formsをテストする一般的な手順は次のとおりです。

* テストする必要があるフォームに移動します
* フィールドの値を設定
* アダプティブフォームを送信する
* エラーメッセージを確認

パッケージ内のサンプルスクリプトは、上記のすべてのアクションを示しています。
のコードを見てみましょう `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上記のコードは、新しいテストスイートを作成します。

* この場合の TestSuite の名前は「 」です。 `Mortgage Form Test` &#39;.
* テストスイートを含む js ファイルへのAEMの絶対パスが指定されます。
* &#39;に設定された場合のレジスターパラメーター `true` 」と入力すると、テストスイートがテスト UI で使用できるようになります。

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
>この機能をAEM Forms 6.4 以降でテストする場合は、新しいアダプティブフォームを作成し、それを使用してテストを行ってください。パッケージに付属するアダプティブフォームの使用はお勧めしません。

アダプティブフォームに対して実行するテストスイートにテストケースを追加できます。

* テストスイートにテストケースを追加するには、 `addTestCase` TestSuite オブジェクトのメソッド。
* この `addTestCase` メソッドは、 TestCase オブジェクトをパラメーターとして受け取ります。
* TestCase を作成するには、 `hobs.TestCase(..)` メソッド。
* 注意：最初のパラメーターは、UI に表示されるテストケースの名前です。
* テストケースを作成したら、テストケースにアクションを追加できます。
* 次を含むアクション `navigateTo`, `asserts.isTrue` は、アクションとしてテストケースに追加できます。

## 自動化されたテストの実行 {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)「 Test Suite 」を展開し、テストを実行します。 すべてが正常に実行された場合は、次の出力が表示されます。

![calvinsdk](assets/calvinimage.png)

## サンプルテストスイートを試す {#try-out-the-sample-test-suites}

サンプルパッケージの一部として、追加のテストスイートが 3 つあります。 次に示すように、クライアントライブラリの js.txt ファイルに適切なファイルを含めることで、ファイルを試すことができます。

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
