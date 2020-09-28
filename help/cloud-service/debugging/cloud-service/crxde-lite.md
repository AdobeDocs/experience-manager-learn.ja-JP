---
title: CRXDE Lite
description: 'CRXDE Liteは、AEM開発者環境としてCloud Serviceをデバッグするための、従来の強力なツールです。 CRXDE Liteは、すべてのリソースとプロパティの検査、JCRの可変部分の操作、権限の調査からデバッグを支援する一連の機能を提供します。 '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# CRXDE Liteを使用したCloud ServiceとしてのAEMのデバッグ

CRXDE Liteは、AEM開発環境(およびローカルのAEM __SDK)としてCloud Serviceでのみ__ 使用できます。

## AEM作成者のCRXDE Liteへのアクセス

CRXDE Liteは、AEM開発環境としてCloud Service上で __のみアクセス可能で__ 、ステージ環境または実稼働では __使用できません__ 。

AEM作成者のCRXDE Liteにアクセスするには：

1. Cloud ServiceのAEM AuthorサービスとしてAEMにログインします。
1. ツール/一般/CRXDE Liteに移動します。

これにより、AEM Authorへのログインに使用した資格情報と権限を使用してCRXDE Liteが開きます。

## コンテンツのデバッグ

CRXDE Liteは、JCRに直接アクセスできます。 CRXDE Liteを介して表示できるコンテンツは、ユーザーに与えられた権限によって制限されます。つまり、アクセス権によっては、JCR内のすべての情報を表示または変更できない場合があります。

、 `/apps`およ `/libs``/oak:index` びは不変です。つまり、どのユーザーも実行時に変更することはできません。 JCR内のこれらの場所は、コードデプロイメントを介してのみ変更できます。

+ JCR構造は、左側のナビゲーションペインを使用してナビゲーションおよび操作します
+ 左側のナビゲーションペインでノードを選択すると、下部ペインにノードプロパティが表示されます。
   + プロパティは、ペインから追加、削除、変更できます
+ 左のナビゲーションで重複がファイルノードをクリックすると、右上のペインでファイルのコンテンツが開きます
+ 左上の「すべて保存」ボタンをタップして変更内容を保持するか、「すべて保存」の横の下向き矢印をタップして未保存の変更を元に戻します。

![CRXDE Lite — コンテンツのデバッグ](./assets/crxde-lite/debugging-content.png)

AEMで、CRXDE Liteを介したCloud Service開発環境として、実行時に可変コンテンツに変更を加える場合は、注意が必要です。
CRXDE Liteを介してAEMに直接変更を加えた場合、追跡や管理が難しくなる可能性があります。 必要に応じて、CRXDE Liteを介して行った変更をAEMプロジェクトの可変コンテンツパッケージ(`ui.content`)に戻し、Gitにコミットして、問題の解決を確実に図ります。 アプリケーションのコンテンツに対するすべての変更は、CRXDE Lite経由で直接AEMに変更を加えるのではなく、コードベースから開始され、デプロイメント経由でAEMに流れ込むのが理想的です。

### アクセス制御のデバッグ

CRXDE Liteは、特定のユーザーまたはグループ（プリンシパル）に関する特定のノードのアクセス制御をテストし、評価する方法を提供します。

CRXDE Liteのテストアクセス制御コンソールにアクセスするには、次の場所に移動します。

+ CRXDE Lite/ツール/テストアクセス制御...

![CRXDE Lite — テストアクセス制御](./assets/crxde-lite/permissions__test-access-control.png)

1. Pathフィールドを使用して、評価するJCRパスを選択します
1. 「プリンシパル」フィールドを使用して、パスの値を指定するユーザーまたはグループを選択します
1. 「テスト」ボタンをタップします

結果は次のように表示されます。

+ __Path__ ：評価されたパスを繰り返します
+ __Principal__ は、パスの評価対象となったユーザーまたはグループを繰り返します
+ __Principals__ リストは、選択したプリンシパルが属するすべてのプリンシパルを指定します。
   + 継承を介して権限を与える可能性のある推移的なグループメンバーシップを理解すると役立ちます。
+ __パスリストーでの権限__ 。選択したプリンシパルが評価されたパスで持つすべてのJCR権限

### サポートされないデバッグアクティビティ

以下は、CRXDE Liteで実行できないデバッグアクティビティ __です__ 。

### OSGi設定のデバッグ

デプロイされたOSGi設定は、CRXDE Liteを介してレビューすることはできません。 OSGi設定はAEMプロジェクトの `ui.apps``/apps/example/config.xxx`コードパッケージに保持されますが、AEM環境としてCloud Serviceにデプロイする場合、OSGi設定リソースはJCRに保持されないので、CRXDE Lite経由では表示されません。

代わりに、 [Developer Console/Configurations](./developer-console.md#configurations) （設定）を使用して、デプロイ済みのOSGi設定を確認します。
