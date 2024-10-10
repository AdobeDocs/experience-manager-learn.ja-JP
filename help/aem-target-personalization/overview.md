---
title: AEM と Adobe Target の使用の手引き
description: Adobe Experience Manager と Adobe Target を使用してパーソナライズされたエクスペリエンスを作成して配信する方法を説明するエンドツーエンドのチュートリアル。このチュートリアルでは、エンドツーエンドのプロセスに関わる様々なペルソナと、それらが相互にどう共同作業するかについて学びます
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '846'
ht-degree: 100%

---

# AEM Sites と Adobe Target の統合 {#getting-started-with-aem-target}

AEM と Target は、両方とも強力なソリューションで、機能が重複しているように見えます。 パーソナライズされたエクスペリエンスを提供するために、これらの製品をどのように、いつ組み合わせて使用すればよいかを理解するのに苦労する場合があります。 各エンドユーザーに最適化されたエクスペリエンスを提供するには、組織内の異なるチームが緊密に連携し、誰が何を行うかを定義する必要があります。

このチュートリアルでは、AEM と Target の 3 つの異なるシナリオについて説明します。これは、組織にとって最適な機能と、異なるチームが共同作業する方法を理解するのに役立ちます。

* シナリオ 1：AEM エクスペリエンスフラグメントを使用したパーソナライズ機能
* シナリオ 2：Visual Experience Composer を使用したパーソナライズ機能
* シナリオ 3：完全な web ページエクスペリエンスのパーソナライズ機能

## AEM エクスペリエンスフラグメントを使用したパーソナライズ機能 {#personalization-using-aem-experience-fragment}

このシナリオでは、AEM と Target を使用します。 両方の製品には明らかに独自の強みがあり、サイトのユーザーにてパーソナライズされたエクスペリエンスを提供するには、 **パーソナライズされたコンテンツ（AEM のコンテンツ）**&#x200B;と&#x200B;**インテリジェントな方法（Target）**&#x200B;を使用して、特定のユーザーに基づいてこれらのコンテンツを提供する必要があります。

AEM は、パーソナライズされたコンテンツを作成し、すべてのコンテンツとアセットを一元的に統合して、パーソナライズ機能戦略を強化するのに役立ちます。AEM を使用すると、コードを記述することなく、1 か所でデスクトップ、タブレット、モバイルデバイス用のコンテンツを簡単に作成できます。 デバイスごとにページを作成する必要はありません。AEM は、コンテンツを使用して各エクスペリエンスを自動的に調整します。 また、ボタンを押すだけで、コンテンツを AEM から Adobe Target にオファーとして書き出すこともできます。

これで、Target の AEM からのオファーの形式でコンテンツがパーソナライズされました。 Target では、行動、コンテキスト、オフラインの変数を組み込んだルールベースの手法と AI 駆動の機械学習の手法を組み合わせ、それに基づいてこれらのオファーを大規模に提供できます。  Target では、A/B テストや多変量分析（MVT） を簡単に設定して実行し、最適なオファー、コンテンツ、エクスペリエンスを見極めることができます。

**エクスペリエンスフラグメント** は、コンテンツ／エクスペリエンスの作成者を、Target を使用してビジネス成果を促進しているパーソナライズ機能の専門家にリンクするための大きな一歩となります。

* AEM コンテンツエディターは、エクスペリエンスフラグメントおよびそのバリエーションとしてパーソナライズされたコンテンツを作成します。
* AEM は Target にエクスペリエンスフラグメント HTML を書き出します
* Target は、AEM エクスペリエンスフラグメントのマークアップをアクティビティのオファーとして使用します
* Target はエクスペリエンスフラグメント HTML を配信し、AEM は参照画像を提供します

  ![エクスペリエンスフラグメントを使用したパーソナライゼーションの図](assets/personalization-use-case-1/use-case-1-diagram.png)

**このシナリオを実装するには、次の操作が必要です。**

* [タグとAdobe I/O を使用した AEM と Adobe Target の統合](./implementation.md#integrating-aem-target-options)
* [レガシーのクラウドサービスを使用する AEM と Adobe Target](./implementation.md#integrating-aem-target-options)

***上記の統合を実装したところで、今度は[詳細なシナリオ](./personalization-use-case-1.md)を見ていきましょう。***

## Visual Experience Composer を使用したパーソナライズ機能

Adobe Target Visual Experience Composer（VEC）を使用することで、マーケターはテストを実行するコードを変更しなくても、 web サイト上で素早く変更を加えることができます。  VEC は WYSIWYG（見ているとおりのものが得られる）ユーザーインターフェイスで、サイトコンテキストのパーソナライズされたエクスペリエンスやオファーを簡単に作成およびテストできます。 Web ページ（もしくはオファー）またはモバイル web ページのレイアウトとコンテンツをドラッグ＆ドロップ、入れ替えおよび変更することで、Target アクティビティのエクスペリエンスとオファーを作成できます。

VEC は Adobe Target の主な機能の 1 つです。 VEC を使用すると、マーケターやデザイナーが視覚的なインターフェイスを使用してコンテンツを作成および変更できます。 コードを直接編集する必要なく、多くのデザインの選択肢を作成できます。 コンポーザーで使用可能な編集オプションを使用して、HTML と JavaScript の編集も可能です。

* コンテンツは AEM に常駐し、コンテンツ編集者がサイトページを作成および管理します
* Target は、AEM でホストされるサイトページを使用して、テストとパーソナライズ機能を実行します
* Target はパーソナライズされたコンテンツを配信します
* Adobe Target VEC を使用して新しいコンテンツが作成されます
* AEM がホストするサイトと AEM 以外がホストするサイトの両方に適用されます

  ![Visual Experience Composer を使用したパーソナライズ機能の図](assets/personalization-use-case-3/use-case-diagram-3.png)

**このシナリオを実装するには、次の操作が必要です。**

* [タグとAdobe I/O を使用した AEM と Adobe Target の統合](./implementation.md#integrating-aem-target-options)

***上記の統合を実装したところで、ここからは[シナリオの詳細](./personalization-use-case-3.md)***&#x200B;を見ていきましょう

## 完全な web ページエクスペリエンスのパーソナライズ機能

Adobe Experience Manager と Adobe Target の統合は、サイトのユーザーにパーソナライズされたエクスペリエンスを提供するのに役立ちます。 また、指定したテスト期間に、web サイトコンテンツのどのバージョンが最もコンバージョンを向上させるのか理解を深めるのに役立ちます。例えば、A/B テストでは、複数のバージョンの web サイトコンテンツを比較して、特定したコンバージョン、売上高、その他の指標のうち最も高い上昇率を示す指標を確認します。 マーケターは、Adobe Target 内でアクティビティを作成し、ユーザーがサイトのコンテンツとどのように関わり、それがサイトの指標にどのような影響を与えるかを把握できます。

* コンテンツは AEM に常駐し、コンテンツ編集者がサイトページを作成および管理します
* Target は、AEM でホストされるサイトページを使用して、テストとパーソナライズ機能を実行します
* Target はパーソナライズされたコンテンツを配信します
* ここで新しいコンテンツは作成されません
* AEM サイトと非 AEM サイトの両方に適用されます

  ![図](assets/personalization-use-case-2/use-case-2-diagram.png)

**このシナリオを実装するには、次の操作が必要です。**

* [タグとAdobe I/O を使用した AEM と Adobe Target の統合](./implementation.md#integrating-aem-target-options)

***上記の統合を実装したところで、ここからは[シナリオの詳細](./personalization-use-case-2.md)***&#x200B;を見ていきましょう
