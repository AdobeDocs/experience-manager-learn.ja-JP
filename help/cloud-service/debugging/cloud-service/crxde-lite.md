---
title: CRXDE Lite
description: 'CRXDE Liteは、AEM as a Classicの強力なツールです。Cloud Service開発環境としてデバッグする場合に役立ちます。 CRXDE Liteは、すべてのリソースとプロパティを調べ、JCRの可変部分を操作し、権限を調べるデバッグを支援する一連の機能を提供します。 '
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# CRXDE Liteを使用したCloud ServiceとしてのAEMのデバッグ

CRXDE Liteは、AEM as aCloud Service開発環境(およびローカルAEM SDK)でのみ&#x200B;__使用できます。__

## AEMオーサーのCRXDE Liteへのアクセス

CRXDE Liteは、AEM as a Cloud Service開発環境として&#x200B;__のみ__&#x200B;アクセスでき、ステージ環境または実稼動環境では&#x200B;__使用できません__。

AEMオーサーのCRXDE Liteにアクセスするには：

1. AEM as a A AuthorサービスにCloud Serviceとしてログインします。
1. ツール/一般/CRXDE Liteに移動します。

これにより、CRXDE LiteがAEMオーサーへのログインに使用される資格情報と権限を使用して開きます。

## コンテンツのデバッグ

CRXDE Liteは、JCRに直接アクセスできます。 CRXDE Lite経由で表示されるコンテンツは、ユーザーに付与された権限によって制限されます。つまり、アクセス権によっては、JCR内のすべての項目を表示または変更できない場合があります。

`/apps`、`/libs`および`/oak:index`は不変です。つまり、どのユーザーも実行時に変更することはできません。 JCR内のこれらの場所は、コードデプロイメント経由でのみ変更できます。

+ JCR構造のナビゲーションと操作は、左側のナビゲーションペインを使用して行います
+ 左側のナビゲーションペインでノードを選択すると、ノードプロパティのが下側のペインに表示されます。
   + プロパティは、ペインから追加、削除、または変更できます
+ 左側のナビゲーションでファイルノードをダブルクリックすると、右上のペインにファイルのコンテンツが開きます
+ 左上の「すべて保存」ボタンをタップして変更内容を保存するか、「すべて保存して未保存の変更内容を元に戻す」の横の下矢印をタップします。

![CRXDE Lite — コンテンツのデバッグ](./assets/crxde-lite/debugging-content.png)

CRXDE Liteを介した、AEM as aCloud Service開発環境での実行時の可変コンテンツの変更は、慎重におこなう必要があります。
CRXDE Liteを介してAEMに直接加えた変更は、追跡と管理が困難な場合があります。 必要に応じて、CRXDE Liteを通じて加えられた変更をAEMプロジェクトの可変コンテンツパッケージ(`ui.content`)に戻し、Gitにコミットして、問題が解決されるようにします。 CRXDE Liteを介してAEMに直接変更するのではなく、すべてのアプリケーションコンテンツの変更はコードベースから始まり、デプロイメントを介してAEMに反映するのが理想的です。

### アクセス制御のデバッグ

CRXDE Liteは、特定のユーザーまたはグループ（プリンシパル）の特定のノードに対するアクセス制御をテストおよび評価する方法を提供します。

CRXDE Liteのアクセス制御をテストコンソールにアクセスするには、次の場所に移動します。

+ CRXDE Lite/ツール/アクセス制御をテスト…

![CRXDE Lite — アクセス制御のテスト](./assets/crxde-lite/permissions__test-access-control.png)

1. 「パス」フィールドで、評価するJCRパスを選択します
1. 「プリンシパル」フィールドを使用して、パスの値を設定するユーザーまたはグループを選択します
1. 「テスト」ボタンをタップします。

結果は次のように表示されます。

+ ____ Pathreiterは、評価されたパスを繰り返します
+ ____ Principalreiterは、パスが評価されたユーザーまたはグループを繰り返します
+ ____ Principalsは、選択したプリンシパルが属するすべてのプリンシパルをリストします。
   + 継承を介して権限を提供する可能性のある推移的なグループメンバーシップを理解すると役立ちます。
+ __パスでの権__ 限には、評価されたパスに対して選択したプリンシパルが持つすべてのJCR権限が一覧表示されます

### サポートされていないデバッグアクティビティ

以下は、CRXDE Liteで&#x200B;__実行できない__&#x200B;デバッグアクティビティです。

### OSGi設定のデバッグ

デプロイ済みのOSGi設定は、CRXDE Liteで確認できません。 OSGi設定は、AEMプロジェクトの`ui.apps`コードパッケージ(`/apps/example/config.xxx`)で管理されますが、AEMにCloud Service環境としてデプロイすると、OSGi設定リソースはJCRに保持されないので、CRXDE Lite経由では表示されません。

代わりに、[開発者コンソール/設定](./developer-console.md#configurations)を使用して、デプロイ済みのOSGi設定を確認します。
