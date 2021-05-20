---
title: 選択グループコンポーネントへの項目の追加
seo-title: 選択グループコンポーネントへの項目の追加
description: 項目を選択グループコンポーネントに動的に追加する
seo-description: 項目を選択グループコンポーネントに動的に追加する
feature: アダプティブフォーム
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 開発
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 3%

---



# 選択グループコンポーネントへの項目の動的な追加

AEM Forms 6.5では、チェックボックス、ラジオボタン、画像リストなど、アダプティブForms選択グループコンポーネントに項目を動的に追加する機能が導入されました。

[この機能は、サンプルサーバー上で有効に使用できます](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。動的チェックボックス項目カードを検索し、「試す」をクリックします。


使用事例に応じて、Visual Editorやコードエディターを使用して項目を追加できます。

**ビジュアルエディターの使用：** 関数呼び出しまたはサービス呼び出しの結果から、選択グループの項目を入力できます。例えば、REST API呼び出しの応答を使用して、選択グループの項目を設定できます。

下のスクリーンショットでは、「Loan Periods(years)」のオプションを、getLoanPeriodsというサービス呼び出しの結果に設定しています。

![ルールエディター](assets/ruleeditor.png)

**コードエディターを使用して、次の操作を行います**。選択グループの項目を、フォームに入力された値に基づいて動的に設定する場合。例えば、次のコードスニペットでは、チェックボックスの項目を、アダプティブフォームの「 applicant name 」フィールドと「 spouse 」フィールドに入力された値に設定します。

コードスニペットで、チェックボックスコンポーネントであるWorkingMembersの項目を設定します。 アイテムの配列は、アダプティブフォームのapplicantNameおよびspouseテキストフィールドの値を取得することで、動的に作成されます

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

送信データは次のようになります

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
* [Formsとドキュメントを開く](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成 「|ファイルのアップロード」をクリックし、前の手順でダウンロードしたファイルをアップロードします。
* [フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 応募者名を入力し、「婚姻状況」から「既婚」を選択します。
* 配偶者の名前を入力します
* 「次へ」をクリックします。
* 婚姻状況が既婚の場合は、申込者名と配偶者名が入力されたチェックボックスが表示されます

**Visual Editorを使用した項目の追加**

* [アセットのダウンロード](assets/usingthevisualeditor.zip)
* Tomcatをまだインストールしていない場合は、インストールします。 [Tomcatのインストール手順は、こちらから参照できます。](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [TomcatでのSampleRest.warファイルのデプロイ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Formsとドキュメントを開く](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成 「|ファイルのアップロード」をクリックし、前の手順でダウンロードしたファイルをアップロードします。
* [フォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 「 Loan amount 」と入力し、フィールドの「 」タブをクリックします。 これにより、トリガーフィールドを表示するルールが設定されます。
* 適切なローン期間を選択します（ローン期間の項目は残りの呼び出しから入力されます）。
* 利率を選択し、「償却スケジュールの取得」をクリックします。
* 償却表に値が入力されます。 償却スケジュールは、REST呼び出しを使用して取得されます。

>[!NOTE]
> Tomcatはポート8080で、AEMはポート4502で動作していると想定されます
