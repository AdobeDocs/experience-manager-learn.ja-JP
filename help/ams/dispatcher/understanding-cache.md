---
title: Dispatcher とキャッシュについて
description: Dispatcher モジュールがキャッシュをどのように動作させるかを理解します。
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# キャッシュについて

[目次](./overview.md)

[&lt; — 前：設定ファイルの説明](./explanation-config-files.md)

このドキュメントでは、Dispatcher のキャッシュの発生方法と設定方法を説明します

## ディレクトリのキャッシュ

ベースラインインストールでは、次のデフォルトのキャッシュディレクトリを使用します

- オーサー
   - `/mnt/var/www/author`
- 発行者
   - `/mnt/var/www/html`

各要求が Dispatcher を走査する際、要求は設定されたルールに従い、対象となる項目への応答に応じてローカルにキャッシュされたバージョンを保持します

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

Apache が DocumentRoot でファイルを検索する際に、どのAEMインスタンスから来たのかがわからないので、公開済みのワークロードをオーサーワークロードとは意図的に別に保ちます。 したがって、オーサーファームでキャッシュが無効になっている場合でも、オーサーの DocumentRoot がパブリッシャーと同じ場合は、キャッシュのファイルが存在するときにそのキャッシュから提供されます。 つまり、公開されたキャッシュからオーサーファイルを提供し、訪問者にとって非常にひどいミックスマッチエクスペリエンスを提供します。

公開されているコンテンツごとに異なる DocumentRoot ディレクトリを別々に保持するのも、非常に悪い考えです。 clientlibs などのサイト間で異なることのない、再キャッシュされた複数の項目を作成し、設定した各 DocumentRoot に対してレプリケーションフラッシュエージェントを設定する必要があります。 ページをアクティベートするたびに、ヘッドの上のフラッシュ量を増やします。 ファイルの名前空間と完全にキャッシュされたパスに依存し、公開済みサイトで複数の DocumentRoot が使用されるのを防ぎます。
</div>

## 設定ファイル

Dispatcher は、でキャッシュ可能と見なされるものを制御します。 `/cache {` セクションに含める必要があります。 
AMS ベースライン設定ファームには、次のようなインクルードが含まれます。


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


