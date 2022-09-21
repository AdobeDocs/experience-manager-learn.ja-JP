---
title: サイトメンテナンスに関するヒントとテクニック
description: サイトメンテナンスに関するヒントとテクニック
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 9%

---


# サイトメンテナンスに関するヒントとテクニック

AEMインスタンスのインストールと保守には、次の 3 つのオプションがあります

* AEMaaCS（クラウドサービス） — システムは常に稼動し、最新の状態に維持され、必要に応じて動的に拡張されます。
* Adobeカスタマーサービスエンジニアが日次/週次/月次のすべてのメンテナンスをおこない、すべてのサービスパックがインストールされ、システムのセキュリティが常に確保され、スムーズに動作する Adobe Managed Services
* オンプレミスで実行し、バックアップ、アップグレード、セキュリティを含むシステム全体を管理する必要がある。

独自のシステムをオンプレミスで実装する場合は、セキュリティで保護されたパフォーマンスの高いシステムを確保するために、次の点に注意してください。 この論文では、「ケアと給餌」の項目に加えて、システムの正常な動作を維持するために、AEM Developers が念頭に置くべきいくつかの項目についても説明します。

## 管理者

バックアップ：頻繁なスケジュールで完全なバックアップと部分的なバックアップを確実に行う。

* 毎日
* 毎週
* 毎月

多くのお客様はスナップショット・バックアップを実行します。基盤となるオペレーティング・システムがこのようなバックアップをサポートしていると想定すると、数分で済みます。 これらのバックアップが (AEMシステムから ) 正しく保存されていることを確認します。 バックアップが機能し、作業システムを定期的に再作成するために使用できることを確認します。何らかの理由でシステムがクラッシュし、バックアップが破損している場合に比べて、問題はありません。

問題のない動作を確認するために、監視する必要がある項目がいくつかあります。

### 定期メンテナンス

#### [指数維持](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=ja)

インデックスを使用すると、クエリをできるだけ早く実行し、他の操作のリソースを解放できます。 インデックスが先端の形状になっていることを確認します。 AEMは、インデックスを使用する代わりに実行されたクエリをキャンセルして、AEM全体のパフォーマンスに影響を与えないように、1 つの不良なクエリを保持します。

#### [Tar コンパクション/リビジョンクリーンアップ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

リポジトリが更新されるたびに、新しいコンテンツのリビジョンが作成されます。その結果、更新のたびにリポジトリのサイズが大きくなります。制御されないリポジトリの増加を回避するには、古いリビジョンをクリーンアップして、ディスクリソースを解放する必要があります。

#### [Lucene バイナリクリーンアップ](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-dashboard.html?lang=en#automated-maintenance-tasks)

Lucene バイナリをパージし、実行中のデータストアのサイズ要件を減らします。

#### [データストアのガベージ](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/data-store-garbage-collection.html?lang=en)

AEM内のアセットを削除すると、基になるデータストアレコードへの参照がノード階層から削除される場合がありますが、データストアレコード自体は残ります。 この未参照のデータストアレコードは、保持する必要のない「ガベージ」になります。 参照されていないアセットが多数存在する場合は、それらを削除し、領域を保持し、バックアップを最適化し、ファイルシステムのメンテナンスパフォーマンスを最適化すると便利です。

#### [ワークフローのパージ](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/workflows-administering.html?lang=en)

ワークフローインスタンスの数を最小限に抑えるとワークフローエンジンのパフォーマンスが向上します。このため、完了したまたは実行中のワークフローインスタンスをリポジトリーから定期的に削除できます。

#### [監査ログのメンテナンス](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-audit-log.html?lang=en)

監査ログの対象となるAEMイベントは、多数のアーカイブデータを生成します。 このデータは、レプリケーション、アセットのアップロード、およびその他のシステムアクティビティによって、短期間で増大する可能性があります。

#### [セキュリティ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=ja)

AEMの最も安全なインスタンスを確実に保証するために、セキュリティチェックリストのベストプラクティスに厳密に従っていることを確認します。

#### Diskspace

ディスク領域を監視して、JCR リポジトリに十分な容量と、約半分の容量を確保します。tar 圧縮は、実行時に余分な容量を使用します。 JCR が破損する 1 つ目の理由は、ディスク容量が不足していることです。

## デベロッパー

カスタムコンポーネントを使用しないようにします。 [コアコンポーネント](https://www.aemcomponents.dev/). 目標は、時間の 80～90%のコアコンポーネントと、カスタムコンポーネントを慎重に使用することに限られます。 これには、多くの場合、ページ上のコンポーネントを新しく確認する方法が必要です。フロントエンド開発者が CSS を使用してコンポーネントのスタイルを簡単に変更できることに気付く必要があります。 また、これらのコアコンポーネントを相互に埋め込むことで、非常に複雑な結果を得ることができます。 創造力を！

### [スタイルシステム](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

スタイルシステムを使用すると、コアコンポーネントやカスタムコンポーネントでも、作成者の裁量によって外観と操作性を変更し、まったく新しい外観のコンポーネントを作成できます。 このようなスタイルの変更は、通常、フロントエンドのデザイナーと知識のある作成者（「スーパーオーサー」と呼ばれることが多い）のみを伴います。

### [ローンチ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

ローンチを使用すると、現在デプロイされているページに影響を与えることなく、新しいプロモーション、販売または Web サイトのロールアウトで作業を完了できます。 また、出席や監督なしで、自動的にライブにするようにスケジュールでき、作成者は来週の（または次の四半期の）作業を今日行うことができ、ライブの前日にページ開発に急ぐ必要はありません。

### [コンテンツフラグメント](https://experienceleague.adobe.com/docs/experience-manager-64/assets/fragments/content-fragments.html?lang=en)

コンテンツフラグメントは、カスタマイズ可能な「情報のチャンク」で、サイト全体で簡単に再利用できます。 変更が必要な場合は、元のチャンクを変更するだけで、更新が使用されているすべての場所に表示されます。

### [エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

コンテンツフラグメントとほとんど同じように聞こえますが、エクスペリエンスフラグメントは、ページの小さく、表示可能な部分です。 また、これらはサイト全体で広く再利用し、AEM内の中央の場所に保持することで、日や週ではなく、サイト全体で潜在的にグローバルな変更を数秒でおこなうタスクを容易にします。

何が再利用されるかを考えてみてください。 フッター？ 免責事項？ ヘッダー？ 特定のタイプのコンテンツ メンテナンスを最小限に抑えながら、これらすべてをサイト全体で共有できます。 免責事項の日付を更新する必要がありますが、サイトの 1,000 ページにありますか？ エクスペリエンスフラグメントを使用した場合は、5 秒の操作です。

## 一般

継続的な学習を通じてAEMの変化に気を付けなさい。過去に行き詰まることは避けてください。 用途 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) および [Adobeデジタルラーニングサービス (ADLS)](https://learning.adobe.com/) 自分の技術を磨くために

## まとめ

AEMは大規模なシステムである場合があり、「歌う」のに多くのタイプの人が必要です。 管理者から開発者（フロントエンドとハードコアの Java 開発者の両方）、作成者まで — 誰にとっても役に立つ機能があります。 日々の管理を行う気がない場合は、常に AMS とAEMがas a Cloud Serviceしています。
