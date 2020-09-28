---
title: スタイルシステムのベストプラクティスとAEM Sitesについて
description: Adobe Experience Manager Sitesでのスタイル制度の導入に際し、ベストプラクティスを説明した詳細な記事。
feature: style-system
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1538'
ht-degree: 3%

---


# スタイルシステムのベストプラクティスについて{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>AEM Style Systemで使用されるBEMに類似した規則を確認するには、 [「Understanding how to code for the Style System](style-system-technical-video-understand.md)」の内容を確認してください。

AEM Style Systemには、2つの主な種類またはスタイルが実装されています。

* **レイアウトのスタイル**
* **表示スタイル**

**レイアウトスタイル** は、コンポーネントの適切な定義と識別可能なレンディション（デザインとレイアウト）を作成するためのコンポーネントの多くの要素に影響し、多くの場合、再利用可能な特定のブランドの概念に合わせて調整されます。 例えば、Teaserコンポーネントは、従来のカードベースのレイアウト、横置きのプロモーション、または画像上にテキストをオーバーレイするヒーローレイアウトとして表示できます。

**表示スタイル** は、レイアウトスタイルの小さなバリエーションに影響を与えるのに使用されますが、レイアウトスタイルの基本的な特性や意図は変わりません。 例えば、ヒーローレイアウトスタイルに表示スタイルがあり、その表示スタイルによってプライマリブランドのカラースキームからセカンダリブランドのカラースキームにカラースキームが変更される場合があります。

## スタイル組織のベストプラクティス {#style-organization-best-practices}

AEM作成者が使用できるスタイル名を定義する場合は、次の操作を行うことをお勧めします。

* 作成者が理解したボキャブラリを使用してスタイルに名前を付ける
* スタイルオプションの数を最小限に抑える
* ブランド標準で許可されているスタイルオプションと組み合わせのみを公開します。
* 効果を持つスタイルの組み合わせのみを公開する
   * 無効な組み合わせが暴露された場合は、少なくとも効果が低下しないようにする

AEMの作成者が使用できるスタイルの組み合わせの数が増えるにつれて、ブランド標準に対してQAを行い、検証する必要のある順序が増えます。 また、オプションが多すぎると、意図した効果を生み出すために必要なオプションや組み合わせが不明確になる可能性があるので、作成者を混乱させる可能性もあります。

### スタイル名とCSSクラス {#style-names-vs-css-classes}

スタイル名、またはAEM作成者に提示されるオプション、および実装するCSSクラス名はAEMで切り離されます。

これにより、AEMの作成者が明確で理解できるボキャブラリ内にスタイルオプションのラベルを付けることができますが、CSS開発者は、CSSクラスに将来的な配達確認、意味的な方法で名前を付けることができます。 次に例を示します。

コンポーネントは、ブランドの **主色と** 二次色で色付けするオプションが必要ですが **、AEMの作成者は、ブランドの主色** と二次色で色付けするオプションを **緑色、** 黄色はデザイン言語の主色と二次色ではな ****&#x200B;く黄色に分かっています。

AEM Style Systemは、作成者にわかりやすいラベル **Green** と **Yellowを使用してこれらの色付き表示スタイルを公開できます。一方、CSS開発者は、セマンティックネーミングを使用し、実際のスタイル実装をCSS**`.cmp-component--primary-color``.cmp-component--secondary-color` で定義できます。

「 **Green** 」のスタイル名はにマップされ `.cmp-component--primary-color`、「 **Yellow** 」はにマップされ `.cmp-component--secondary-color`ます。

会社のブランド色が将来変更される場合、変更が必要になるのは、との単一実装と、スタイル名 `.cmp-component--primary-color` の `.cmp-component--secondary-color`みです。

## 使用例としてのTeaserコンポーネント {#the-teaser-component-as-an-example-use-case}

Teaserコンポーネントのスタイル設定でレイアウトと表示のスタイルが異なる使用例を次に示します。

これは、スタイル名（作成者に公開）の仕組みと、背景となるCSSクラスの編成方法について説明します。

### Teaserコンポーネントスタイルの設定 {#component-styles-configuration}

次の図は、使用例で説明されているバリエーションのTeaserコンポーネントの [!UICONTROL スタイル] 設定を示しています。

[ [!UICONTROL スタイルグループ名] ]、[レイアウト]、[表示]は、この記事で概念的にスタイルの種類を分類するために使用される[表示スタイル]および[レイアウトスタイル]の一般的な概念と一致します。

