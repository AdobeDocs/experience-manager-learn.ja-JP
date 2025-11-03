---
title: Personalization ユースケースのライブデモ
description: A/B テスト、行動ターゲティング、既知のユーザーパーソナライゼーションの例を使用して、WKND イネーブルメント web サイトでのアクションでのパーソナライゼーションを体験してください。
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Architect, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-03T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: 9e99936fb03e085f6bc276c7d6ef5cc08e34d1e5
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 2%

---

# Personalization ユースケースのライブデモ

[WKND イネーブルメント web サイト &#x200B;](https://wknd.enablementadobe.com/us/en.html){target="wknd"} にアクセスして、A/B テスト、行動ターゲティング、既知のユーザーパーソナライゼーションの実際の例をご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3476461/?learn=on&enablevpops)

このページでは、各パーソナライゼーションシナリオの実践デモについて説明します。 これを使用して、独自のAEM サイトにこれらの機能を構築する前に、何が可能かを調べます。

>[!IMPORTANT]
>
> 複数のブラウザーウィンドウまたは匿名/プライベートブラウジングモードでデモサイトを開いて、パーソナライズされた様々なバリエーションを同時に体験できます。
> プライベートブラウジングモードを使用する場合、Firefox と Safari は ECID cookie をブロックする場合があります。または、新しいパーソナライゼーションシナリオを試す前に、通常のブラウジングモードを使用するか、cookie をクリアする場合があります。

## デモの使用例

[WKND イネーブルメント web サイト &#x200B;](https://wknd.enablementadobe.com/us/en.html){target="wknd"} では、次の 3 種類のパーソナライゼーションを示します。

| Personalizationタイプ | 表示される内容 | タイミング |
|---------------------|-----------------|---------|
| **行動ターゲティング** | コンテンツは、閲覧行動と興味に基づいて適応します。 一般的に _次のページまたは同じページのパーソナライゼーション_ と呼ばれる | リアルタイムおよびバッチ |
| **既知のユーザーのPersonalization** | 複数のシステムをまたいで作成されたデータから構築された完全な顧客プロファイルに基づいて、カスタマイズされたエクスペリエンスを提供します。 一般に _大規模なパーソナライゼーション_ と呼ばれる | リアルタイム |
| **A/B テスト** | 最もパフォーマンスの高いコンテンツを見つけるためにテストした様々なコンテンツバリエーション。 一般に _実験_ と呼ばれる | リアルタイム |

## 行動ターゲティング

コンテンツは、閲覧セッション中の訪問者の行動と興味に基づいて自動的に適応します。 これは、一般に _次のページまたは同じページのパーソナライゼーション_ と呼ばれます。

### ホーム、アドベンチャー、雑誌のページ

これらのエクスペリエンスは、現在のブラウジング動作（リアルタイムパーソナライゼーション）に基づいて直ちに表示されます。 Adobe Experience Platform Edge Networkを使用すると、パーソナライゼーションに関する意思決定をリアルタイムで行えます。

| ページ | 表示される内容 | テスト方法 | エクスペリエンス |
|------|-----------------|-------------|------------|
| [ホーム](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 湖畔で一緒に自転車を運転する家族を特集したパーソナライズされた **家族に優しいアドベンチャーヒーローバナー** 共有された思い出を作り出すガイド付き体験を促進します | [Bali Surf Camp](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"} または [Gastronomic Marais Tour](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"} にアクセスして、ホームページに戻ります | ![&#x200B; ホーム – ファミリー・アドベンチャー・ヒーロー &#x200B;](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [Adventures](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | サイクリングに焦点を当てた **「フリーバイクチューンアップ」プロモーションのヒーロー** 「We&#39;ve Got You Covered」のメッセージ、および WKND のエキスパートパートナーからの無料のバイクメンテナンスオファー | バイク関連のアドベンチャー（例：[Cycling Tuscany](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"}）にアクセスし、Adventures ページに移動します | ![&#x200B; アドベンチャー – 主人公をチューンアップフリーバイク &#x200B;](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [Adventures](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | キャンプをテーマにした **ギアのコレクションのヒーロー** には、「次のアドベンチャーは正しいギアから始まる」というメッセージと共に、重要なキャンプ用品（寝袋、ジャケット、ブーツ）が表示されます | キャンプ関連のアドベンチャー（例：[Yosemite Backpacking](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"}）にアクセスし、Adventures ページに移動します | ![&#x200B; アドベンチャー – キャンプギアコレクションヒーロー &#x200B;](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [&#x200B; マガジン &#x200B;](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | ロールされた WKND 誌と顕著な「SALE **を取り上げた、時間に敏感な** 雑誌販売プロモーション」 イシューとアウトドアコレクションに関するバッジと特別な読者の価格 | 1 つ以上の雑誌記事（[&#x200B; スキーツアー &#x200B;](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"} など）を読み、Magazine ランディングページに移動します | ![Magazine - Sale Hero](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### アドベンチャーおよび雑誌のページ（バッチ）

これらのエクスペリエンスは、過去の行動に基づいており、次回の訪問または当日の後半（バッチパーソナライゼーション）に表示されます。 データは集計され、プロファイル属性に処理されて、Adobe Experience Platform Edge Networkに対してアクティブ化されます。

| ページ | 表示される内容 | テスト方法 | エクスペリエンス |
|------|-----------------|-------------|------------|
| [Adventures](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | **パームツリーの下のカラフルなサーフボード** 「あなたのサーフジャーニーはここから始まります」のメッセージと、あなたの興味に基づいてキュレートされたサーフの宛先コンテンツを備えたサーフテーマのヒーロー | 複数の [surf-related adventures](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"} を訪問し、翌日に Adventures ページに戻ります | ![&#x200B; アドベンチャー – サーフィンヒーロー &#x200B;](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [&#x200B; マガジン &#x200B;](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | 世界の旅行先にクラシックな VW バンを搭載し、サブスクライバー限定の特典を備えた「パーソナライズされたマガジン体験」を強調した **パーソナライズされたマガジン購読オファー** | 3 つ以上の [&#x200B; 雑誌記事 &#x200B;](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} を読み、翌日に Magazine ランディングページに戻ります | ![Magazine - Subscribe Hero](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**詳細情報：** 独自のAEM サイトに行動ターゲティングを実装する準備はできていますか？ [&#x200B; 行動ターゲティングチュートリアル &#x200B;](./use-cases/behavioral-targeting.md) から始めて、完全な設定プロセスを学習します。

## 既知のユーザーのPersonalization

購入履歴やカスタマーライフサイクルステージなど、複数のシステムをまたいだデータから作成された完全な顧客プロファイルに基づいて、エクスペリエンスをパーソナライズできます。 Adobe Experience Platform Edge Networkを使用すると、パーソナライゼーションに関する意思決定をリアルタイムで行えます。

### ホームページヒーロー

WKND ホームページのヒーローバナーは、認証済みのユーザープロファイルに基づいてパーソナライズされます。 これらのデモアカウントを使用してテストし、パーソナライズされたエクスペリエンスを確認します。

| ページ | 表示される内容 | テスト方法 | プロファイルコンテキスト | エクスペリエンス |
|------|-----------------|-------------|-----------------|------------|
| [ホーム](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 今後のスキーアドベンチャーに備えたエキスパートパッキングのヒントを備えた **プレミアムスキー用品** プロモーションを提供するスキーショップのインテリア | `teddy/teddy` または `asmith/asmith` でログインし、ページを更新します | 最近購入したスキーの冒険、したがってスキー用品をアップセル | ![&#x200B; ホーム – スキー用品アップセル &#x200B;](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**詳細情報：** 独自のAEM サイトに既知のユーザーをパーソナライゼーションする準備はできていますか？ 完全な設定プロセスを学ぶには、[&#x200B; 既知のユーザーのPersonalization チュートリアル &#x200B;](./use-cases/known-user-personalization.md) から始めてください。

## A/B テスト（実験）

様々なコンテンツのバリエーションをテストして、ビジネス目標に最適なパフォーマンスを発揮するコンテンツを決定します。 Adobe Targetは、訪問者に対して様々なバリエーションをランダムに提供し、パフォーマンスを向上させます。 これは、一般に _実験_ と呼ばれます。

### ホームページの特集記事

WKND ホームページでは、「西オーストラリアでのキャンプ _の 3 つのバリエーションの特集記事を使用して、アクティブな A/B テスト_ 実行します。 各訪問者には、次のバリエーションのいずれかが表示されます。

| ページ | 表示される内容 | テスト方法 | エクスペリエンス |
|------|-----------------|-------------|------------|
| [ホーム](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | 「Our Featured」セクションでランダムに割り当てられた 3 つの注目の記事バリエーションの 1 つ：**「Off the Grid: Epic Camping Routes Across Western Australia」または** 「Wranging the Wild: Camping Adventures in Western Australia」 **または 3 つ目のバリエーション）** それぞれがユニークな画像とメッセージを備えており、最も共感できるテストを行います | 様々なブラウザーでホームページにアクセスしたり、匿名/プライベートモードを使用したり、cookie をクリアして様々なバリエーションを表示したりします | ![A/B テストのバリエーション &#x200B;](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**詳細情報：** AEM サイトに A/B テストを実装する準備はできていますか？ [&#x200B; 実験（A/B テスト）チュートリアル &#x200B;](./use-cases/experimentation.md) から開始して、完全な設定プロセスを学習します。


## 次の手順

独自のAEM サイトにパーソナライゼーションを実装する準備はできていますか？ [Personalizationの概要から始めて &#x200B;](./overview.md) 設定プロセス全体を確認します。


