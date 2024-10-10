---
title: Asset Compute 拡張機能のアカウントとサービスの設定
description: Asset Compute ワーカーの開発には、AEM as a Cloud Service、Application Builder、Microsoft や Amazon が提供するクラウドストレージなどのアカウントおよびサービスにアクセスできる必要があります。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '592'
ht-degree: 100%

---

# アカウントとサービスの設定

このチュートリアルでは、以下のサービスがプロビジョニングされ、学習者の Adobe ID でアクセスできるようになっている必要があります。

すべてのアドビサービスは、Adobe ID を使用して同じアドビ組織からアクセスできる必要があります。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Application Builder](#app-builder)
   + プロビジョニングには 2～10 日かかる場合があります。
+ クラウドストレージ
   + [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + または [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>このチュートリアルを続行する前に、前述のすべてのサービスにアクセスできることを確認してください。
> 
> 必要なサービスの設定とプロビジョニングの方法については、以下の節を参照してください。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

カスタムの Asset Compute ワーカーを呼び出すように AEM Assets の処理プロファイルを設定するには、AEM as a Cloud Service 環境へのアクセスが必要です。

サンドボックスプログラムまたはサンドボックス以外の開発環境を使用できれば理想的です。

なお、ローカル AEM SDK は Asset Compute マイクロサービスと通信できないので、このチュートリアルを完了するには不十分です。代わりに、本物の AEM as a Cloud Service 環境が必要です。

## App Builder{#app-builder}

この [Application Builder](https://developer.adobe.com/app-builder/) フレームワークは、カスタムアクションを作成して、アドビのサーバーレスプラットフォームである Adobe I/O Runtime にデプロイするために使用します。AEM Asset Compute プロジェクトは特別に作成された Application Builder プロジェクトで、処理プロファイルを通じて AEM Assets と統合され、アセットバイナリへのアクセスと処理を可能にします。

Application Builder にアクセスするには、プレビューにサインアップします。

1. [App Builder 体験版にサインアップします](https://developer.adobe.com/app-builder/trial/)。
1. プロビジョニングが完了したことを通知するメールが届くまで約 2～10 日待ってから、チュートリアルを続行します。
   + プロビジョニングされているかどうかわからない場合は次の手順に進みます。 [Adobe Developer Console](https://developer.adobe.com/console/) で __App Builder__ プロジェクトを作成できない場合は、まだプロビジョニングされていません。

## クラウドストレージ

Asset Compute プロジェクトのローカル開発には、クラウドストレージが必要です。

AEM as a Cloud Service で直接使用できるように Asset Compute ワーカーが Adobe I/O Runtime にデプロイされている場合は、アセットの読み取り先とレンディションの書き込み先となるクラウドストレージが AEM から提供されるので、このクラウドストレージは厳密には必要ではありません。

### Microsoft Azure Blob Storage{#azure-blob-storage}

Microsoft Azure Blob Storage へのアクセス権をまだ持っていない場合は、[12 か月無料のアカウント](https://azure.microsoft.com/ja-jp/free/)にサインアップします。

このチュートリアルでは Azure Blob Storage を使用しますが、[Amazon S3](#amazon-s3) も同様に使用できます（チュートリアルの内容がわずかに変わるだけです）。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Azure Blob Storage のプロビジョニングのクリックスルー（オーディオなし）_

1. [Microsoft Azure アカウント](https://azure.microsoft.com/ja-jp/account/)にログインします。 
1.  __ストレージアカウント__&#x200B;の「Azure サービス」セクションに移動します。
1. 「__+ 追加__」をタップして、 新しい Blob Storage アカウントを作成します。
1. 必要に応じて、`aem-as-a-cloud-service` などの新しい&#x200B;__リソースグループ__&#x200B;を作成します。 
1.  `aemguideswkndassetcomput` などの&#x200B;__ストレージアカウント名__&#x200B;を指定します。
   + ローカルの Asset Compute 開発ツールで[クラウドストレージの設定](../develop/environment-variables.md)に使用される&#x200B;__ストレージアカウント名__
   + ストレージアカウントに関連付けられている&#x200B;__アクセスキー__&#x200B;も、[クラウドストレージの設定](../develop/environment-variables.md)時に必要になります。
1. それ以外の値はデフォルトのままにして、「__レビューと作成__」ボタンをタップします。
   + オプションで、近くの&#x200B;__場所__&#x200B;を選択します。
1. プロビジョニングリクエストが正しいかどうかを確認し、問題ない場合は「__作成__」ボタンをタップします。

### Amazon S3{#amazon-s3}

このチュートリアルを完了するには [Microsoft Azure Blob Storage](#azure-blob-storage) の使用をお勧めしますが、[Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) も使用できます。

Amazon S3 ストレージを使用する場合は、 [プロジェクトの環境変数の設定](../develop/environment-variables.md#amazon-s3)時に、Amazon S3 クラウドストレージの資格情報を指定します。

このチュートリアル用に特別にクラウドストレージをプロビジョニングする必要がある場合は、 [Azure Blob Storage](#azure-blob-storage) を使用することをお勧めします。
