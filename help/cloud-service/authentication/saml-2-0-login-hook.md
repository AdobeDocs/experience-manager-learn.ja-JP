---
title: SAML 2.0 カスタムログインフック
description: AEMのカスタム SAML 2.0 ログインフックを開発する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# SAML 2.0 ログインフック

AEMのカスタム SAML 2.0 ログインフックを開発する方法を説明します。 このチュートリアルでは、SAML 2.0 ID プロバイダーと統合し、ユーザーが SAML 資格情報を使用して認証できるカスタムログインフックを作成する手順を説明します。

IDP がユーザープロファイルデータと SAML アサーションのユーザーグループメンバーシップを送信できない場合、またはAEMへの同期前にデータを変換する必要がある場合は、カスタム SAML フックを実装して SAML 認証プロセスを拡張できます。 SAML フックを使用すると、認証フロー中に、グループメンバーシップの割り当てのカスタマイズ、ユーザープロファイル属性の変更、カスタムビジネスロジックの追加を行うことができます。

>[!NOTE]
>カスタム SAML フックは **0}AEM as a Cloud Service} および** AEM LTS} でサポートされてい **す。**&#x200B;この機能は、古いバージョンのAEMでは使用できません。

## 一般的なユースケース

カスタム SAML フックは、次の操作が必要な場合に役立ちます。

+ SAML アサーションで提供される以外の、カスタムビジネスロジックに基づくグループメンバーシップの動的な割り当て
+ AEMに同期する前に、ユーザープロファイルデータを変換またはエンリッチメントする
+ 複雑な SAML 属性構造のAEM ユーザープロパティへのマッピング
+ カスタム承認ルールまたは条件付きグループの割り当ての実装
+ SAML 認証時のカスタムログまたは監査の追加
+ 認証プロセス中に外部システムと統合する

## OSGi サ `SamlHook` ビスインターフェイス

`com.adobe.granite.auth.saml.spi.SamlHook` インターフェイスには、SAML 認証プロセスの様々なステージで呼び出される 2 つのフックメソッドが用意されています。

### `postSamlValidationProcess()` メソッド

このメソッドは **after** と呼ばれ、SAML 応答は検証されましたが、**before** ユーザー同期プロセスが開始されます。 これは、属性の追加や変換など、SAML アサーションデータを変更するのに最適な場所です。

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### ユースケース

+ アサーションにグループメンバーシップを追加します
+ アトリビュート値を同期する前に変換する
+ 外部ソースからのデータでアサーションを強化
+ カスタム・ビジネス・ルールの検証

### `postSyncUserProcess()` メソッド

このメソッドは **after** と呼ばれ、ユーザー同期処理が完了しました。 このフックは、AEM ユーザーが作成または更新された後に、追加の操作を実行するために使用できます。

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### ユースケース

+ 標準同期でカバーされない追加のユーザープロファイルプロパティの更新
+ AEMでのカスタムユーザー関連リソースの作成または更新
+ ユーザー認証後のトリガーワークフローまたは通知
+ カスタム認証イベントのログ

**重要：** リポジトリのユーザープロパティを変更するには、フック実装に次が必要です。

+ `SlingRepository` 経由で挿入された `@Reference` 参照
+ 適切な権限を持つ設定済みの [ サービスユーザー ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) （「Apache Sling Service User Mapper Service Amendment」で設定）
+ try-catch-finally ブロックを使用した適切なセッション管理

## カスタム SAML フックの実装

次の手順は、カスタム SAML フックの作成およびデプロイ方法の概要を示しています。

### SAML フック実装の作成

`com.adobe.granite.auth.saml.spi.SamlHook` インターフェイスを実装する新しい Java クラスをAEM プロジェクトに作成します。

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### SAML フックの設定

SAML フックは、OSGi 設定を使用して、適用先の IDP を指定します。 次の場所にあるプロジェクトに OSGi 設定ファイルを作成します。

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

