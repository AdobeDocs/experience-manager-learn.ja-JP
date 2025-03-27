---
title: CRXDE Lite
description: CRXDE Lite は、AEM as a Cloud Service のような開発者環境をデバッグするための、従来の強力なツールです。CRXDE Lite は、すべてのリソースとプロパティの調査から、JCR の可変部分の操作、権限の調査まで、デバッグを支援する一連の機能を提供します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '613'
ht-degree: 100%

---

# CRXDE Lite による AEM as a Cloud Service のデバッグ

CRXDE Lite は、AEM as a Cloud Service 開発環境&#x200B;__のみで__&#x200B;利用できます（ローカルの AEM SDK と同様）。

## AEM オーサーの CRXDE Lite へのアクセス

CRXDE Lite は、AEM as a Cloud Service 開発環境&#x200B;__のみで__&#x200B;利用でき、ステージ環境または実稼動環境では利用&#x200B;__できません__。

AEM オーサーの CRXDE Lite にアクセスする手順は次のとおりです。

1. AEM as a Cloud Service の AEM オーサーサービスにログインします。
1. ツール／一般／CRXDE Lite に移動します

AEM オーサーへのログインに使用する資格情報と権限を使用して、CRXDE Lite が開きます。

## コンテンツのデバッグ

CRXDE Lite では、JCR に直接アクセスできます。CRXDE Lite 経由で表示されるコンテンツは、ユーザーに付与された権限によって制限されます。つまり、アクセス権によっては、JCR 内のすべての項目を表示または変更できない場合があります。

`/apps`、`/libs` および `/oak:index` は不変です。つまり、どのユーザーも実行時に変更できません。JCR 内のこれらの場所は、コードデプロイメントでのみ変更できます。

+ JCR 構造は、左側のナビゲーションパネルを使用してナビゲーションおよび操作します
+ 左側のナビゲーションウィンドウでノードを選択すると、ノードプロパティが下側のパネルに表示されます。
   + プロパティは、ウィンドウから追加、削除、変更できます
+ 左側のナビゲーションでファイルノードをダブルクリックすると、右上のパネルにファイルのコンテンツが表示されます
+ 左上の「すべて保存」ボタンをクリックして変更内容を保存するか、「すべて保存」の横にある下向き矢印をクリックして、未保存の変更内容を元に戻します。

![CRXDE Lite - コンテンツのデバッグ](./assets/crxde-lite/debugging-content.png)

実行時に、CRXDE Lite を介して AEM as a Cloud Service の開発環境で可変コンテンツに変更を加える場合は、注意が必要です。
CRXDE Lite を介して AEM に直接加えられた変更は、追跡と管理が困難な場合があります。必要に応じて、CRXDE Lite を通じて行われた変更を AEM プロジェクトの可変コンテンツパッケージ（`ui.content`）を送信し、問題が解決されるよう Git にコミットします。CRXDE Lite を介して AEM に直接変更するのではなく、すべてのアプリケーションコンテンツの変更がコードベースから始まり、デプロイメントを介して AEM に送られるのが理想です。

### アクセス制御のデバッグ

CRXDE Lite を使用すると、特定のユーザーまたはグループ（プリンシパル）の特定のノードに対するアクセス制御をテストおよび評価できます。

CRXDE Lite で「アクセス制御のテスト」コンソールにアクセスするには、次の場所に移動します。

+ CRXDE Lite／ツール／アクセス制御をテスト...

![CRXDE Lite - アクセス制御のテスト](./assets/crxde-lite/permissions__test-access-control.png)

1. 「パス」フィールドを使用して、評価する JCR パスを選択します
1. 「プリンシパル」フィールドを使用して、パスを評価するユーザーまたはグループを選択します
1. 「テスト」ボタンをクリックします

結果は次のように表示されます。

+ __Path__ 評価されたパスを繰り返します
+ __Principal__ パスが評価されたユーザーまたはグループを繰り返します
+ __Principals__ 選択したプリンシパルが属するすべてのプリンシパルの一覧が表示されます。
   + これは、継承を通じてアクセス許可を提供する推移的なグループメンバーシップの理解に役立ちます。
+ __Privileges at Path__ 選択したプリンシパルが評価されたパスに対して持つすべての JCR 権限の一覧が表示されます

### サポートされていないデバッグアクティビティ

次は、CRXDE Lite で実行することが&#x200B;__できない__&#x200B;デバッグ作業です。

### OSGi 設定のデバッグ

デプロイ済みの OSGi 設定は、CRXDE Liteで確認できません。OSGi の設定は、AEM プロジェクトの `ui.apps` コードパッケージの `/apps/example/config.xxx` で管理されていますが、AEM as a Cloud Service 環境にデプロイすると、OSGi 設定リソースは JCR に保持されず、CRXDE Lite で表示されません。

代わりに、[Developer Console／設定](./developer-console.md#configurations)で、デプロイされた OSGi の設定を確認することができます。
