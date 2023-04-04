---
title: アカウント拡張機能のアカウントとサービスのAsset compute
description: asset computeワーカーの開発には、AEM as a Cloud Service、App Builder、MicrosoftまたはAmazonが提供するクラウドストレージを含むアカウントおよびサービスにアクセスする必要があります。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 3%

---

# アカウントおよびサービスの設定

このチュートリアルでは、次のサービスをプロビジョニングし、学習者のAdobe IDを通じてアクセスできる必要があります。

すべてのAdobe サービスは、Adobe IDを使用して、同じAdobe組織からアクセスできる必要があります。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + プロビジョニングには 2 ～ 10 日かかる場合があります
+ クラウドストレージ
   + [Azure Blob ストレージ](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + または [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>このチュートリアルを続行する前に、前述のすべてのサービスにアクセスできることを確認してください。
> 
> 必要なサービスの設定およびプロビジョニング方法について、以下のセクションを確認してください。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

カスタムAsset computeワーカーを呼び出すようにAEM Assets処理プロファイルを設定するには、AEMas a Cloud Service環境へのアクセスが必要です。

サンドボックスプログラムまたはサンドボックス以外の開発環境を使用できるのが理想です。

ローカルAEM SDK はAsset computeマイクロサービスと通信できず、代わりに真のAEMas a Cloud Service環境が必要なので、ローカルAEM SDK ではこのチュートリアルを完了するには不十分です。

## App Builder{#app-builder}

この [App Builder](https://developer.adobe.com/app-builder/) framework は、カスタムアクションを構築し、AdobeのサーバーレスプラットフォームであるAdobe I/O Runtimeにデプロイするために使用されます。 AEMAsset computeプロジェクトは、処理プロファイルを介してAEM Assetsと統合し、アセットバイナリへのアクセスと処理を可能にする、特別に作成された App Builder プロジェクトです。

App Builder にアクセスするには、新規登録してプレビューします。

1. [App Builder 体験版にサインアップ](https://developer.adobe.com/app-builder/trial/).
1. プロビジョニングが完了したことを電子メールで通知されるまで約 2 ～ 10 日待ってから、チュートリアルを続行します。
   + プロビジョニングされているかどうかがわからない場合は、次の手順に進み、 __App Builder__ プロジェクト [Adobe Developer Console](https://developer.adobe.com/console/) まだプロビジョニングされていません。

## クラウドストレージ

クラウドプロジェクトのローカル開発には、Asset computeストレージが必要です。

asset computeワーカーがAEM as a Cloud Serviceで直接使用するためにAdobe I/O Runtimeにデプロイされる場合、このクラウドストレージは、アセットの読み取りとレンディションの書き込み元となるクラウドストレージをAEMが提供するので、厳密に必要とは限りません。

### Microsoft Azure Blob Storage{#azure-blob-storage}

Microsoft Azure BLOB ストレージへのアクセス権をまだ持っていない場合は、 [12 か月の無料アカウント](https://azure.microsoft.com/en-us/free/).

このチュートリアルでは Azure Blob Storage を使用しますが、 [Amazon S3](#amazon-s3) は、チュートリアルに対する小さな変更のみと同様に使用できます。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Azure Blob ストレージのプロビジョニングのクリックスルー（オーディオなし）_

1. にログインします。 [Microsoft Azure アカウント](https://azure.microsoft.com/en-us/account/).
1. 次に移動： __ストレージアカウント__ Azure サービスセクション
1. タップ __+追加__ 新しい BLOB ストレージアカウントを作成するには、以下を実行します。
1. 新しい __リソースグループ__ 必要に応じて、次に例を示します。 `aem-as-a-cloud-service`
1. 次を提供： __ストレージアカウント名__&#x200B;例： `aemguideswkndassetcomput`
   + この __ストレージアカウント名__  使用対象 [クラウドストレージの設定](../develop/environment-variables.md) ローカルAsset compute開発ツール
   + この __アクセスキー__ ストレージアカウントに関連付ける必要があるのは、 [クラウドストレージの設定](../develop/environment-variables.md).
1. それ以外の値はデフォルトのままにし、 __確認して作成__ ボタン
   + 必要に応じて、 __場所__ 近くにいる。
1. プロビジョニング要求が正しいかどうかを確認し、 __作成__ 満たされた場合のボタン

### Amazon S3{#amazon-s3}

使用 [Microsoft Azure Blob Storage](#azure-blob-storage) ただし、このチュートリアルを完了することをお勧めします。 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) また、を使用することもできます。

Amazon S3 ストレージを使用する場合は、 [プロジェクトの環境変数の設定](../develop/environment-variables.md#amazon-s3).

このチュートリアル用にクラウドストレージを特別にプロビジョニングする必要がある場合は、 [Azure Blob ストレージ](#azure-blob-storage).