`idpIdentifier` は、対応する SAML 認証ハンドラー OSGi ファクトリ設定で設定された `idpIdentifier` 値と一致する必要があります（PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`）。 この一致は重要です。SAML フックは、`idpIdentifier` 値が同じ SAML 認証ハンドラーインスタンスに対してのみ呼び出されます。 SAML 認証ハンドラーはファクトリ設定です。つまり、複数のインスタンス（`com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`、`com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json` など）を持つことができ、各フックは `idpIdentifier` を介して特定のハンドラーに関連付けられます。 `service.ranking` プロパティは、複数のフックが設定される場合（値が大きいほど最初に実行される場合）の実行順序を制御します。

### Maven 依存関係の追加

必要な SAML SPI 依存関係をAEM Maven コアプロジェクトの `pom.xml` に追加します。

**AEM as a Cloud Service プロジェクトの場合**、SAML インターフェイスを含むAEM SDK API の依存関係を使用します。

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

`aem-sdk-api` アーティファクトには、`com.adobe.granite.auth.saml.spi.SamlHook` を含む、必要なすべてのAdobe Granite SAML インターフェイスが含まれています。

### サービスユーザーの設定（オプション）

SAML フックで、ユーザープロパティなどのAEM JCR リポジトリ内のコンテンツを変更する必要がある場合（`postSyncUserProcess` の例を参照）、[ サービスユーザー ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) を設定する必要があります。

1. `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json` のプロジェクトにサービスユーザーマッピングを作成します。

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. repoinit スクリプトを作成して、`/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json` でサービスユーザーと権限を定義します。

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

これにより、サービスユーザーに、リポジトリ内のユーザープロパティの読み取りと変更の権限が付与されます。

### AEMへのデプロイ

カスタム SAML フックをAEM as a Cloud Serviceにデプロイします。

1. AEM プロジェクトの構築
1. コードをCloud Manager Git リポジトリにコミットします。
1. フルスタックデプロイメントパイプラインを使用したデプロイ
1. SAML フックは、ユーザーが SAML で認証を行うと自動的にアクティブになります


### 重要な考慮事項

+ **IDP 識別子の一致**: SAML フックで設定された `idpIdentifier` は、SAML 認証ハンドラーファクトリ設定（`idpIdentifier`）の `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>` と完全に一致する必要があります
+ **属性名**：フックで参照される属性名（例：`groupMembership`）が、SAML 認証ハンドラーで設定された属性と一致することを確認します
+ **パフォーマンス**：すべての SAML 認証中に実行されるので、フック実装を軽量に保ちます
+ **エラー処理**：認証に失敗する可能性のある重大なエラーが発生した場合、SAML フック実装は `com.adobe.granite.auth.saml.spi.SamlHookException` をスローする必要があります。 SAML 認証ハンドラーは、これらの例外をキャッチして、`AuthenticationInfo.FAIL_AUTH` を返します。 リポジトリ操作の場合は、常に `RepositoryException` をキャッチし、エラーを適切にログに記録します。 try-catch-finally ブロックを使用して、リソースの適切なクリーンアップを確保します
+ **テスト**：実稼動環境にデプロイする前に、下位の環境でカスタムフックを十分にテストします
+ **複数のフック**：複数の SAML フック実装を設定できます。一致するすべてのフックが実行されます。 OSGi コンポーネントの `service.ranking` プロパティを使用して、実行順序を制御します（順位の高い値が最初に実行されます）。 複数の SAML 認証ハンドラーファクトリ設定（`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`）で SAML フックを再利用するには、それぞれの SAML 認証ハンドラーに一致する異なる `idpIdentifier` を持つ複数のフック設定（OSGi ファクトリ設定）を作成します
+ **セキュリティ**：ビジネスロジックで使用する前に、SAML アサーションのすべてのデータを検証および不要部分を削除します
+ **リポジトリアクセス**:`postSyncUserProcess` でユーザープロパティを変更する場合は、管理セッションではなく、常に適切な権限を持つ [ サービスユーザー ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) を使用します
+ **サービスユーザー権限**:[ サービスユーザー ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) に必要な最小限の権限を付与します（例：完全な管理者権限ではなく、`jcr:read` 上の `rep:write` と `/home/users` のみ）
+ **セッション管理**：例外が発生した場合でも、リポジトリセッションが正しく閉じられるように、常に try-catch-finally ブロックを使用します
+ **User synchronization timing**：ユーザーがOAKに同期された後に `postSyncUserProcess` フックが実行されるので、ユーザーオブジェクトは必ずリポジトリ内のその時点に存在します
