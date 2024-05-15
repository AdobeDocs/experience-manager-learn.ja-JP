---
title: 選択グループコンポーネントへの項目の追加
description: 項目を選択グループコンポーネントに動的に追加
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 337
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 100%

---

# 項目を選択グループコンポーネントに動的に追加

AEM Forms 6.5 では、アダプティブフォーム選択グループコンポーネント（チェックボックス、ラジオボタン、画像リストなど）に項目を動的に追加する機能が導入されました。


ユースケースに応じて、ビジュアルエディターやコードエディターを使用して項目を追加できます。

**ビジュアルエディターの使用：**&#x200B;関数呼び出しまたはサービス呼び出しの結果から、選択グループの項目を入力できます。 例えば、REST API 呼び出しの応答を使用して、選択グループの項目を設定できます。

以下のスクリーンショットでは、 Loan Period(years) のオプションを、getLoanPeriods というサービス呼び出しの結果に設定しています。

![ルールエディター](assets/ruleeditor.png)

**コードエディターの使用**：フォームに入力された値に基づいて、選択グループの項目を動的に設定する場合。 例えば、次のコードスニペットでは、チェックボックスの項目を、アダプティブフォームの申込者の名前と配偶者のフィールドに入力された値に設定します。

コードスニペットでは、チェックボックスコンポーネントである WorkingMembers の項目を設定します。 項目の配列は、アダプティブフォームの applicantName および spouse テキストフィールドの値を取得することで、動的に作成されています。

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

送信されたデータは次のようになります

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**ルールエディターを使用した項目の追加**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**コードエディターを使用した項目の追加**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

お使いのシステムでこれを試す方法は次のとおりです。

**コードエディターを使用した項目の追加**

* [アセットをダウンロード](assets/usingthecodeeditor.zip)
* [Forms とドキュメントを開く](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成／ファイルのアップロード」をクリックし、前の手順でダウンロードしたファイルをアップロードします
* [Forms をプレビュー](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 「申込者名」を入力し、「配偶者の有無」を「既婚」に選択します
* 配偶者の名前を入力
* 「次へ」をクリックします。
* 配偶者の有無が既婚の場合は、申込者の名前と配偶者の名前が入力されたチェックボックスが表示されます

**ビジュアルエディターを使用した項目の追加**

* [アセットをダウンロード](assets/usingthevisualeditor.zip)
* Tomcat をインストールします（まだインストールしていない場合）。 [Tomcat のインストール手順は、こちらを参照してください](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=ja)
* [この zip ファイルに含まれる SampleRest.war ファイルを Tomcat にデプロイします。](assets/sample-rest.zip)
* [Forms とドキュメントを開く](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成／ファイルのアップロード」をクリックし、前の手順でダウンロードしたファイルをアップロードします
* [Forms をプレビュー](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 「ローン金額」を入力し、フィールドの外にタブアウトします。これにより、ローン期間フィールドを表示するルールがトリガーされます。
* 適切なローン期間を選択します（ローン期間の項目は残りの呼び出しから入力されます）
* 利率を選択し、「償還スケジュールを取得」をクリックします。
* 償還テーブルに値が入力されます。 償還スケジュールは、REST 呼び出しを使用して取得されます。

>[!NOTE]
> Tomcat はポート 8080 で、AEM はポート 4502 で実行されていると想定されています
