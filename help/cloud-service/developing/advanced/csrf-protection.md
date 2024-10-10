---
title: CSRF 対策
description: AEM CSRF トークンを生成し、認証済みユーザーに対して AEM への許可された POST、PUT、DELETE リクエストに追加する方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 125
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '439'
ht-degree: 100%

---

# CSRF 対策

AEM CSRF トークンを生成し、認証済みユーザーに対して AEM への許可された POST、PUT、DELETE リクエストに追加する方法を説明します。

AEM では、__認証された__ __POST__、__PUT、__DELETE__ HTTP リクエストに対して、AEM オーサーサービスとパブリッシュサービスの両方に有効な CSRF トークンを送信する必要があります。

CSRF トークンは、__GET__ リクエストや&#x200B;__匿名__&#x200B;リクエストには必要ありません。

CSRF トークンが POST、PUT、DELETE リクエストで送信されない場合、AEM は 403 Forbidden 応答を返し、次のエラーをログに記録します。

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

[AEM の CSRF 対策について詳しくは、ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=ja)を参照してください。


## CSRF クライアントライブラリ

AEM は、コアプロトタイプ関数にパッチを適用することで、CSRF トークン XHR を生成および追加し、POST リクエストを形成するために使用できるクライアントライブラリを提供します。この機能は、`granite.csrf.standalone` クライアントライブラリカテゴリによって提供されます。

この方法を使用するには、ページに読み込まれるクライアントライブラリに依存関係として `granite.csrf.standalone` を追加します。例えば、`wknd.site` クライアントライブラリカテゴリを使用している場合は、ページに読み込まれるクライアントライブラリに依存関係として `granite.csrf.standalone` を追加します。

## CSRF 対策を備えたカスタムフォームの送信

[`granite.csrf.standalone` クライアントライブラリ](#csrf-client-library)の使用がユースケースに適さない場合は、フォーム送信に CSRF トークンを手動で追加できます。次の例は、フォーム送信に CSRF トークンを追加する方法を示しています。

このコードスニペットは、フォーム送信時に AEM から CSRF トークンを取得し、`:cq_csrf_token` という名前のフォーム入力に追加する方法を示しています。CSRF トークンの有効期間は短いので、フォーム送信の直前に CSRF トークンを取得して設定し、その有効期間を確保することをお勧めします。

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
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## CSRF 対策による取得

[`granite.csrf.standalone` クライアントライブラリ](#csrf-client-library)の使用がユースケースに適さない場合は、CSRF トークンを XHR に手動で追加するか、リクエストを取得できます。次の例は、取得で生成した XHR に CSRF トークンを追加する方法を示しています。

このコードスニペットは、AEM から CSRF トークンを取得し、取得リクエストの `CSRF-Token` HTTP リクエストヘッダーに追加する方法を示しています。CSRF トークンの有効期間は短いので、取得リクエストの直前に CSRF トークンを取得して設定し、その有効期間を確保することをお勧めします。

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

AEM パブリッシュサービスで CSRF トークンを使用する場合、CSRF トークンエンドポイントへの GET リクエストを許可するように Dispatcher 設定を更新する必要があります。次の設定では、AEM パブリッシュサービスの CSRF トークンエンドポイントへの GET リクエストを許可します。 この設定が追加されない場合、CSRF トークンエンドポイントは 404 Not Found 応答を返します。

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
