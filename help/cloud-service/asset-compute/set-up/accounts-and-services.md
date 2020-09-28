---
title: アセット計算の拡張機能用のアカウントとサービスの設定
description: アセットコンピューティングワーカーの開発には、AEMを含むアカウントやサービス(Cloud Serviceとして)、AdobeプロジェクトのFirefly、およびMicrosoftやAmazonが提供するクラウドストレージにアクセスする必要があります。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 2%

---


# アカウントとサービスの設定

このチュートリアルでは、プロビジョニングを行い、学習者のAdobe IDからアクセスできる次のサービスを使用する必要があります。

すべてのAdobeサービスは、Adobe IDを使用して、同じAdobe組織を通じてアクセスできる必要があります。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [AdobeプロジェクトFireFly](#adobe-project-firefly)
   + プロビジョニングには2 ～ 10日かかる場合があります
+ クラウドストレージ
   + [Azure Blobストレージ](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + または [AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>このチュートリアルを続ける前に、前述のすべてのサービスにアクセスできることを確認してください。
> 
> 必要なサービスを設定およびプロビジョニングする方法について、以下の節を確認します。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

AEM Assets処理プロファイルを設定してカスタムのAsset Computeアプリケーションを呼び出すには、Cloud Service環境としてAEMにアクセスする必要があります。

サンドボックスプログラムまたはサンドボックス以外の開発環境を使用できるのが理想的です。

ローカルAEM SDKは、このチュートリアルを完了するのに十分ではありません。ローカルのAEM SDKは、Asset Computeマイクロサービスと通信できず、代わりに、Cloud Service環境としての真のAEMが必要です。

## Adobeプロジェクトの蛍{#adobe-project-firefly}

Adobe [プロジェクトのFirefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) Frameworkは、AdobeのサーバーレスプラットフォームであるAdobe I/O Runtimeにカスタムアプリケーションを構築し、導入するために使用されます。 AEM Asset Computeアプリケーションは、処理プロファイルを介してAEM Assetsに統合され、アセットバイナリにアクセスして処理する機能を提供する、特別に構築されたFireflyアプリケーションです。

Project Fireflyにアクセスするには、プレビューにサインアップします。

1. [Project Fireflyプレビューにサインアップします](https://adobeio.typeform.com/to/obqgRm)。
1. プロビジョニングが完了したことを電子メールで通知されるまで、約2 ～ 10日待ってから、チュートリアルに進みます。
   + プロビジョニングが完了しているかどうかが不明な場合は、次の手順に進みます。まだプロビジョニングされていない __Adobeデベロッパーコンソールで__ プロジェクトFirefly [プロジェクトを作成できない場合は](https://console.adobe.io) 、次の手順に進みます。

## クラウドストレージ

クラウドストレージは、Asset Computeアプリケーションのローカル開発に必要です。

AEMがCloud Serviceとして直接使用するためにAsset ComputeアプリケーションをAdobe I/O Runtimeにデプロイする場合、AEMはアセットの読み取りとレンディションを行うクラウドストレージを提供するので、このクラウドストレージは厳密には必要ありません。

### Microsoft Azure Blob Storage{#azure-blob-storage}

Microsoft Azure Blobストレージへのアクセス権をまだ持っていない場合は、12か月 [無料のアカウントにサインアップ](https://azure.microsoft.com/en-us/free/)します。

このチュートリアルではAzure Blobストレージを使用しますが、 [AmazonS3](#amazon-s3) はチュートリアルに対する小さなバリエーションのみでも使用できます。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)
_Azure Blobストレージのプロビジョニングのクリックスルー（オーディオなし）_


1. [Microsoft Azureアカウントにログインします](https://azure.microsoft.com/en-us/account/)。
1. [ __ストレージアカウント__ ] [Azureサービス]セクションに移動します
1. 「 ____ +追加」をタップして、新しいBLOBストレージアカウントを作成します
1. 必要に応じて、新しい __リソースグループを作成します__ 。例： `aem-as-a-cloud-service`
1. 次のように、 __ストレージアカウント名を入力します__。 `aemguideswkndassetcomput`
   + この __ストレージアカウント名__ は、ローカルのAsset Compute Development Toolのクラウドストレージ [(Cloud Development)の](../develop/environment-variables.md) 設定に使用されます。
   + クラウドストレージの __設定時には、ストレージアカウントに関連付けられた__ アクセスキー [も必要です](../develop/environment-variables.md)。
1. その他はすべてデフォルトのままにし、「 __レビュー__ 」ボタンと「作成」ボタンをタップします。
   + 必要に応じて、 __身近な場所を選択します__ 。
1. プロビジョニングの正確性の要求を確認し、満たされた場合は「 __作成__ 」ボタンをタップします

### Amazon S3{#amazon-s3}

このチュートリアルを完了するには、 [Microsoft Azure Blobストレージを使用することをお勧めします](#azure-blob-storage) 。ただし、 [AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) も使用できます。

AmazonS3ストレージを使用する場合は、プロジェクトの環境変数を [設定する際に、AmazonS3クラウドストレージ資格情報を指定し](../develop/environment-variables.md#amazon-s3)ます。

このチュートリアル用にクラウドストレージを特別にプロビジョニングする必要がある場合は、 [Azure Blobストレージを使用することをお勧めします](#azure-blob-storage)。
