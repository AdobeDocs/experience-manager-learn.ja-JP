---
title: CSRF 保護
description: 認証済みユーザーに許可されたPOST、PUT、および削除リクエストにAEM CSRF トークンを生成し、追加する方法について説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# CSRF 保護

認証済みユーザーに許可されたPOST、PUT、および削除リクエストにAEM CSRF トークンを生成し、追加する方法について説明します。

AEMには有効な CSRF トークンの送信が必要です __認証済み__ __POST__、__PUT、または __DELETE__ AEM オーサーサービスとパブリッシュサービスの両方に対する HTTP リクエスト。

CSRF トークンは、 __GET__ リクエストまたは __匿名__ リクエスト。

CSRF トークンがPOST、PUT、またはDELETEのリクエストと共に送信されない場合、AEMは 403 Forbidden 応答を返し、AEMは次のエラーをログに記録します。

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

詳しくは、 [AEM CSRF 保護の詳細に関するドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## CSRF クライアントライブラリ

AEMは、コアプロトタイプ関数のパッチを適用することで、CSRF トークン XHR およびフォームPOSTリクエストの生成と追加に使用できるクライアントライブラリを提供します。 機能は、 `granite.csrf.standalone` クライアントライブラリカテゴリ。

この方法を使用するには、 `granite.csrf.standalone` ページ上で読み込まれるクライアントライブラリへの依存関係として。 例えば、 `wknd.site` クライアントライブラリカテゴリ、追加 `granite.csrf.standalone` ページ上で読み込まれるクライアントライブラリへの依存関係として。

## CSRF 保護を使用したカスタムフォームの送信

次の [`granite.csrf.standalone` クライアントライブラリ](#csrf-client-library) を使用すると、CSRF トークンを手動でフォーム送信に追加することができます。 次の例は、フォーム送信に CSRF トークンを追加する方法を示しています。

このコードスニペットは、フォーム送信時に、AEMから CSRF トークンを取得し、という名前のフォーム入力に追加する方法を示しています。 `:cq_csrf_token`. CSRF トークンの有効期間は短いので、フォームの送信直前に CSRF トークンを取得して設定し、有効性を確保することをお勧めします。

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('afterend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## CSRF 保護を使用して取得

次の [`granite.csrf.standalone` クライアントライブラリ](#csrf-client-library) は、使用事例に適していません。XHR に手動で CSRF トークンを追加したり、リクエストを取得したりできます。 次の例は、CSRF トークンを XHR に追加し、fetch で作成した XHR に追加する方法を示しています。

このコードスニペットは、AEMから CSRF トークンを取得し、取得リクエストの `CSRF-Token` HTTP リクエストヘッダー。 CSRF トークンの有効期間は短いので、フェッチリクエストが実行される直前に CSRF トークンを取得して設定し、有効性を確保することをお勧めします。

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Dispatcher 設定

AEM パブリッシュサービスで CSRF トークンを使用する場合は、CSRF トークンエンドポイントへのGET要求を許可するように Dispatcher 設定を更新する必要があります。 次の設定は、AEM パブリッシュサービスの CSRF トークンエンドポイントに対するGETリクエストを許可します。 この設定が追加されない場合、CSRF トークンエンドポイントは 404 Not Found 応答を返します。

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
