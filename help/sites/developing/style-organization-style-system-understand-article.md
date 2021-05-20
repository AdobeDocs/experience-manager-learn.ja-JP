---
title: スタイルシステムのベストプラクティス(AEM Sites)について
description: Adobe Experience Manager Sitesでスタイルシステムを実装する際のベストプラクティスを説明する詳細な記事です。
feature: スタイルシステム
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: 開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1541'
ht-degree: 4%

---


# スタイルシステムのベストプラクティスについて{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>AEMスタイルシステムで使用されるBEMに似た表記規則について理解するには、[スタイルシステムのコード方法について](style-system-technical-video-understand.md)の内容を確認してください。

AEMスタイルシステムには、2つの主なフレーバーまたはスタイルが実装されています。

* **レイアウトスタイル**
* **スタイルの表示**

**レイアウ** トスタイルは、コンポーネントの多くの要素に影響を与え、コンポーネントの適切に定義され、識別可能なレンディション（デザインとレイアウト）を作成し、多くの場合、再利用可能な特定のBrandの概念に合わせます。例えば、ティーザーコンポーネントは、従来のカードベースのレイアウト、水平方向のプロモーションスタイル、または画像上のテキストをオーバーレイするヒーローレイアウトとして表示できます。

**表示ス** タイルは、レイアウトスタイルのマイナーなバリエーションに影響を与えるために使用されますが、レイアウトスタイルの基本的な性質や意図は変更されません。例えば、ヒーローレイアウトスタイルには、カラースキームをプライマリブランドカラースキームからセカンダリブランドカラースキームに変更する表示スタイルが含まれている場合があります。

## スタイル組織のベストプラクティス{#style-organization-best-practices}

AEMの作成者が使用できるスタイル名を定義する場合は、次の操作をおこなうことをお勧めします。

* 作成者が理解した語彙を使用してスタイルに名前を付ける
* スタイルオプションの数を最小限に抑える
* ブランド標準で許可されているスタイルオプションと組み合わせのみを公開します。
* 効果を持つスタイルの組み合わせのみを表示します。
   * 無効な組み合わせが露出した場合は、少なくとも効果が低い組み合わせにならないようにします

AEM作成者が使用できるスタイルの組み合わせの数が増えるにつれ、QAを実施し、ブランド標準に照らして検証する必要がある順列が増えます。 また、オプションが多すぎると、作成者を混乱させる可能性があります。これは、目的の効果を生み出すためにどのオプションまたは組み合わせが必要かが不明になる可能性があるからです。

### スタイル名とCSSクラス{#style-names-vs-css-classes}

スタイル名、またはAEM作成者に提示されるオプション、および実装するCSSクラス名はAEMで切り離されています。

これにより、スタイルオプションにAEM作成者が明確で理解できる語彙でラベルを付けることができますが、CSS開発者は、将来にわたって適切で意味的な方法でCSSクラスに名前を付けることができます。 以下に例を示します。

コンポーネントには、ブランドの&#x200B;**プライマリ**&#x200B;と&#x200B;**セカンダリ**&#x200B;のカラーを設定するオプションが必要ですが、AEM作成者は、プライマリとセカンダリのデザイン言語ではなく、 **緑**&#x200B;と&#x200B;**黄色**&#x200B;として色を認識します。

AEMスタイルシステムは、作成者にわかりやすいラベル&#x200B;**Green**&#x200B;と&#x200B;**Yellow**&#x200B;を使用して、これらの色付き表示スタイルを公開できます。一方、CSS開発者は、`.cmp-component--primary-color`と`.cmp-component--secondary-color`の意味的な命名を使用して、実際のスタイル実装をCSSで定義できます。

**Green**&#x200B;のスタイル名は`.cmp-component--primary-color`に、**Yellow**&#x200B;は`.cmp-component--secondary-color`にマッピングされます。

会社のブランドカラーが将来変化する場合、変更が必要なのは`.cmp-component--primary-color`と`.cmp-component--secondary-color`の単一の実装とスタイル名だけです。

## ティーザーコンポーネントの使用例{#the-teaser-component-as-an-example-use-case}

以下は、複数の異なるレイアウトスタイルと表示スタイルを持つティーザーコンポーネントのスタイル設定の使用例です。

これにより、スタイル名（作成者に公開）の仕組みと、バッキングCSSクラスの編成方法が調べられます。

### ティーザーコンポーネントのスタイル設定{#component-styles-configuration}

次の画像は、使用例で説明したバリエーションに対応するティーザーコンポーネントの[!UICONTROL スタイル]設定を示しています。

[!UICONTROL スタイルグループ]の名前、レイアウト、表示は、この記事で概念的にスタイルの種類を分類する際に使用する表示スタイルとレイアウトスタイルの一般的な概念と一致します。

