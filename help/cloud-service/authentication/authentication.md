---
title: AEM as a Cloud Service での認証
description: AEM as a Cloud Service での認証について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 28
source-git-commit: 777d5514d0fe79948289e9e24fe20273e48972f8
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 100%

---

# AEM as a Cloud Service の認証

AEM as a Cloud Service は、複数の認証オプションをサポートしており、サービスの種類によって異なります。

|                       | AEM オーサー | AEM パブリッシュ |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✔ |
| • [Adobe IMS を介した SAML 2.0](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=ja#how-to-set-up) | ✔ | ✔ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [シングルサインオン（SSO）](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html?lang=ja#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html?lang=ja#integration-with-an-idp) | ✘ | ✔ |
| [トークン認証](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |
| 基本認証 | ✘ | ✘ |

## 認証オプション

認証方式の設定方法と使用方法について詳しくは、以下の対応するリンクをクリックしてください。

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Adobe Admin Console で Adobe IMS を使用して、AEM オーサーのアクセス権を管理します。
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        AEM パブリッシュサービスの SAML 2.0 統合を使用して、web サイトのユーザーを IDP に対して認証します。
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="トークン" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">トークン認証</a></strong></div>
      <p>
        API サービストークンを使用して、アプリケーションおよびミドルウェアが AEM に対して認証を行うことを許可します。
      </p>
    </td>   
  </tr>
</table>
