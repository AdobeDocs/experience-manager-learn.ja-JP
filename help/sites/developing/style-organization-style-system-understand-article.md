---
title: AEM Sites スタイルシステムのベストプラクティスについて
description: Adobe Experience Manager Sites でスタイルシステムを実装する際のベストプラクティスについて詳しく説明する記事です。
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 328
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1522'
ht-degree: 100%

---

# スタイルシステムのベストプラクティスについて{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>[スタイルシステムでコーディングする方法について](style-system-technical-video-understand.md)の内容を確認して、AEM スタイルシステムで使用される BEM のような規則を理解してください。

AEM スタイルシステムには、2 つの主なフレーバーまたはスタイルが実装されています。

* **レイアウトスタイル**
* **表示スタイル**

**レイアウトスタイル**&#x200B;は、コンポーネントの多くの要素に影響を与えて、コンポーネントの明確に定義され識別可能なレンディション（デザインとレイアウト）を作成し、多くの場合、再利用可能な特定のブランドの概念に合わせます。 例えば、ティーザーコンポーネントは、従来のカードベースのレイアウト、水平方向のプロモーションスタイル、または画像上のテキストをオーバーレイするヒーローレイアウトとして表示できます。

**表示スタイル**&#x200B;は、レイアウトスタイルのマイナーなバリエーションに影響を与えるために使用されますが、レイアウトスタイルの基本的な性質や意図は変更されません。 例えば、ヒーローレイアウトスタイルには、カラースキームをプライマリブランドのカラースキームからセカンダリブランドのカラースキームに変更する表示スタイルが含まれている場合があります。

## スタイル組織のベストプラクティス {#style-organization-best-practices}

AEM の作成者が使用できるスタイル名を定義する場合は、次の方法が最適です。

* 作成者が理解できる語彙を使用してスタイルに名前を付ける
* スタイルオプションの数を最小限に抑える
* ブランド標準で許可されているスタイルオプションと組み合わせのみを公開する
* 効果を持つスタイルの組み合わせのみを公開する
   * 効果のない組み合わせが公開される場合は、少なくとも悪影響がないことを確認する

AEM の作成者が使用できる可能なスタイルの組み合わせの数が増えるにつれて、ブランド標準に対して QA および検証を行う必要がある組み合わせが増えます。また、オプションが多すぎると、目的の効果を生み出すために必要なオプションや組み合わせが不明になる可能性があり、作成者が混乱する場合があります。

### スタイル名と CSS クラス {#style-names-vs-css-classes}

スタイル名、または AEM 作成者に提示されるオプションと実装する CSS クラス名は、AEM で分離されています。

これにより、AEM の作成者は明確で理解できる語彙でスタイルオプションにラベルを付けることができますが、CSS デベロッパーは、将来にわたって有効な、セマンティックな方式で CSS クラスに名前を付けることができます。 次に例を示します。

コンポーネントには、ブランドの&#x200B;**プライマリ**&#x200B;カラーおよび&#x200B;**セカンダリ**&#x200B;カラーで色付けするオプションが必要ですが、AEM の作成者は、カラーをプライマリとセカンダリのデザイン言語ではなく、**グリーン**&#x200B;や&#x200B;**イエロー**&#x200B;として知っています。

AEM スタイルシステムでは、作成者は、**グリーン**&#x200B;および&#x200B;**イエロー**&#x200B;という使いやすいラベルを使用してカラー表示スタイルを公開できる一方、CSS デベロッパーは `.cmp-component--primary-color` および `.cmp-component--secondary-color` のセマンティックなネーミングを使用して、CSS で実際のスタイルの実装を定義できます。

スタイル名&#x200B;**グリーン**&#x200B;は `.cmp-component--primary-color` に、**イエロー**&#x200B;は `.cmp-component--secondary-color` にマッピングされます。

今後、会社のブランド カラーが変更された場合、変更する必要があるのは、`.cmp-component--primary-color` と `.cmp-component--secondary-color` の単一の実装と、スタイル名だけです。

## ティーザーコンポーネントを使用したユースケース {#the-teaser-component-as-an-example-use-case}

次に、ティーザーコンポーネントのスタイルをレイアウトと表示のスタイルが異なる複数のスタイルに設定するユースケースを示します。

これは、スタイル名（作成者に公開）の仕組みと、バッキング CSS クラスが編成される方法を探索します。

### ティーザーコンポーネントのスタイルの設定 {#component-styles-configuration}

次の画像は、ユースケースで説明したバリエーションのティーザーコンポーネントの[!UICONTROL スタイル]設定を示しています。

[!UICONTROL スタイルグループ]の名前、レイアウト、表示は、この記事でスタイルの種類を概念的に分類するために使用される表示スタイルおよびレイアウトスタイルの一般的な概念と偶然一致します。