キャッシュする対象またはキャッシュしない対象のルールを作成する場合は、ドキュメントを参照してください [ここ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## 作成者のキャッシュ

ユーザーがオーサーコンテンツをキャッシュしない実装は、多数あります。 
作成者に対するパフォーマンスと応答性の大幅なアップグレードが見つかりません。

オーサーファームを正しくキャッシュするように設定する際に実行する方法について説明します。

基本作成者は次のとおりです `/cache {` オーサーファームファイルのセクション：


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

ここで注意すべき重要な点は、 `/docroot` は、オーサーのキャッシュディレクトリに設定されます。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

次を確認します。 `DocumentRoot` 著者の `.vhost` ファイルはファームと一致します `/docroot` パラメータ
</div>

キャッシュルール include ステートメントには、ファイルが含まれます `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` これには次のルールが含まれます。

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

オーサーシナリオでは、コンテンツは常に時間と目的に変更されます。 頻繁に変更されない項目のみをキャッシュします。
キャッシュするルールがあります `/libs` これらは、AEMのベースラインインストールの一部であり、Service Pack、累積 Fix Pack、アップグレード、またはホットフィックスをインストールするまで変更されます。 したがって、これらの要素をキャッシュすることは非常に理にかなっており、サイトを使用するエンドユーザーのオーサーエクスペリエンスの大きなメリットをもたらします。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

これらのルールは、 <b>`/apps`</b> カスタムアプリケーションコードが存在する場所です。 このインスタンスでコードを開発している場合、ファイルを保存する際に非常に混乱が生じ、キャッシュされたコピーが提供されるので、UI に反映されるかどうかがわかりません。 ここでの目的は、コードをAEMにデプロイする場合も頻繁ではなく、デプロイメント手順の一部としてオーサーキャッシュをクリアする必要があるということです。 このメリットも大きく、エンドユーザーに対してキャッシュ可能なコードの実行を高速化できる点です。
</div>


## ServeOnStale （SOS で提供される古い/SOS）

これは、Dispatcher の機能の 1 つの宝石です。 パブリッシャーが読み込み中であるか、応答しなくなった場合、通常は 502 または 503 http 応答コードがスローされます。 これが発生し、この機能が有効になっている場合、Dispatcher は、新しいコピーでなくても、キャッシュ内のコンテンツが存在するものをベストエフォートとして提供するように指示されます。 機能を提供しないエラーメッセージを表示するのではなく、表示している場合は何かを提供する方が良いです。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

パブリッシャーレンダラーがソケットタイムアウトまたは 500 エラーメッセージを持っている場合、この機能はトリガーしません。 AEMに単に到達できない場合、この機能は何もしません
</div>

この設定は任意のファームで設定できますが、パブリッシュファームファイルに適用すると効果的です。 次に、ファームファイルで有効になっている機能の構文例を示します。

```
/cache { 
    /serveStaleOnError "1"
```

## クエリパラメーター/引数を含むページのキャッシュ

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

Dispatcher モジュールの通常の動作の 1 つは、要求の URI にクエリパラメーターが含まれている場合です ( 通常、 `/content/page.html?myquery=value`) ファイルのキャッシュをスキップし、AEMインスタンスに直接移動します。 このリクエストは動的ページであると考えられており、キャッシュしないでください。 これは、キャッシュの効率に悪影響を与える可能性があります。
</div>
<br/>

参照 [記事](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) サイトのパフォーマンスに与える重要なクエリパラメーターの影響を示します。

デフォルトでは、 `ignoreUrlParams` 許可するルール `*`.  つまり、すべてのクエリパラメーターは無視され、使用するパラメーターに関係なく、すべてのページをキャッシュできます。

次に、URI 内の引数参照を使用してユーザーがどこから来たかを知る、ソーシャルメディアのディープリンク参照メカニズムを構築した例を示します。

<b>無視可能な例：</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

ページは 100%キャッシュ可能ですが、引数が存在するのでキャッシュされません。 
の設定 `ignoreUrlParams` as a許可リストがこの問題の修正に役立ちます：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

現在は、Dispatcher が要求を見ると、要求に `query` のパラメータ `?` ページを参照し、引き続きキャッシュします。

<b>動的な例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

ページを変更するクエリパラメーターがある場合は、そのクエリーパラメーターを無視されたリストから除外し、ページのキャッシュを再び解除する必要があることに注意してください。  例えば、クエリパラメーターを使用する検索ページでは、レンダリングされる生の HTML が変更されます。

各検索の HTML ソースは次のようになります。

 `/search.html?q=fruit`：

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

 `/search.html?q=vegetables`：

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

次にアクセスした場合 `/search.html?q=fruit` 最初に、html をキャッシュし、結果が実を示すようにします。

次に、 `/search.html?q=vegetables` 二つ目は、実の結果を示すことだ。
これは、のクエリパラメーターが原因です。 `q` キャッシュに関しては、が無視されます。  この問題を回避するには、クエリパラメーターに基づいて異なるHTMLをレンダリングするページを控え、それらのキャッシュを拒否する必要があります。

例：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

JavaScript 経由でクエリーパラメーターを使用するページは、この設定のパラメーターを無視して、引き続き完全に機能します。  HTML ファイルは保存時に変更されないので、  JavaScript を使用して、ローカルブラウザーでブラウザーの DOM をリアルタイムで更新します。  つまり、JavaScript を使用してクエリパラメーターを使用する場合、ページのキャッシュ時にこのパラメーターを無視できる可能性が高くなります。  そのページでパフォーマンスの向上をキャッシュし、楽しむことができます。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

これらのページを追跡するには、何らかの維持が必要ですが、パフォーマンスの向上に十分な価値があります。  CSE に対し、過去 90 日間のクエリパラメーターを使用したすべてのページのリストを提供するように Web サイトトラフィックに対してレポートを実行するように依頼して、どのページを分析し、どのクエリパラメーターを無視するかを確認できます
</div>
<br/>

## 応答ヘッダーのキャッシュ

Dispatcher がキャッシュするのは明らかです。 `.html` ページと clientlibs ( `.js`, `.css`) ですが、同じ名前で、 `.h` ファイル拡張子。 これにより、次の応答がコンテンツに対する応答だけでなく、キャッシュからの応答ヘッダーに対しておこなわれます。

AEMは単なる UTF-8 エンコーディング以上を処理できます

アイテムには、キャッシュ TTL のエンコーディングの詳細や最終変更日のタイムスタンプを制御するのに役立つ特別なヘッダーが含まれている場合があります。

キャッシュ時のこれらの値はデフォルトで削除され、Apache httpd Web サーバーは、通常のファイル処理メソッドでアセットを処理する独自のジョブを実行します。通常は、ファイル拡張子に基づく MIME タイプ推測に制限されます。

Dispatcher がアセットと目的のヘッダーをキャッシュしている場合は、適切なエクスペリエンスを公開し、すべての詳細がクライアントブラウザーに表示されるようにします。

次に、キャッシュするヘッダーが指定されたファームの例を示します。

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


この例では、CDN が探すヘッダーを提供するようにAEMを設定し、そのヘッダーがキャッシュを無効にするタイミングを知っています。 つまり、AEMでは、ヘッダーに基づいて無効化するファイルを適切に指定できるようになりました。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>

正規表現や glob 照合は使用できないことに注意してください。 キャッシュするヘッダーのリテラルリストです。 キャッシュするリテラルヘッダーのリストのみを配置します。
</div>


## 猶予期間の自動無効化

多くのページアクティベーションをおこなう作成者のアクティビティが多いAEMシステムでは、無効化が繰り返し発生する競合状態になる場合があります。 大量に繰り返されるフラッシュリクエストは不要で、猶予期間がクリアされるまでフラッシュを繰り返さないよう、ある程度の許容値を構築できます。

### この仕組みの例：

無効にするリクエストが 5 つある場合 `/content/exampleco/en/` すべては 3 秒以内に発生します。

この機能をオフにすると、キャッシュディレクトリを無効にできます `/content/exampleco/en/` 5 回

この機能をオンにして 5 秒に設定すると、キャッシュディレクトリが無効になります `/content/exampleco/en/` <b>1 回</b>

次に、この機能を 5 秒の猶予期間用に設定する構文の例を示します。

```
/cache { 
    /gracePeriod "5"
```

## TTL ベースの無効化

Dispatcher モジュールの新しい機能は、次のとおりです。 `Time To Live (TTL)` キャッシュされた項目の無効化オプションに基づいています。 アイテムがキャッシュされると、キャッシュ制御ヘッダーが存在するかどうかを調べ、同じ名前と `.ttl` 拡張子。

ファーム設定ファイルで設定する機能の例を次に示します。

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
AEMが TTL ヘッダーを送信するように設定して、Dispatcher がヘッダーを保持するようにする必要があることに注意してください。 この機能を切り替えると、AEMがキャッシュ制御ヘッダーを送信したファイルを削除するタイミングを Dispatcher が把握できるようになります。 AEMが TTL ヘッダーの送信を開始しない場合、Dispatcher はここで特別な処理を実行しません。
</div>

## キャッシュフィルタールール

パブリッシャー上でキャッシュする要素のベースライン設定の例を次に示します。

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

公開したサイトをできるだけ貪欲にし、すべてをキャッシュしたいと考えています。

キャッシュ時にエクスペリエンスを壊す要素がある場合は、ルールを追加して、その項目をキャッシュするオプションを削除できます。 上の例で示すように、csrf トークンはキャッシュされず、除外されています。 これらのルールの作成に関する詳細は、こちらを参照してください [ここ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[次へ —> 変数の使用と理解](./variables.md)