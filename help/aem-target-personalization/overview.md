---
title: AEM と Adobe Target の使用の手引き
description: Adobe Experience ManagerとAdobe Targetを使用してパーソナライズされたエクスペリエンスを作成し、配信する方法を示す、エンドツーエンドのチュートリアルです。 このチュートリアルでは、エンドツーエンドのプロセスに関わる様々なユーザーと、それらが相互にどのように共同作業するかについても学びます
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# AEM と Adobe Target の使用の手引き {#getting-started-with-aem-target}

AEMと Target は、両方とも強力なソリューションで、機能が一見重複しているように見えます。 お客様は、パーソナライズされたエクスペリエンスを提供するために、これらの製品をどのように、いつ組み合わせて使用すればよいかを理解するのに苦労する場合があります。 各エンドユーザーに最適化されたエクスペリエンスを提供するには、組織内の異なるチームが緊密に連携し、誰が何をおこなうかを定義する必要があります。

このチュートリアルでは、AEMと Target の 3 つの異なるシナリオについて説明します。これは、組織にとって最適な機能と、異なるチームが共同作業する方法を理解するのに役立ちます。

* シナリオ 1 :AEM Experience Fragments を使用したパーソナライゼーション
* シナリオ 2 :Visual Experience Composer を使用したパーソナライゼーション
* シナリオ 3 :完全な Web ページエクスペリエンスのパーソナライズ

## AEM Experience Fragments を使用したパーソナライゼーション {#personalization-using-aem-experience-fragment}

このシナリオでは、AEMと Target を使用します。 明らかに、両方の製品に独自の強みがあり、サイトのユーザーに対してパーソナライズされたエクスペリエンスを提供するには、 **パーソナライズされたコンテンツ (AEMのコンテンツ )** および **インテリジェント方法 (Target)** 特定のユーザーに基づいてこれらのコンテンツを提供する。

AEMは、パーソナライズされたコンテンツを作成し、すべてのコンテンツとアセットを一元的に統合して、パーソナライゼーション戦略を強化するのに役立ちます。 AEMを使用すると、コードを記述することなく、1 か所でデスクトップ、タブレット、モバイルデバイス用のコンテンツを簡単に作成できます。 デバイスごとにページを作成する必要はありません。AEMは、コンテンツを使用して各エクスペリエンスを自動的に調整します。 また、ボタンを押した状態で、コンテンツをAEMからAdobe Targetにオファーとして書き出すこともできます。

これで、Target のAEMからのオファーの形式でパーソナライズされたコンテンツが作成されました。 Target では、行動、コンテキスト、オフラインの変数を組み込んだルールベースの手法と AI 駆動の機械学習手法を組み合わせ、それに基づいてこれらのオファーを大規模に提供できます。  Target では、A/B テストや多変量分析 (MVT) アクティビティを簡単に設定して実行し、最適なオファー、コンテンツ、エクスペリエンスを見極めることができます。

**エクスペリエンスフラグメント** は、コンテンツ/エクスペリエンスの作成者を、Target を使用してビジネス成果を促進しているパーソナライゼーションの専門家にリンクする大きな一歩を踏み出します。

* AEMコンテンツエディターは、エクスペリエンスフラグメントおよびそのバリエーションとしてパーソナライズされたコンテンツを作成します。
* AEMが Target にエクスペリエンスフラグメントHTMLを書き出しま&#x200B;す
* Target で&#x200B;は、AEM Experience Fragment マークアップをアクティビティのオファーとして使用します
* Target はエクスペリエンスフラグメントHTMLを配信し、AEMは参照画像を提供します

   ![エクスペリエンスフラグメントを使用したパーソナライゼーション図](assets/personalization-use-case-1/use-case-1-diagram.png)

**このシナリオを実装するには、次の操作が必要です。**

* [Launch とAdobe I/Oを使用したAEMとAdobe Targetの統合](./implementation.md#integrating-aem-target-options)
* [AEMとAdobe Target( レガシーCloud Servicesを使用 )](./implementation.md#integrating-aem-target-options)

***上記の統合を実装した後、を参照してください。 [詳細なシナリオ](./personalization-use-case-1.md).***

## Visual Experience Composer を使用したパーソナライゼーション

Adobe Target Visual Experience Composer(VEC) を使用してテストを実行するコードを変更しなくても、マーケターは Web サイト上で迅速に変更を加えることができます。 VEC は WYSIWYG（表示されるもの）ユーザーインターフェイスで、サイトコンテキストのパーソナライズされたエクスペリエンスやオファーを簡単に作成およびテストできます。 Web ページ（またはオファー）またはモバイル Web ページのレイアウトとコンテンツをドラッグ&amp;ドロップ、入れ替えおよび変更することで、Target アクティビティのエクスペリエンスとオファーを作成できます。

VEC はAdobe Targetの主な機能の 1 つです。 VEC を使用すると、マーケティング担当者やデザイナーが視覚的なインターフェイスを使用してコンテンツを作成および変更できます。 コードを直接編集する必要なく、多くのデザインの選択肢を作成できます。 HTMLと JavaScript の編集は、コンポーザーで使用可能な編集オプションを使用しても可能です。

* コンテンツはAEM内に存在し、コンテンツエディターはサイトページを作成および管理します
* Target は、AEMでホストされるサイトページを使用して、テストとパーソナライゼーションを実行します
* Target がパーソナライズされたコンテンツを配信
* Adobe Target VEC を使用して新しいコンテンツが作成されました
* AEMがホストするサイトと非AEMのホストするサイトの両方に適用されます

   ![Visual Experience Composer の図を使用したパーソナライゼーション](assets/personalization-use-case-3/use-case-diagram-3.png)

**このシナリオを実装するには、次の操作が必要です。**

* [Launch とAdobe I/Oを使用したAEMとAdobe Targetの統合](./implementation.md#integrating-aem-target-options)

***上記の統合を実装した後、を参照してください。 [シナリオの詳細を示します。](./personalization-use-case-3.md)***

## 完全な Web ページエクスペリエンスのパーソナライズ

Adobe Experience ManagerとAdobe Targetの統合により、サイトのユーザーにパーソナライズされたエクスペリエンスを提供できます。 また、指定したテスト期間に、コンバージョンを最も向上させるのに最適な Web サイトコンテンツのバージョンをより深く理解するのに役立ちます。 例えば、A/B テストでは、複数のバージョンの Web サイトコンテンツを比較して、特定したコンバージョン、売上高、その他の指標のうち最も高い上昇率を示す指標を確認します。 マーケターは、Adobe Target内でアクティビティを作成して、ユーザーがサイトのコンテンツとどのようにやり取りするか、およびサイト指標に与える影響を把握できます。

* コンテンツはAEM内に存在し、コンテンツエディターはサイトページを作成および管理します
* Target は、AEMでホストされるサイトページを使用して、テストとパーソナライゼーションを実行します
* Target がパーソナライズされたコンテンツを配信
* ここに新しいコンテンツは作成されません
* AEMと非AEMの両方のサイトに適用されます

   ![図](assets/personalization-use-case-2/use-case-2-diagram.png)

**このシナリオを実装するには、次の操作が必要です。**

* [Launch とAdobe I/Oを使用したAEMとAdobe Targetの統合](./implementation.md#integrating-aem-target-options)

***上記の統合を実装した後、を参照してください。 [シナリオの詳細を示します。](./personalization-use-case-2.md)***
