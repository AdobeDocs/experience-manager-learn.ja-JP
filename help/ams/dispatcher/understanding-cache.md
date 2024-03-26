---
title: Dispatcher とキャッシュについて
description: Dispatcher モジュールがキャッシュをどのように動作させるかを理解します。
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 491
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: ht
source-wordcount: '1708'
ht-degree: 100%

---

# キャッシュについて

[目次](./overview.md)

[&lt;- 前：設定ファイルの説明](./explanation-config-files.md)

このドキュメントでは、Dispatcherのキャッシュがどのように行われ、どのように設定できるかを説明します。

## ディレクトリのキャッシュ

ベースラインインストールでは、次のデフォルトのキャッシュディレクトリを使用します。

- 作成者
   - `/mnt/var/www/author`
- パブリッシャー
   - `/mnt/var/www/html`

それぞれのリクエストが Dispatcher を通過する際、リクエストは設定されたルールに従い、対象となる項目への応答に応じてローカルにキャッシュされたバージョンを保持します。

>[!NOTE]
>
>Apache が DocumentRoot でファイルを検索する際、どの AEM インスタンスから来たのかがわからないので、公開済みのワークロードとオーサーワークロードを意図的に分離します。したがって、オーサーファームでキャッシュを無効にしても、オーサーの DocumentRoot がパブリッシャーと同じ場合、キャッシュが存在するときにはそのキャッシュのファイルが使用されます。つまり、公開キャッシュのオーサーファイルが使用されることになるため、訪問者にミスマッチで低質なエクスペリエンスが提供されてしまいます。
>
>公開済みのコンテンツごと個別の DocumentRoot ディレクトリを保持するのも、良い考えとは言えません。サイト間でも変わることのない、再キャッシュされた複数の項目（clientlibs など）を作成し、設定した各 DocumentRoot に対してレプリケーションフラッシュエージェントを設定する必要があります。ページをアクティブ化するたびに、フラッシュオーバーヘッド量を増やします。ファイルの名前空間と完全にキャッシュされたパスに依存し、公開済みサイトで複数の DocumentRoot が使用されるのを防ぎます。

## 設定ファイル

Dispatcher は、ファームファイルの「`/cache {`」セクションで、何がキャッシュ可能であると判断されるかを制御します。 
AMS ベースライン設定ファームには、次のようなインクルードが含まれます。


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