[!UICONTROL スタイルグループ]の名前と[!UICONTROL スタイルグループ]の数は、コンポーネントの使用例やプロジェクト固有のコンポーネントスタイル規則に合わせて調整する必要があります。

例えば、**表示**&#x200B;スタイルグループ名は&#x200B;**色**&#x200B;という名前にすることができます。

![スタイルグループを表示](assets/style-config.png)

### スタイル選択メニュー{#style-selection-menu}

次の画像は、[!UICONTROL スタイル]メニューの作成者が、コンポーネントに適したスタイルを選択する際に操作を行う画面です。 [!UICONTROL スタイルグリップ]の名前とスタイル名は、すべて作成者に公開されていることに注意してください。

![スタイルドロップダウンメニュー](assets/style-menu.png)

### デフォルトのスタイル{#default-style}

デフォルトのスタイルは、多くの場合、コンポーネントで最もよく使用されるスタイルで、ページに追加されたティーザーのデフォルトのスタイル設定されていないビューです。

デフォルトスタイルの一般性に応じて、CSSは`.cmp-teaser`（修飾子なし）または`.cmp-teaser--default`に直接適用されます。

デフォルトのスタイルルールがすべてのバリエーションに適用される頻度が高い場合は、`.cmp-teaser`をデフォルトのスタイルのCSSクラスとして使用することをお勧めします。BEMに似た規則に従っていると仮定して、すべてのバリエーションが暗黙的に継承する必要があるからです。 そうでない場合は、`.cmp-teaser--default`などのデフォルトの修飾子を使用して適用する必要があります。この修飾子は、 [コンポーネントのスタイル設定の「デフォルトのCSSクラス](#component-styles-configuration) 」フィールドに追加する必要があります。

「名前」のスタイルをデフォルトのスタイルとして割り当てることもできます（例：以下で定義するヒーロースタイル`(.cmp-teaser--hero)`など）。ただし、CSSクラスの`.cmp-teaser`または`.cmp-teaser--default`実装に対してデフォルトのスタイルを実装する方が明確です。

>[!NOTE]
>
>デフォルトのレイアウトスタイルには表示スタイル名はありませんが、作成者はAEMスタイルシステムの選択ツールで「表示」オプションを選択できます。
>
>これは、ベストプラクティスに違反しています。
>
>**効果を持つスタイルの組み合わせのみを表示します。**
>
>作成者が&#x200B;**緑**&#x200B;の表示スタイルを選択した場合、何も起こりません。
>
>この使用例では、他のすべてのレイアウトスタイルはブランドの色を使用して色を付ける必要があるので、この違反を認めます。
>
>以下の&#x200B;**プロモーション（右揃え）**&#x200B;の節では、不要なスタイルの組み合わせを防ぐ方法を説明します。

![デフォルトスタイル](assets/default.png)

* **レイアウトスタイル**
   * デフォルト
* **表示スタイル**
   * なし
* **有効なCSSクラス**: `.cmp-teaser--promo` または  `.cmp-teaser--default`

### プロモーションスタイル{#promo-style}

**プロモレイアウトスタイル**&#x200B;は、サイト上の価値の高いコンテンツを宣伝するために使用され、Webページ上の領域を占めるために水平方向にレイアウトされ、ブランドの色でスタイル設定する必要があります。

これを実現するため、ティーザーコンポーネントのAEMスタイルシステムで、**Promo**&#x200B;の&#x200B;**レイアウトスタイル**&#x200B;と、**Green**&#x200B;と&#x200B;**Yellow**&#x200B;の&#x200B;**表示スタイル**&#x200B;を設定します。

#### プロモーションのデフォルト

![プロモーションのデフォルト](assets/promo-default.png)

* **レイアウトスタイル**
   * スタイル名：**Promo**
   * CSS クラス: `cmp-teaser--promo`
* **表示スタイル**
   * なし
* **有効なCSSクラス**:  `.cmp-teaser--promo`

#### プロモプライマリ

![プロモーションプライマリ](assets/promo-primary.png)

* **レイアウトスタイル**
   * スタイル名：**Promo**
   * CSS クラス: `cmp-teaser--promo`
* **表示スタイル**
   * スタイル名：**緑**
   * CSS クラス: `cmp-teaser--primary-color`
* **有効なCSSクラス**:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### プロモセカンダリ

![プロモセカンダリ](assets/promo-secondary.png)

* **レイアウトスタイル**
   * スタイル名：**Promo**
   * CSS クラス: `cmp-teaser--promo`
* **表示スタイル**
   * スタイル名：**黄色**
   * CSS クラス: `cmp-teaser--secondary-color`
* **有効なCSSクラス**:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### プロモーションの右揃えのスタイル{#promo-r-align}

**プロモーションの右揃え**&#x200B;レイアウトスタイルは、プロモーションスタイルのバリエーションで、スタイルは画像とテキスト（画像は右、テキストは左）の位置を反転します。

右側の線形は、コアとなる表示スタイルで、プロモーションレイアウトスタイルと組み合わせて選択された表示スタイルとしてAEMスタイルシステムに入力できます。 これは、次のベストプラクティスに違反します。

**効果を持つスタイルの組み合わせのみを表示します。**

..既に[デフォルトのスタイル](#default-style)で違反しています。

右揃えはプロモーションレイアウトスタイルにのみ影響し、他の2つのレイアウトスタイルには影響しないので、デフォルトとヒーローの場合、プロモーションレイアウトスタイルのコンテンツを右揃えにするCSSクラスを含む、新しいレイアウトスタイルのプロモ（右揃え）を作成できます。`cmp -teaser--alternate`.

複数のスタイルを1つのスタイルエントリに組み合わせることで、使用可能なスタイルとスタイルの順列の数を減らすこともできます。これは最小限に抑えるのが最善です。

CSSクラスの名前`cmp-teaser--alternate`は、作成者にわかりやすい「右揃え」の命名規則と一致する必要はありません。

#### プロモーションの右揃えのデフォルト

![右揃え](assets/promo-alternate-default.png)

* **レイアウトスタイル**
   * スタイル名：**プロモーション（右揃え）**
   * CSS クラス: `cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * なし
* **有効なCSSクラス**:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### プロモーションの右揃えプライマリ

![プロモーションの右揃えプライマリ](assets/promo-alternate-primary.png)

* **レイアウトスタイル**
   * スタイル名：**プロモーション（右揃え）**
   * CSS クラス: `cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * スタイル名：**緑**
   * CSS クラス: `cmp-teaser--primary-color`
* **有効なCSSクラス**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### プロモーションの右揃えのセカンダリ

![プロモーションの右揃えのセカンダリ](assets/promo-alternate-secondary.png)

* **レイアウトスタイル**
   * スタイル名：**プロモーション（右揃え）**
   * CSS クラス: `cmp-teaser--promo cmp-teaser--alternate`
* **表示スタイル**
   * スタイル名：**黄色**
   * CSS クラス: `cmp-teaser--secondary-color`
* **有効なCSSクラス**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### ヒーロースタイル{#hero-style}

ヒーローレイアウトスタイルでは、コンポーネントの画像を背景として表示し、タイトルとリンクがオーバーレイ表示されます。 ヒーローレイアウトスタイル（プロモーションレイアウトスタイルと同様）は、ブランドカラーで色付けする必要があります。

ヒーローレイアウトスタイルにブランドカラーを適用するには、プロモーションレイアウトスタイルと同じ表示スタイルを利用できます。

コンポーネントごとに、スタイル名は単一のCSSクラスのセットにマッピングされます。つまり、プロモーションレイアウトスタイルの背景に色を付けるCSSクラス名は、ヒーローレイアウトスタイルのテキストとリンクに色を付ける必要があります。

これは、CSSルールをスコープすることで実現できますが、AEM上でこれらの順列がどのように適用されるかを理解するために、CSS開発者が必要です。

**昇格**&#x200B;レイアウトスタイルの背景に、プライマリ（緑）カラーを使用してカラーを適用するCSS:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

**Hero**&#x200B;レイアウトスタイルのテキストにプライマリ（緑）カラーを適用するCSS:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero Default

![ヒーロースタイル](assets/hero.png)

* **レイアウトスタイル**
   * スタイル名：**Hero**
   * CSS クラス: `cmp-teaser--hero`
* **表示スタイル**
   * なし
* **有効なCSSクラス**:  `.cmp-teaser--hero`

#### ヒーロープライマリ

![ヒーロープライマリ](assets/hero-primary.png)

* **レイアウトスタイル**
   * スタイル名：**Promo**
   * CSS クラス: `cmp-teaser--hero`
* **表示スタイル**
   * スタイル名：**緑**
   * CSS クラス: `cmp-teaser--primary-color`
* **有効なCSSクラス**:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### ヒーローセカンダリ

![ヒーローセカンダリ](assets/hero-secondary.png)

* **レイアウトスタイル**
   * スタイル名：**Promo**
   * CSS クラス: `cmp-teaser--hero`
* **表示スタイル**
   * スタイル名：**黄色**
   * CSS クラス: `cmp-teaser--secondary-color`
* **有効なCSSクラス**:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## その他のリソース {#additional-resources}

* [スタイルシステムドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEMクライアントライブラリの作成](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（ブロック要素修飾子）ドキュメントWebサイト](https://getbem.com/)
* [LESSドキュメントWebサイト](https://lesscss.org/)
* [jQuery Webサイト](https://jquery.com/)
