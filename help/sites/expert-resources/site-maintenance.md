---
title: 日常のサイトメンテナンスガイド
description: 管理者、作成者、開発者のどれに該当しようと、サイトメンテナンスでは、AEM Sites インスタンスのあらゆる側面に触れます。このガイドを使用すると、成功に向けた戦略を確実に設定することができます。
role: Admin
level: Beginner, Intermediate
topic: Administration
feature: Learn From Your Peers
jira: KT-14255
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
duration: 255
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '998'
ht-degree: 100%

---

# サイトメンテナンスのヒントとテクニック

AEM インスタンスのインストールとメンテナンスに関しては、次の 3 つの選択肢があります。

* AEMaaCS（クラウドサービス）：システムは常に稼動し、最新の状態に維持され、必要に応じて規模が動的に拡張されます。
* Adobe Managed Services：アドビカスタマーサービスエンジニアが日次／週次／月次のすべてのメンテナンスを行い、すべてのサービスパックがインストールされ、システムのセキュリティが常に確保されシステムが円滑に動作します。
* オンプレミスで実行：バックアップ、アップグレードおよびセキュリティを含むシステム全体を管理する必要があrります。

独自のシステムをオンプレミスで実装する場合は、セキュリティで保護されたパフォーマンスの高いシステムを確保するために、次の点に注意してください。ここでは、「ケアとフィード」の項目に加えて、システムの正常な動作を維持するために、AEM 開発者が念頭に置くべきいくつかの項目についても説明します。

## 管理者

バックアップ：頻繁なスケジュールで完全なバックアップと部分的なバックアップを確実に行います。

* 毎日
* 毎週
* 毎月

多くのお客様はスナップショットバックアップを実行します。基盤となるオペレーティングシステムがこのようなバックアップをサポートしていると仮定すると、このバックアップは数分で済みます。これらのバックアップが（AEM システムから）正しく保存されていることを確認します。バックアップが機能し、作業システムを定期的に再作成するために使用できることを確認します。システムがクラッシュしたときにバックアップが何らかの理由で破損しているような場合ほど悪い状況はありません。

問題のない運用を確保するために監視すべき項目がいくつかあります。

### 定期メンテナンス

#### [インデックスメンテナンス](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=ja)

インデックスを使用すると、クエリをできるだけ早く実行し、他の操作のためにリソースを解放できます。インデックスが最高の状態になっていることを確認します。AEM では、インデックスを使用する代わりにトラバースするクエリをキャンセルして、1 つの不適切なクエリが AEM の全体的なパフォーマンスに影響を与えないようにします。

#### [Tar コンパクション／リビジョンクリーンアップ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=ja)

リポジトリが更新されるたびに、新しいコンテンツのリビジョンが作成されます。その結果、更新のたびにリポジトリのサイズが大きくなります。リポジトリのサイズが無制限に増大しないように、古いリビジョンをクリーンアップして、ディスクリソースを解放する必要があります。

#### [Lucene バイナリクリーンアップ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html?lang=ja#automated-maintenance-tasks)

Lucene バイナリをパージし、実行中のデータストアのサイズ要件を減らします。

#### [データストアガベージ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html?lang=ja)

AEM のアセットが削除されると、基になっているデータストアレコードへの参照がノード階層から削除される場合がありますが、データストアレコード自体は残ります。この参照されないデータストアレコードは、保持する必要のない「ガベージ」になります。参照されていないアセットが多数存在する場合は、それらを削除してスペースを確保し、バックアップを最適化し、ファイルシステムのメンテナンスパフォーマンスを向上させると効果的です。

#### [ワークフローパージ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=ja)

ワークフローインスタンスの数を最小限に抑えるとワークフローエンジンのパフォーマンスが向上します。このため、完了したまたは実行中のワークフローインスタンスをリポジトリから定期的に削除できます。

#### [監査ログのメンテナンス]（https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html?lang=ja）