[!UICONTROL スタイルグループ]の名前と[!UICONTROL スタイルグループ]数は、コンポーネントのユースケースとプロジェクト固有のコンポーネントのスタイリング規則に合わせて調整する必要があります。

例えば、**ディスプレイ**&#x200B;スタイルグループ名は、**カラー**&#x200B;という名前にすることもできます。

![ディスプレイスタイルグループ](assets/style-config.png)

### スタイル選択メニュー {#style-selection-menu}

下の画像は、[!UICONTROL スタイル]メニューの作成者が操作して、コンポーネントに適切なスタイルを選択する様子を示しています。[!UICONTROL スタイルグループ]名とスタイル名はすべて作成者に公開されることに注意してください。

![スタイルドロップダウンメニュー](assets/style-menu.png)

### デフォルトスタイル {#default-style}

デフォルトスタイルは、多くの場合、コンポーネントで最も一般的に使用されるスタイルで、ティーザーがページに追加したときの、スタイルが設定されていないデフォルトの表示です。

デフォルトスタイルの一般性に応じて、CSS は `.cmp-teaser`（修飾子なし）または `.cmp-teaser--default` に直接適用できます。

デフォルトのスタイルルールがすべてのバリエーションに適用される頻度が多い場合は、`.cmp-teaser` をデフォルトスタイルの CSS クラスとして使用することをお勧めします。これは、BEM のような規則に従っていると仮定すると、すべてのバリエーションがそれらを暗黙的に継承する必要があるためです。そうでない場合は、`.cmp-teaser--default` などのデフォルト修飾子を介して適用して、これを[コンポーネントのスタイル設定のデフォルト CSS クラス](#component-styles-configuration)フィールドに追加する必要があります。そうしないと、これらのスタイルルールが各バリエーションでオーバーライドされます。

「名前付き」スタイルをデフォルトスタイルとして割り当てることも可能です。その例は、以下で定義されているヒーロースタイル `(.cmp-teaser--hero)` です。ただし、`.cmp-teaser` または `.cmp-teaser--default` CSS クラスの実装に対してデフォルトスタイルを実装する方が明確です。

>[!NOTE]
>
>デフォルトのレイアウトスタイルには表示スタイル名はありませんが、作成者は AEM スタイルシステムの選択ツールで「表示」オプションを選択できます。
>
>これは、ベストプラクティスに違反しています。
>
>**効果のある、スタイルの組み合わせのみを表示する**
>
>作成者が&#x200B;**グリーン**&#x200B;の表示スタイルを選択しても、何も起こりません。
>
>他のすべてのレイアウトスタイルはブランドカラーを使用して色を付ける必要があるので、このユースケースでは、この違反を認めます。
>
>以下の&#x200B;**プロモ（右揃え）**&#x200B;の節では、スタイルの不要な組み合わせを防ぐ方法について説明します。

![デフォルトスタイル](assets/default.png)

* **レイアウトスタイル**
   * デフォルト
* **表示スタイル**
   * なし
* **有効な CSS クラス**：`.cmp-teaser--promo` または `.cmp-teaser--default`

### プロモスタイル {#promo-style}

**プロモレイアウトスタイル**&#x200B;は、価値の高いコンテンツをサイトで宣伝するために使用され、web ページ上の帯状の領域を占めるために水平にレイアウトされます。また、ブランドカラーでスタイル設定できる必要があります。デフォルトのプロモレイアウトスタイルでは黒いテキストを使用します。

これを達成するために、**プロモ**&#x200B;の&#x200B;**レイアウトスタイル**、および&#x200B;**グリーン**&#x200B;と&#x200B;**イエロー**&#x200B;の&#x200B;**表示スタイル**&#x200B;を、AEM スタイルシステムでティーザーコンポーネントに設定します。

#### プロモのデフォルト

![プロモのデフォルト](assets/promo-default.png)

* **レイアウトスタイル**
   * スタイル名：**プロモ**
   * CSS クラス：`cmp-teaser--promo`
* **表示スタイル**
   * なし
* **有効な CSS クラス**：`.cmp-teaser--promo`

#### プロモプライマリ

![プロモプライマリ](assets/promo-primary.png)

* **レイアウトスタイル**
   * スタイル名：**プロモ**
   * CSS クラス：`cmp-teaser--promo`
* **表示スタイル**
   * スタイル名：**グリーン**
   * CSS クラス：`cmp-teaser--primary-color`
* **有効な CSS クラス**：`cmp-teaser--promo.cmp-teaser--primary-color`

#### プロモセカンダリ

![プロモセカンダリ](assets/promo-secondary.png)

* **レイアウトスタイル**
   * スタイル名：**プロモ**
   * CSS クラス：`cmp-teaser--promo`
* **表示スタイル**
   * スタイル名：**イエロー**
   * CSS クラス：`cmp-teaser--secondary-color`
* **有効な CSS クラス**：`cmp-teaser--promo.cmp-teaser--secondary-color`

### プロモ右揃えスタイル {#promo-r-align}

**プロモ右揃え**&#x200B;レイアウトスタイルはプロモーションスタイルのバリエーションで、画像とテキストの場所（画像は右、テキストは左）を反転させるものです。

右揃えは、根本的には表示スタイルであり、プロモーションレイアウトスタイルと組み合わせて選択された表示スタイルとして AEM スタイルシステムに入力できます。これは、次のベストプラクティスに違反します。

**効果のある、スタイルの組み合わせのみを表示する**

..これは既に[デフォルトスタイル](#default-style)で違反しています。

右揃えはプロモレイアウトスタイルにのみ影響し、他の 2 つのレイアウトスタイル（デフォルトおよびヒーロー）には影響しないので、プロモレイアウトスタイルのコンテンツを右揃えにする CSS クラスを含んだ新しいレイアウトスタイル「プロモ（右揃え）」を次のように作成できます`cmp -teaser--alternate`。

このように複数のスタイルを 1 つのスタイルエントリに組み合わせると、使用可能なスタイルとスタイルの並べ替えの数を減らすこともでき、数を最小限に抑えるうえで最適です。

CSS クラスの名前 `cmp-teaser--alternate` は、「右揃え」という作成者にわかりやすい命名法と一致しなくてもかまいません。

#### プロモ右揃えのデフォルト

![プロモ右揃え](assets/promo-alternate-default.png)

* **レイアウトスタイル**
   * スタイル名： **プロモ（右揃え）**
   * CSS クラス：`cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * なし
* **有効な CSS クラス**：`.cmp-teaser--promo.cmp-teaser--alternate`

#### プロモ右揃えプライマリ

![プロモ右揃えプライマリ](assets/promo-alternate-primary.png)

* **レイアウトスタイル**
   * スタイル名： **プロモ（右揃え）**
   * CSS クラス：`cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * スタイル名：**グリーン**
   * CSS クラス：`cmp-teaser--primary-color`
* **有効な CSS クラス**：`.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### プロモ右揃えのセカンダリ

![プロモ右揃えセカンダリ](assets/promo-alternate-secondary.png)

* **レイアウトスタイル**
   * スタイル名： **プロモ（右揃え）**
   * CSS クラス：`cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * スタイル名：**イエロー**
   * CSS クラス：`cmp-teaser--secondary-color`
* **有効な CSS クラス**：`.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### ヒーロースタイル {#hero-style}

ヒーローレイアウトスタイルでは、コンポーネントの画像を背景として、タイトルとリンクがオーバーレイ表示されます。 ヒーローレイアウトスタイルは、プロモレイアウトスタイルと同様に、ブランドカラーを使用してカラーリングできる必要があります。

ヒーローレイアウトスタイルにブランドカラーを適用するには、プロモレイアウトスタイルと同じ表示スタイルを利用できます。

コンポーネントごとに、スタイル名が単一の CSS クラスのセットにマッピングされます。つまり、プロモレイアウトスタイルの背景に色を付ける CSS クラス名は、レイアウトスタイルのテキストとリンクに色を付ける必要があります。

これは CSS ルールのスコープを設定することで簡単に実現できますが、CSS デベロッパーはこれらの順列が AEM でどのように制定されるかを理解する必要があります。

**プロモ**&#x200B;レイアウトスタイルの背景を原色（グリーン）で色付けするための CSS：

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

**ヒーロー**&#x200B;レイアウトスタイルのテキストを原色 （グリーン）で色付けするための CSS：

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### ヒーローのデフォルト

![ヒーロースタイル](assets/hero.png)

* **レイアウトスタイル**
   * スタイル名：**ヒーロー**
   * CSS クラス：`cmp-teaser--hero`
* **表示スタイル**
   * なし
* **有効な CSS クラス**：`.cmp-teaser--hero`

#### ヒーロープライマリ

![ヒーロープライマリ](assets/hero-primary.png)

* **レイアウトスタイル**
   * スタイル名：**プロモ**
   * CSS クラス：`cmp-teaser--hero`
* **表示スタイル**
   * スタイル名：**グリーン**
   * CSS クラス：`cmp-teaser--primary-color`
* **有効な CSS クラス**：`cmp-teaser--hero.cmp-teaser--primary-color`

#### ヒーローセカンダリ

![ヒーローセカンダリ](assets/hero-secondary.png)

* **レイアウトスタイル**
   * スタイル名：**プロモ**
   * CSS クラス：`cmp-teaser--hero`
* **表示スタイル**
   * スタイル名：**イエロー**
   * CSS クラス：`cmp-teaser--secondary-color`
* **有効な CSS クラス**：`cmp-teaser--hero.cmp-teaser--secondary-color`

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM クライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（ブロック要素修飾子）ドキュメント web サイト](https://getbem.com/)
* [LESS ドキュメント web サイト](https://lesscss.org/)
* [jQuery web サイト](https://jquery.com/)
