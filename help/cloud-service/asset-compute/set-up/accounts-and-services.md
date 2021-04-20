---
title: asset computeの拡張用のアカウントとサービスの設定
description: asset computeワーカーの開発では、AEMを含むアカウントやサービス(Cloud Service、AdobeプロジェクトのFirefly、MicrosoftまたはAmazonが提供するクラウドストレージ)にアクセスする必要があります。
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 2%

---


# アカウントとサービスの設定

このチュートリアルでは、プロビジョニングを行い、学習者のAdobe IDからアクセスできる次のサービスを使用する必要があります。

すべてのAdobeサービスは、Adobe IDを使用して、同じAdobe組織を通じてアクセスできる必要があります。

+ [Cloud ServiceとしてのAEM](#aem-as-a-cloud-service)
+ [AdobeプロジェクトFireFly](#adobe-project-firefly)
   + プロビジョニングには2 ～ 10日かかる場合があります
+ クラウドストレージ
   + [Azure Blobストレージ](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + または[AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>このチュートリアルを続ける前に、前述のすべてのサービスにアクセスできることを確認してください。
> 
> 必要なサービスを設定およびプロビジョニングする方法について、以下の節を確認します。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

AEM Assets処理プロファイルがカスタムAsset computeワーカーを呼び出すように設定するには、Cloud Service環境としてAEMにアクセスする必要があります。

サンドボックスプログラムまたはサンドボックス以外の開発環境を使用できるのが理想的です。

ローカルAEM SDKは、このチュートリアルを完了するのに十分ではありません。ローカルのAEM SDKは、Asset computeのマイクロサービスと通信できないので、Cloud Service環境としてのtrue AEMが必要です。

## Adobeプロジェクトの蛍光{#adobe-project-firefly}

[AdobeプロジェクトFirefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)フレームワークは、AdobeのサーバーレスプラットフォームであるAdobe I/O Runtimeにカスタムアクションを作成し、導入するために使用されます。 AEMAsset computeプロジェクトは、処理プロファイルを介してAEM Assetsと統合し、アセットバイナリにアクセスして処理する機能を提供する、特別に構築されたFireflyプロジェクトです。

Project Fireflyにアクセスするには、プレビューにサインアップします。

1. [Project Fireflyプレビューにサインアップします](https://adobeio.typeform.com/to/obqgRm)。
1. プロビジョニングが完了したことを電子メールで通知されるまで、約2 ～ 10日待ってから、チュートリアルに進みます。
   + プロビジョニングが完了しているかどうかが不明な場合は、次の手順に進みます。まだプロビジョニングされていない[Adobe開発者コンソール](https://console.adobe.io)に&#x200B;__プロジェクトのFirefly__&#x200B;プロジェクトを作成できない場合は、次の手順に進みます。

## クラウドストレージ

クラウドストレージは、Asset computeプロジェクトのローカル開発に必要です。

AEMがCloud Serviceとして直接使用するためにAsset computeワーカーをAdobe I/O Runtimeにデプロイする場合、このクラウドストレージは、アセットの読み取りとレンディションの書き込みを行うクラウドストレージをAEMが提供するので、厳密には必要ありません。

### Microsoft Azure Blob Storage{#azure-blob-storage}

Microsoft Azure Blobストレージへのアクセス権をまだ持っていない場合は、[無料の12ヶ月のアカウント](https://azure.microsoft.com/en-us/free/)にサインアップします。

このチュートリアルではAzure Blobストレージを使用しますが、[AmazonS3](#amazon-s3)は、チュートリアルに対する小さなバリエーションのみでも使用できます。

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Azure Blobストレージのプロビジョニングのクリックスルー（オーディオなし）_


1. [Microsoft Azureアカウント](https://azure.microsoft.com/en-us/account/)にログインします。
1. __ストレージアカウント__ Azureサービスセクションに移動します
1. __+ 追加__&#x200B;をタップして、新しいBLOBストレージアカウントを作成します
1. 必要に応じて、新しい&#x200B;__リソースグループ__&#x200B;を作成します。例：`aem-as-a-cloud-service`
1. __ストレージアカウント名__&#x200B;を指定します。例：`aemguideswkndassetcomput`
   + __ストレージアカウント名__&#x200B;は、ローカルAsset compute開発ツールの[クラウドストレージ](../develop/environment-variables.md)の設定に使用されます
   + [クラウドストレージ](../develop/environment-variables.md)を設定する際にも、ストレージアカウントに関連付けられた&#x200B;__アクセスキー__&#x200B;が必要です。
1. その他はすべてデフォルトのままにし、__「レビュー」+「作成」__&#x200B;ボタンをタップします
   + オプションで、身近な&#x200B;__場所__&#x200B;を選択します。
1. プロビジョニングの正確性の要求を確認し、満たされた場合は「__作成__」ボタンをタップします

### Amazon S3{#amazon-s3}

このチュートリアルを完了するには、[Microsoft Azure Blobストレージ](#azure-blob-storage)を使用することをお勧めしますが、[AmazonS3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)も使用できます。

AmazonS3ストレージを使用する場合、[プロジェクトの環境変数](../develop/environment-variables.md#amazon-s3)を設定する際に、AmazonS3クラウドストレージ資格情報を指定します。

このチュートリアルでクラウドストレージを特別にプロビジョニングする必要がある場合は、[Azure Blobストレージ](#azure-blob-storage)を使用することをお勧めします。