「 [!UICONTROL スタイルグループ] 」の名前と「  スタイルグループの数」は、コンポーネントの使用例とプロジェクト固有のコンポーネントスタイル規則に合わせて調整する必要があります。

例えば、 **表示** スタイルグループ名が「 **Colors」という名前になっているとします**。

![スタイルグループを表示](assets/style-config.png)

### スタイル選択メニュー {#style-selection-menu}

以下の画像には、 [!UICONTROL スタイル] ・メニューの作成者が、コンポーネントに適したスタイルを選択する際に操作を行います。 スタイルグ  リップ名とスタイル名は、すべて作成者に公開されます。

![スタイルドロップダウンメニュー](assets/style-menu.png)

### Default style {#default-style}

デフォルトのスタイルは多くの場合、コンポーネントで最も一般的に使用されるスタイルで、ページに追加する際にスタイルが設定されていないデフォルトの表示です。

初期設定のスタイルの共通性に応じて、CSSは、(修飾子なし `.cmp-teaser` )またはの上に直接適用され `.cmp-teaser--default`ます。

デフォルトのスタイル規則がすべてのバリエーションに適用されるわけではない場合は、BEMのような規則に従うと仮定して、すべてのバリエーションが暗黙的に継承する必要があるので、デフォルトのスタイルのCSSクラスとして使用することをお勧めします。 `.cmp-teaser` そうでない場合は、初期設定の修飾子(例： `.cmp-teaser--default`コンポーネントのスタイル設定の「Default CSS Classes [](#component-styles-configuration) （デフォルトCSSクラス）」フィールドに追加する必要がある)を使用して適用する必要があります。そうでない場合は、これらのスタイルルールを各バリエーションで上書きする必要があります。

「名前付き」スタイルをデフォルトのスタイルとして割り当てることもできます。例えば、以下に定義するヒーロースタイルを使用できますが、 `(.cmp-teaser--hero)` または `.cmp-teaser``.cmp-teaser--default` CSSクラスの実装に対してデフォルトのスタイルを実装する方がより明確です。

>[!NOTE]
>
>デフォルトのレイアウトスタイルには表示スタイル名がありませんが、作成者はAEMスタイルシステム選択ツールで「表示」オプションを選択できます。
>
>これは、ベストプラクティスに違反しています。
>
>**効果を持つスタイルの組み合わせのみを公開する**
>
>作成者が「 **緑」の表示スタイルを選択した場合** 、何も起こりません。
>
>この使用例では、他のすべてのレイアウトスタイルはブランドの色を使用して色付きを付ける必要があるので、この違反を認めます。
>
>以下の「 **プロモーション（右揃え）** 」セクションでは、不要なスタイルの組み合わせを回避する方法を説明します。

![デフォルトスタイル](assets/default.png)

* **レイアウトのスタイル**
   * デフォルト
* **表示スタイル**
   * なし
* **有効なCSSクラス**: `.cmp-teaser--promo` または `.cmp-teaser--default`

### プロモーションのスタイル {#promo-style}

プロ **モーションレイアウトスタイルは** 、サイト上の価値の高いコンテンツを宣伝するために使用され、Webページ上の広範囲を占めるために水平方向にレイアウトされます。また、ブランドの色別にスタイル可能で、デフォルトのプロモーションレイアウトスタイルは黒のテキストです。

これを達成するために、Teaserコンポーネント用の **AEM Styleには、Promoの** レイアウトスタイル **、** Greenの表示スタイル、Yellowの **表示スタイルを********** 設定する。

#### Promoのデフォルト

![プロモーションのデフォルト](assets/promo-default.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモ**
   * CSS クラス: `cmp-teaser--promo`
* **表示スタイル**
   * なし
* **有効なCSSクラス**: `.cmp-teaser--promo`

#### プロモーションプライマリ

![プロモーション一次](assets/promo-primary.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモ**
   * CSS クラス: `cmp-teaser--promo`
* **表示スタイル**
   * スタイル名： **緑**
   * CSS クラス: `cmp-teaser--primary-color`
* **有効なCSSクラス**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### プロモセカンダリ

![プロモセカンダリ](assets/promo-secondary.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモ**
   * CSS クラス: `cmp-teaser--promo`
* **表示スタイル**
   * スタイル名： **黄色**
   * CSS クラス: `cmp-teaser--secondary-color`
* **有効なCSSクラス**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### プロモーションの右揃えのスタイル {#promo-r-align}

Promo Right-aligned **** layout styleは、プロモーションスタイルの一種です。このスタイルは、画像とテキスト（画像、右、テキスト、左）の位置を反転します。

右側の線形は、その中核となる表示スタイルです。この場合、AEMスタイルシステムに、Promoレイアウトスタイルと組み合わせて選択した表示スタイルとして入力できます。 これは、次のベストプラクティスに違反します。

**効果を持つスタイルの組み合わせのみを公開する**

..が既に [デフォルトのスタイルで違反している](#default-style)。

右揃えはプロモーションレイアウトスタイルにのみ影響し、他の2つのレイアウトスタイルには影響しないので、次のように指定します。defaultとheroを指定した場合、Promoレイアウトスタイルのコンテンツを右揃えにするCSSクラスを含む、新しいレイアウトスタイルのプロモーション（右揃え）を作成できます。 `cmp -teaser--alternate`.

複数のスタイルを1つのスタイルエントリに組み合わせることで、使用可能なスタイルの数やスタイルの順序を減らすこともできます。これは最小限に抑えるのが最適です。

CSSクラスの名前は、作成者にわかりやすい「右揃え」の名前 `cmp-teaser--alternate`と一致している必要はありません。

#### プロモーションの右揃えのデフォルト

![プロモーションを右揃え](assets/promo-alternate-default.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモーション（右揃え）**
   * CSS クラス: `cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * なし
* **有効なCSSクラス**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### プロモーションの右揃えのプライマリ

![プロモーションの右揃えのプライマリ](assets/promo-alternate-primary.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモーション（右揃え）**
   * CSS クラス: `cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * スタイル名： **緑**
   * CSS クラス: `cmp-teaser--primary-color`
* **有効なCSSクラス**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### プロモーションのセカンダリ右揃え

![プロモーションのセカンダリ右揃え](assets/promo-alternate-secondary.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモーション（右揃え）**
   * CSS クラス: `cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * スタイル名： **黄色**
   * CSS クラス: `cmp-teaser--secondary-color`
* **有効なCSSクラス**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### ヒーロースタイル {#hero-style}

Heroレイアウトスタイルでは、コンポーネントの画像が背景として表示され、タイトルとリンクがオーバーレイされます。 ヒーローレイアウトスタイルは、Promoレイアウトスタイルと同様、ブランドの色で色付きにする必要があります。

ヒーローレイアウトスタイルにブランドの色を付けるには、プロモーションレイアウトスタイルと同じ表示スタイルを使用できます。

コンポーネントごとに、スタイル名はCSSクラスの1つのセットにマップされます。つまり、Promoレイアウトスタイルの背景色を付けるCSSクラス名は、Heroレイアウトスタイルのテキストとリンクに色を付ける必要があります。

これは、CSSルールをスコープすることで簡単に実現できますが、AEMでこれらの順位を適用する方法をCSS開発者が理解する必要はありません。

Promoteレイアウトスタイルの背景にプライマリ（緑）カラーでカラー **を付けるCSS** :

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

Hero **レイアウトスタイルのテキストにプライマリ（緑）カラーを適用するCSS** :

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### ヒーローのデフォルト

![ヒーロースタイル](assets/hero.png)

* **レイアウトのスタイル**
   * スタイル名： **ヒーロー**
   * CSS クラス: `cmp-teaser--hero`
* **表示スタイル**
   * なし
* **有効なCSSクラス**: `.cmp-teaser--hero`

#### ヒーロープライマリ

![ヒーロープライマリ](assets/hero-primary.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモ**
   * CSS クラス: `cmp-teaser--hero`
* **表示スタイル**
   * スタイル名： **緑**
   * CSS クラス: `cmp-teaser--primary-color`
* **有効なCSSクラス**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### ヒーローセカンダリ

![ヒーローセカンダリ](assets/hero-secondary.png)

* **レイアウトのスタイル**
   * スタイル名： **プロモ**
   * CSS クラス: `cmp-teaser--hero`
* **表示スタイル**
   * スタイル名： **黄色**
   * CSS クラス: `cmp-teaser--secondary-color`
* **有効なCSSクラス**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEMクライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM （ブロック・エレメント・モディファイア）ドキュメントのWebサイト](https://getbem.com/)
* [LESSドキュメントWebサイト](https://lesscss.org/)
* [jQuery Webサイト](https://jquery.com/)