キャッシュする対象またはキャッシュしない対象のルールを作成する場合は、[こちら](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#configuring-the-dispatcher-cache-cache)のキュメントを参照してください。


## オーサーのキャッシュ

ユーザーがオーサーのコンテンツをキャッシュしない実装は、多数あります。 
オーサーに対するパフォーマンスと応答性の大幅なアップグレードが見つかりません。

オーサーファームを正しくキャッシュするように設定する際に実行する方法について説明します。

次は、オーサーファームファイルのベースとなるオーサーの「`/cache {`」セクションです。


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

ここでの重要な注意点は、`/docroot` は、オーサーのキャッシュディレクトリに設定されるということです。

>[!NOTE]
>
>オーサーの `.vhost` ファイルの `DocumentRoot` がファームの `/docroot` パラメーターと一致していることを確認してください。

キャッシュルール include ステートメントは、これらのルールを含むファイル`/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any`を含めます。

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

オーサーシナリオでは、コンテンツは四六時中、意図的に変更されます。頻繁に変更されない項目のみをキャッシュします。
`/libs` はベースライン AEM インストールの一部であり、サービスパック、累積修正パック、アップグレードまたは Hotfix をインストールするまで変更されるため、これをキャッシュするルールを設けました。したがって、これらの要素をキャッシュすることは非常に理にかなっており、サイトを使用するエンドユーザーのオーサーエクスペリエンスに大きなメリットをもたらします。

>[!NOTE]
>
>これらのルールは、<b>`/apps`</b> もキャッシュします。ここには、カスタムアプリケーションコードが格納されています。このインスタンスでコードを開発している場合、ファイルを保存すると、キャッシュされたコピーが提供されるので UI に反映されるかどうかがわからず、混乱を招くことになります。ここでの目的は、コードを AEM にデプロイする場合も頻度を少なくすることで、デプロイメント手順の一部としてオーサーキャッシュをクリアする必要があります。ここでも、エンドユーザーにとって、キャッシュ可能なコードを迅速に実行できるという、大きなメリットがあります。

## ServeOnStale（別名 Serve on Stale／SOS）

これは Dispatcher の中でも貴重な機能の一つです。パブリッシャーが読み込み中であるか、応答しなくなった場合、通常は 502 または 503 HTTP 応答コードが返されます。そのような場合にこの機能が有効になっていると、Dispatcher はたとえ最新のコピーでなくても、次善としてキャッシュに残っているコンテンツを提供するように指示されます。何の機能もないエラーメッセージを表示するよりも、できる限りのものを提供したほうがよいでしょう。

>[!NOTE]
>
>パブリッシャーレンダラーでソケットタイムアウトまたは 500 エラーメッセージが表示された場合、この機能はトリガーしないことに注意が必要です。AEM に到達できないだけの場合、この機能は何も行いません

この設定は任意のファームで設定できますが、パブリッシュファームファイルに適用すると効果的です。次に、ファームファイルで有効になる機能の構文例を示します。

```
/cache { 
    /serveStaleOnError "1"
```

## クエリパラメーター／引数を含むページのキャッシュ

>[!NOTE]
>
>Dispatcher モジュールの通常の動作の 1 つは、要求の URI にクエリパラメーターが含まれている場合で（通常、`/content/page.html?myquery=value`）、ファイルのキャッシュをスキップし、AEM インスタンスに直接移動します。このリクエストは動的ページの可能性があるので、キャッシュしないでください。キャッシュの効率に悪影響を与える可能性があります。

重要なクエリパラメーターがサイトパフォーマンスにどのような影響を与えるかについては、こちらの[記事](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner)を参照してください。

デフォルトで、`ignoreUrlParams` のルールが `*` を許可するように設定します。つまり、すべてのクエリパラメーターは無視され、使用するパラメーターに関係なく、すべてのページをキャッシュできます。

次に、URI 内の引数参照を使用してユーザーがどこから来たかを知る、ソーシャルメディアのディープリンク参照メカニズムを構築した例を示します。

*無視可能な例：*

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

ページは 100%キャッシュ可能ですが、引数が存在するのでキャッシュされません。 
`ignoreUrlParams` を許可リストに設定すると、この問題の修正に役立ちます。

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Dispatcher はリクエストを検知すると、リクエストに `?` 参照の `query` パラメーターがあるという事実を無視して、引き続きページをキャッシュします。

<b>動的な例：</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

ページを変更するクエリパラメーターがある場合は、そのクエリパラメーターを無視リストから除外し、ページのキャッシュを再び解除する必要があることに注意してください。例えば、クエリパラメーターを使用する検索ページでは、レンダリングされる生の HTML が変更されます。

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

初めて `/search.html?q=fruit` にアクセスした場合、果物を表示している結果とともに HTML がキャッシュされます。

2 回目に `/search.html?q=vegetables` を訪問すると、果物の結果が示されます。
これは、キャッシュに関して、`q` のクエリパラメーターが無視されることが原因です。  この問題を回避するには、クエリパラメーターに基づいて異なる HTML をレンダリングするページを控え、それらのキャッシュを拒否する必要があります。

例：

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

JavaScript 経由でクエリパラメーターを使用するページは、この設定のパラメーターを無視して、引き続き完全に機能します。  HTML ファイルは保存時に変更されないので、JavaScript を使用して、ローカルブラウザーでブラウザーの DOM をリアルタイムで更新します。  つまり、JavaScript を使用してクエリパラメーターを使用する場合、ページのキャッシュ時にこのパラメーターを無視できる可能性が高くなります。  そのページをキャッシュできるようにして、パフォーマンスを向上させることができます。

>[!NOTE]
>
>これらのページを追跡するには、ある程度良い状態に維持する必要がありますが、パフォーマンスが向上するというメリットも得られます。CSE に依頼し、web サイトのトラフィックに関するレポートを実行し、クエリパラメーターを使用して過去 90 日間のすべてのページのリストを提供してもらいます。受け取ったリストを分析し、どのページを確認し、どのクエリパラメータを無視すべきでないかを把握します。

## 応答ヘッダーのキャッシュ

Dispatcher が `.html` ページと clientlibs（つまり、`.js`、`.css`）をキャッシュすることは明らかですが、名前が同じで `.h` ファイル拡張子が付いたファイルのコンテンツと一緒に、特定の応答ヘッダーもキャッシュできることをご存知でしょうか 。これにより、次回の応答がコンテンツに対してだけでなく、キャッシュからの応答ヘッダーに対しても行われます。

AEM は単なる UTF-8 エンコーディング以外も処理できます

アイテムには、キャッシュ TTL のエンコーディングの詳細や最終変更日のタイムスタンプを制御するのに役立つ特別なヘッダーが含まれている場合があります。

キャッシュ時のこれらの値はデフォルトで削除され、Apache httpd web サーバーは、通常のファイル処理メソッドでアセットを処理する独自のジョブを実行します。通常は、ファイル拡張子に基づく MIME タイプの推測に限定されます。

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


この例では、CDN が探すヘッダーを提供するように AEM を設定し、そのキャッシュを無効にするタイミングが分かるようにします。つまり、AEM では、ヘッダーに基づいて無効化するファイルを適切に指定できるようになりました。

>[!NOTE]
>
>正規表現やグロブマッチングは使用できません。キャッシュするヘッダーのリテラルリストです。キャッシュするリテラルヘッダーのリストのみを配置します。

## 猶予期間の自動無効化

多くのページアクティベーションを行うオーサーのアクティビティが多い AEM システムでは、無効化が繰り返し発生する競合状態になる場合があります。 大量に繰り返されるフラッシュリクエストは不要で、猶予期間がクリアされるまでフラッシュを繰り返さないよう、ある程度の許容値を構築できます。

### この仕組みの例：

無効にするリクエストが 5 つある場合 `/content/exampleco/en/` すべては 3 秒以内に発生します。

この機能をオフにすると、キャッシュディレクトリ `/content/exampleco/en/` が 5 回無効になります

この機能をオンにして 5 秒に設定すると、キャッシュディレクトリ `/content/exampleco/en/` が <b>1 回</b>無効になります

次に、この機能を 5 秒の猶予期間用に設定する構文の例を示します。

```
/cache { 
    /gracePeriod "5"
```

## TTL ベースの無効化

Dispatcher モジュールの新しい機能は、キャッシュされた項目の `Time To Live (TTL)` に基づいた無効化オプション。アイテムがキャッシュされると、キャッシュ制御ヘッダーが存在するかどうかが調べられ、名前が同じで `.ttl` 拡張子が付いたファイルがキャッシュディレクトリに作成されます。

ファーム設定ファイルで設定する機能の例を次に示します。

```
/cache { 
    /enableTTL "1"
```

>[!NOTE]
>
>AEM で TTL ヘッダーを送信するように設定して、Dispatcher がヘッダーを保持するようにする必要があります。この機能を切り替えると、AEM がキャッシュ制御ヘッダーを送信したファイルを削除するタイミングを Dispatcher が把握できるようになります。 AEM が TTL ヘッダーの送信を開始しない場合、Dispatcher は特別な処理を実行しません。

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

公開したサイトをできるだけ貪欲なものにし、すべてをキャッシュしたいと考えています。

キャッシュ時にエクスペリエンスが損なわれる要素がある場合は、ルールを追加して、その項目をキャッシュするオプションを削除できます。 上の例で示すように、csrf トークンはキャッシュされず、除外されています。 これらのルールの作成に関する詳細は、[こちら](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ja#configuring-the-dispatcher-cache-cache)を参照してください 

[次へ／変数の使用と理解](./variables.md)
