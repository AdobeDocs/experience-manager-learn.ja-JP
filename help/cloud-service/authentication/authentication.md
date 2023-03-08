---
title: AEMでの認証as a Cloud Service
description: AEMas a Cloud Serviceのでの認証について説明します。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
source-git-commit: 171daf292355203b903a6c29bebad9216dfd95b7
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 3%

---

# AEMas a Cloud Service認証

AEM as a Cloud Serviceは、複数の認証オプションをサポートし、サービスの種類によって異なります。

|  | コンテンツ作成者 | AEM パブリッシュ |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| ・ [Adobe IMS経由の SAML 2.0](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [シングルサインオン (SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [トークン認証](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## 認証オプション

認証方法の設定方法と使用方法の詳細については、以下の対応するリンクをクリックしてください。

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Adobe Admin ConsoleでAdobe IMSを使用して、AEM オーサーのアクセスを管理します。
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        AEM パブリッシュサービスの SAML 2.0 統合を使用して、Web サイトのユーザーを IDP に対して認証します。
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="トークン" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">トークン認証</a></strong></div>
      <p>
        API サービストークンを使用して、アプリケーションおよびミドルウェアがAEMに対して認証を行うことを許可します。
      </p>
    </td>   
  </tr>
</table>
