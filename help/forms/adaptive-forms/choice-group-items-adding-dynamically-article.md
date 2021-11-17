---
title: 選択グループコンポーネントへの項目の追加
description: 項目を選択グループコンポーネントに動的に追加
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 3%

---

# 項目を選択グループコンポーネントに動的に追加する

AEM Forms 6.5 では、チェックボックス、ラジオボタン、画像リストなどのアダプティブForms選択グループコンポーネントに項目を動的に追加する機能が導入されました。


ユースケースに応じて、Visual Editor やコードエディターを使用して項目を追加できます。

**Visual Editor を使用する場合：** 関数呼び出しまたはサービス呼び出しの結果から、選択グループの項目を入力できます。 例えば、REST API 呼び出しの応答を使用して、選択グループの項目を設定できます。

以下のスクリーンショットでは、 Loan Period(years) のオプションを、getLoanPeriods というサービス呼び出しの結果に設定しています。

![ルールエディター](assets/ruleeditor.png)

**コードエディターの使用**:選択グループの項目を、フォームに入力された値に基づいて動的に設定する場合。 例えば、次のコードスニペットでは、チェックボックスの項目を、アダプティブフォームの applicant name フィールドと spouse フィールドに入力された値に設定します。

コードスニペットでは、チェックボックスコンポーネントである WorkingMembers の項目を設定します。 アイテムの配列は、アダプティブフォームの applicantName および spouse テキストフィールドの値を取得することで、動的に作成されています。

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

お使いのシステムでこれを試すには：

**コードエディターを使用した項目の追加**

* [アセットのダウンロード](assets/usingthecodeeditor.zip)
* [Forms And Documents を開く](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成」 「|ファイルのアップロード」と、前の手順でダウンロードしたファイルをアップロードします。
* [フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 「応募者名」を入力し、「配偶者の有無」を「既婚」に選択します。
* 配偶者の名前を入力
* 次へをクリック
* 配偶者の有無に関わらず、申込者名と配偶者の有無がチェックボックスに表示されます。

**Visual Editor を使用した項目の追加**

* [アセットのダウンロード](assets/usingthevisualeditor.zip)
* Tomcat をインストールします（まだインストールしていない場合）。 [Tomcat のインストール手順は、こちらを参照してください。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [この zip ファイルに含まれる SampleRest.war ファイルを Tomcat にデプロイします。](assets/sample-rest.zip)
* [Forms And Documents を開く](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成」 「|ファイルのアップロード」と、前の手順でダウンロードしたファイルをアップロードします。
* [フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 「 Loan amount 」を入力し、「 」フィールドの外側に「 」タブを入力します。 これにより、トリガーフィールドを表示するルールが設定されます。
* 適切なローン期間を選択します（ローン期間の項目は残りの呼び出しから入力されます）
* 利率を選択し、「償却スケジュールの取得」をクリックします。
* 償却テーブルには値が入力されます。 償却スケジュールは、REST 呼び出しを使用して取得されます。

>[!NOTE]
> Tomcat はポート 8080 で、AEMはポート 4502 で動作していると想定されます