監査ログの対象となる AEM イベントが発生すると、多くのアーカイブデータが生成されます。このデータは、レプリケーション、アセットのアップロード、およびその他のシステムアクティビティによって、短期間で増大する可能性があります。

#### [セキュリティ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=ja)

AEM の最も安全なインスタンスを確保するために、セキュリティチェックリストのベストプラクティスに厳密に従っていることを確認します。

#### ディスク領域

ディスク領域を監視して、JCR リポジトリに十分な容量とさらに約半分の容量を確保します。Tar コンパクションは、実行時に余分な容量を使用します。JCR が破損する第一の理由は、ディスク容量の不足です。

## デベロッパー

カスタムコンポーネントを使用しないようにし、[コアコンポーネント](https://www.aemcomponents.dev/)を使用します。目標としては、時間の 80～90％はコアコンポーネントを使用し、カスタムコンポーネントはごく控えめに使用します。これには、多くの場合、ページ上のコンポーネントを確認する新しい方法が必要です。フロントエンド開発者が CSS を使用してコンポーネントのスタイルを簡単に変更できることに気づく必要があります。また、これらのコアコンポーネントを相互に埋め込むことで、かなり複雑な結果を得ることができます。クリエイティブになりましょう。

### [スタイルシステム](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=ja)

スタイルシステムを使用すると、コアコンポーネントだけでなくカスタムコンポーネントでさえも、作成者の裁量でルックアンドフィールを変更して、まったく新しい外観のコンポーネントを作成できます。このようなスタイルの変更には、通常、フロントエンドデザイナーと知識のある作成者（「スーパーオーサー」と呼ばれることが多い）のみが関与します。

### [ローンチ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=ja)

ローンチを使用すると、現在デプロイされているページに影響を与えることなく、新しいプロモーション、販売または web サイトのロールアウトの作業を完了できます。また、担当者がその場に臨んだり監督したりせずに自動的に運用を開始するようにスケジュールでき、作成者が来週の（または次の四半期の）作業を今日行うことができ、運用開始の前日にページ開発に躍起になる必要はありません。それはまさしく時間のプレゼントです。

### [コンテンツフラグメント](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html?lang=ja)

コンテンツフラグメントは、サイト全体で簡単に再利用できるカスタマイズ可能な「情報のチャンク」です。変更が必要な場合は、元のチャンクを変更するだけで、使用されているすべての場所に更新がすぐに反映されます。

### [エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ja)

コンテンツフラグメントとほとんど同じように聞こえますが、エクスペリエンスフラグメントは、ページの小さい目に見える部分です。また、これらはサイト全体で広く再利用でき、AEM 内で一元的に維持管理できるので、数日や数週間ではなく数秒でサイト全体に潜在的にグローバルな変更を容易に加えることができます。

何が再利用されることになるかを、前もって考えてみてください。フッターでしょうか。免責事項でしょうか。ヘッダーでしょうか。それとも、特定のタイプのコンテンツでしょうか。メンテナンスを最小限に抑えながら、これらすべてをサイト全体で共有できます。免責事項の日付を更新する必要がありますが、それはサイトの 1,000 ページにわたって存在しますか。エクスペリエンスフラグメントを使用した場合は、5 秒で済む操作です。

## 一般

継続的な学習を通じて AEM の変更に遅れないようにしてください。過去にとらわれないでください。[Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=ja) と [アドビデジタルラーニングサービス（ADLS）](https://learning.adobe.com/)を利用してスキルを磨いてください。

## まとめ

AEM は大規模なシステムになる場合があり、稼働させるには多くのタイプの人が必要です。管理者から開発者（フロントエンド開発者とハードコア Java 開発者の両方）、さらには作成者まで、すべての人にとって重要なことがきっと何かあるはずです。また、日々の管理に気乗りがしない場合は、AMS や AEM as a Cloud Service をいつでも利用できます。
