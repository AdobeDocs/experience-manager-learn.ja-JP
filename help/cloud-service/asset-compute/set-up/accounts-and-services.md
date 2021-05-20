---
title: アカウント拡張機能のアカウントとサービスのAsset compute
description: asset computeワーカーの開発には、AEM as aCloud Service、AdobeプロジェクトFirefly、MicrosoftまたはAmazonが提供するクラウドストレージなど、アカウントやサービスへのアクセスが必要です。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 2%

---


# アカウントとサービスの設定

このチュートリアルでは、次のサービスをプロビジョニングし、学習者のAdobe IDを通じてアクセスできる必要があります。

すべてのAdobeサービスは、Adobe IDを使用して、同じAdobe組織からアクセスできる必要があります。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [AdobeプロジェクトFireFly](#adobe-project-firefly)
   + プロビジョニングには2 ～ 10日かかる場合があります
+ クラウドストレージ
   + [Azure Blobストレージ](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + または[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>このチュートリアルを続行する前に、前述のすべてのサービスにアクセスできることを確認してください。
> 
> 必要なサービスの設定およびプロビジョニング方法に関する以下の節を確認します。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

AEM as a Cloud Service環境へのアクセスは、AEM Assets処理プロファイルを設定してカスタムAsset computeワーカーを呼び出すために必要です。

サンドボックスプログラムまたはサンドボックス以外の開発環境を使用できるのが理想的です。

ローカルのAEM SDKは、Asset computeマイクロサービスと通信できず、代わりに、Cloud Service環境としての真のAEMが必要なので、ローカルのAEM SDKでは、このチュートリアルを完了するには不十分です。

## AdobeプロジェクトFirefly{#adobe-project-firefly}

[AdobeプロジェクトFirefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)フレームワークは、AdobeのサーバーレスプラットフォームであるAdobe I/O Runtimeにカスタムアクションを作成しデプロイするために使用されます。 AEMAsset computeプロジェクトは、処理プロファイルを介してAEM Assetsと統合し、アセットバイナリにアクセスして処理する機能を提供する、特別に構築されたFireflyプロジェクトです。

Project Fireflyにアクセスするには、プレビューに新規登録します。

1. [Project Fireflyプレビューに新規登録](https://adobeio.typeform.com/to/obqgRm)。
1. プロビジョニングが完了したことを電子メールで通知されるまで、約2～10日待ってから、チュートリアルを続行します。
   + プロビジョニング済みかどうかがわからない場合は、次の手順に進み、 [Adobe開発者コンソール](https://console.adobe.io)で&#x200B;__Project Firefly__&#x200B;プロジェクトを作成できない場合は、まだプロビジョニングされていません。

## クラウドストレージ

クラウドストレージは、Asset computeプロジェクトのローカル開発に必要です。

asset computeワーカーがAEMをCloud Serviceとして直接使用するためにAdobe I/O Runtimeにデプロイされる場合、AEMはアセットの読み取りとレンディションの元となるクラウドストレージを提供するので、厳密にはこのクラウドストレージは必要ありません。

### Microsoft Azure Blob Storage{#azure-blob-storage}

Microsoft Azure Blob Storageへのアクセス権をまだ持っていない場合は、12ヶ月の無料アカウント](https://azure.microsoft.com/en-us/free/)に新規登録します。[

このチュートリアルではAzure Blob Storageを使用しますが、[Amazon S3](#amazon-s3)は、チュートリアルに対する小さな変更のみと共に使用できます。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Azure Blob Storageのプロビジョニングのクリックスルー（オーディオなし）_


1. [Microsoft Azureアカウント](https://azure.microsoft.com/en-us/account/)にログインします。
1. 「__ストレージアカウント__ Azureサービス」セクションに移動します。
1. __+「追加__」をタップして、新しいBLOBストレージアカウントを作成します
1. 必要に応じて、新しい&#x200B;__リソースグループ__&#x200B;を作成します。次に例を示します。`aem-as-a-cloud-service`
1. __ストレージアカウント名__&#x200B;を指定します。次に例を示します。`aemguideswkndassetcomput`
   + __ストレージアカウント名__&#x200B;は、ローカルAsset compute開発ツール用の[クラウドストレージ](../develop/environment-variables.md)の設定に使用されます
   + [クラウドストレージ](../develop/environment-variables.md)を設定する際には、ストレージアカウントに関連付けられた&#x200B;__アクセスキー__&#x200B;も必要です。
1. それ以外はデフォルトのままにし、「__レビュー+作成__」ボタンをタップします
   + 必要に応じて、近くにある&#x200B;__場所__&#x200B;を選択します。
1. プロビジョニング要求が正しいかどうかを確認し、「__作成__」ボタンをタップします（正しく設定されている場合）

### Amazon S3{#amazon-s3}

このチュートリアルを完了するには、[Microsoft Azure Blob Storage](#azure-blob-storage)を使用することをお勧めしますが、[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)も使用できます。

Amazon S3ストレージを使用する場合は、[プロジェクトの環境変数](../develop/environment-variables.md#amazon-s3)を設定する際に、Amazon S3クラウドストレージの資格情報を指定します。

このチュートリアル用にクラウドストレージを特別にプロビジョニングする必要がある場合は、[Azure Blob Storage](#azure-blob-storage)を使用することをお勧めします。
