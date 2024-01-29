---
title: 自動フォーム変換サービス
description: 自動フォーム変換サービス. この記事では、AEM の管理者が自動フォーム変換サービスを設定して、PDF フォームを自動的にアダプティブフォームに変換する方法について説明します。このヘルプ記事は、組織内の IT 管理者と AEM 管理者を対象としています。
feature: Adaptive Forms
thumbnail: 39493.jpg
jira: KT-6114
topic: Development
role: Admin
level: Beginner
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 0715a2cc-c042-4ddc-85a1-7720f420351b
duration: 582
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '267'
ht-degree: 100%

---

# 自動フォーム変換サービス

この記事では、AEM の管理者が自動フォーム変換サービスを設定して、PDF フォームを自動的にアダプティブフォームに変換する方法について説明します。このヘルプ記事は、組織内の IT 管理者と AEM 管理者を対象としています。具体的には、以下の操作に関する十分な知識があるユーザーを対象としています。

* Adobe Experience Manager パッケージと AEM パッケージのインストール、設定、管理
* Linux オペレーティングシステムと Microsoft Windows オペレーティングシステムの使用
* SMTP メールサーバーの設定

## 前提条件：

自動フォーム変換サービスを使用するには、以下の条件を満たしている必要があります。

* 組織内で自動フォーム変換サービスが有効になっていること
* 変換サービス用の管理者権限が設定された Adobe ID アカウントが作成されていること
* 最新の AEM サービスパックが付属した AEM 6.4 オーサーインスタンスまたは AEM 6.5 オーサーインスタンスが稼働していること
* AEM インスタンス上の AEM ユーザーが forms-user グループのメンバーになっていること

>[!NOTE]
>アドビによる組織と管理者に対する権限の設定が完了したら、管理者は次のビデオで説明されている手順に従って、Admin Console にログインしてプロファイルを作成し、プロファイルに開発者を追加することができます。開発者は、ローカルの AEM Forms インスタンスを、Adobe Cloud 上の自動フォーム変換サービスに接続する必要があります。

* このビデオでは、ローカルの AEM Forms インスタンスを Adobe Cloud 上で自動フォーム変換サービスに接続するために必要な手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/39493?quality=12&learn=on)

## 次の手順

[PDF フォームからアダプティブフォームへの変換](./convert-pdf-form-into-adaptive-form.md)