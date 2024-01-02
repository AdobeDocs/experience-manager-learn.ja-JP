---
title: AEM Eventing
description: AEMのイベンティング、その概要、使用する理由、タイミング、その例について説明します。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: 839d552199fe7d10a0cde4011bdfe8cf42cc8ec9
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 0%

---


# AEM Eventing

AEMのイベンティング、その概要、使用する理由、タイミング、その例について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>AEMas a Cloud Serviceイベントは、プレリリースモードで登録ユーザーのみが使用できます。 AEMのas a Cloud Service環境でAEMイベンティングを有効にするには、 [AEM-Eventing チーム](mailto:grp-aem-events@adobe.com).

## 内容

AEM Eventing は、AEM Events のサブスクリプションを外部システムで処理できるようにする、クラウドネイティブなイベンティングシステムです。 AEMイベントは、特定のアクションが発生したたびにAEMによって送信される状態変更通知です。 例えば、コンテンツフラグメントが作成、更新または削除されたときのイベントを含めることができます。

![AEM Eventing](./assets/aem-eventing.png)

上の図では、AEMas a Cloud Serviceがイベントを生成してAdobe I/Oイベントに送信する方法を視覚化し、イベント購読者に公開しています。

要約すると、次の 3 つの主要なコンポーネントがあります。

1. **イベントプロバイダー：** AEMas a Cloud Service。
1. **Adobe I/Oイベント：** Adobeの製品およびテクノロジーに基づいてアプリとエクスペリエンスを統合、拡張および構築するための開発者プラットフォーム。
1. **イベントコンシューマー：** AEMイベントを購読する顧客が所有するシステム。 たとえば、CRM（顧客関係管理）、PIM（製品情報管理）、OMS（受注管理システム）、カスタム・アプリケーションなどです。

### 相違点

The [Apache Sling eventing](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)、OSGi イベンティングおよび [JCR の監視](https://jackrabbit.apache.org/oak/docs/features/observation.html) すべてのオファーメカニズムで、イベントを購読および処理できます。 ただし、これらはこのドキュメントで説明するAEM Eventing とは異なります。

AEM Eventing の主な違いは次のとおりです。

- イベントコンシューマーコードは、AEMと同じ JVM で実行されず、AEMの外部で実行されます。
- AEM製品コードは、イベントの定義とイベントイベントイベントへの送信をAdobe I/Oします。
- イベント情報は標準化され、JSON 形式で送信されます。 詳しくは、 [cloudevents](https://cloudevents.io/).
- イベントコンシューマーは、AEMに再度通信するために、AEMas a Cloud ServiceAPI を使用します。


## 使用する理由とタイミング

AEM Eventing は、システムアーキテクチャと運用効率に多数の利点を提供します。 AEM Eventing を使用する主な理由は次のとおりです。

- **イベント・ドリブン・アーキテクチャを構築する**：独立して拡張でき、障害に対して回復性の高い、疎結合システムの作成を容易にします。
- **低コード化と運用コストの削減**:AEM内のカスタマイズを回避し、システムの保守と拡張が容易になり、運用コストを削減します。
- **AEMと外部システム間の通信をシンプル化**：特定のシステムやサービスに配信するAEMイベントを決定するなど、Adobe I/Oイベントが通信を管理することで、ポイントツーポイント接続を排除します。
- **イベントの耐久性の向上**:Adobe I/Oイベントは、高い可用性と拡張性を備えたシステムで、大量のイベントを処理し、購読者に確実に配信するように設計されています。
- **イベントの並列処理**：複数の購読者に同時にイベントを配信でき、様々なシステム間でイベント処理を分散させます。
- **サーバーレスアプリケーションの開発**：イベントコンシューマーコードのサーバーレスアプリケーションとしてのデプロイをサポートし、システムの柔軟性と拡張性をさらに強化します。

### 制限事項

AEM Eventing には、強力な機能ですが、考慮すべきいくつかの制限があります。

- **可用性はAEM as a Cloud Serviceに制限されています**：現在、AEM Eventing は、AEM as a Cloud Serviceでのみ使用できます。
- **限定的なイベントのサポート**：現時点では、AEMコンテンツフラグメントイベントのみがサポートされています。 ただし、今後、さらに多くのイベントが追加されるにつれ、範囲は拡大すると予想されます。

## 有効にする方法

AEM Eventing は、AEMのas a Cloud Service環境ごとに有効になり、プレリリースモードの環境でのみ使用できます。 連絡先 [AEM-Eventing チーム](mailto:grp-aem-events@adobe.com) AEM Eventing でAEM環境を有効にする場合。

既に有効になっている場合は、 [AEM Cloud Service環境でのAEMイベントの有効化](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) を参照してください。

## 購読方法

AEM Events をサブスクライブする場合、AEMでコードを記述する必要はなく、 [Adobe Developer Console](https://developer.adobe.com/) プロジェクトが設定されました。 Adobe Developerコンソールは、AdobeAPI、SDK、イベント、ランタイムおよび App Builder へのゲートウェイです。

この場合、 _プロジェクト_ Adobe Developerコンソールでは、AEM as a Cloud Service環境から発生するイベントをサブスクライブし、外部システムへのイベント配信を設定できます。

詳しくは、 [Adobe DeveloperコンソールでAEMイベントを購読する方法](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 消費方法

AEMイベントを使用する主な方法は次の 2 つです。 _プッシュ_ メソッドおよび _取る_ メソッド。

- **Push メソッド**：このアプローチでは、イベントが利用可能になると、Adobe I/Oイベントによってイベントコンシューマーが事前に通知を受けます。 統合オプションには、Webhook、Adobe I/O Runtime、Amazon EventBridge が含まれます。
- **プルメソッド**：ここでは、イベントコンシューマーは、Adobe I/Oイベントを積極的にポーリングして、新しいイベントがないかどうかを確認します。 このメソッドの主な統合オプションは、Adobe I/Oジャーナリング API です。

詳しくは、 [AEMイベントを介したAdobe I/O処理](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## 例

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Webhook でのAEMイベントの受信" src="./assets/examples/webhook/Eventing-webhook.png"/></a>
        <div><strong><a href="./examples/webhook.md">Webhook でのAEMイベントの受信</a></strong></div>
        <p>
          Adobeが提供する Webhook を使用してAEMイベントを受け取り、イベントの詳細を確認します。
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM Events ジャーナルを読み込み" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">AEM Events ジャーナルを読み込み</a></strong></div>
        <p>
          Adobeが提供する Web アプリケーションを使用して、ジャーナルからAEMイベントを読み込み、イベントの詳細を確認します。
        </p>
      </td>
    </tr>
</table>
