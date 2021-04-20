---
title: CRXDE Lite
description: 'CRXDE Liteは、AEM開発者環境としてCloud Serviceをデバッグするための、従来の強力なツールです。 CRXDE Liteは、すべてのリソースとプロパティの検査、JCRの可変部分の操作、権限の調査からデバッグを支援する一連の機能を提供します。 '
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# CRXDE Liteを使用したCloud ServiceとしてのAEMのデバッグ

CRXDE Liteは&#x200B;__ONLY__&#x200B;で、AEM上でCloud Service開発環境として(ローカルのAEM SDKと同様に)利用できます。

## AEM作成者のCRXDE Liteへのアクセス

CRXDE Liteは、AEM上でCloud Service開発環境として&#x200B;__のみ__&#x200B;アクセス可能で、ステージまたは実稼動環境では&#x200B;__利用できません。__

AEM作成者のCRXDE Liteにアクセスするには：

1. Cloud ServiceのAEM AuthorサービスとしてAEMにログインします。
1. ツール/一般/CRXDE Liteに移動します。

これにより、AEM Authorへのログインに使用した資格情報と権限を使用してCRXDE Liteが開きます。

## コンテンツのデバッグ

CRXDE Liteは、JCRに直接アクセスできます。 CRXDE Liteを介して表示できるコンテンツは、ユーザーに与えられた権限によって制限されます。つまり、アクセス権によっては、JCR内のすべての情報を表示または変更できない場合があります。

`/apps`、`/libs`、`/oak:index`は不変であり、どのユーザーも実行時に変更することはできません。 JCR内のこれらの場所は、コードデプロイメントを介してのみ変更できます。

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

+ ____ 評価されたパスを反復します
+ ____ Principalは、パスが評価されたユーザーまたはグループを繰り返します。
+ ____ Principalsは、選択したプリンシパルが属するすべてのプリンシパルを示します。
   + 継承を介して権限を与える可能性のある推移的なグループメンバーシップを理解すると役立ちます。
+ __「__ パスの権限」には、選択したプリンシパルが評価されたパスで持つすべてのJCR権限が表示されます

### サポートされないデバッグアクティビティ

以下は、CRXDE Liteで&#x200B;__実行できない__&#x200B;デバッグアクティビティです。

### OSGi設定のデバッグ

デプロイされたOSGi設定は、CRXDE Liteを介してレビューすることはできません。 OSGi設定は、AEMプロジェクトの`ui.apps`コードパッケージ(`/apps/example/config.xxx`)で管理されますが、AEMにCloud Service環境としてデプロイした場合、OSGi設定リソースはJCRに保持されないので、CRXDE Liteを介しては表示されません。

代わりに、[Developer Console > Configurations](./developer-console.md#configurations)を使用して、デプロイ済みのOSGi設定を確認します。
