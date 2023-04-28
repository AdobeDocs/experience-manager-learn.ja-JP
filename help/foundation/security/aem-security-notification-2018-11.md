---
title: AEMセキュリティ通知（2018年11月）
seo-title: AEM Security Notification (November 2018)
description: AEM Experience Manager のセキュリティ通知 Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: ht
source-wordcount: '428'
ht-degree: 100%

---

# AEMセキュリティ通知（2018年11月）

## 概要

この記事では、AEM で最近報告された、新しい脆弱性と古い脆弱性のいくつかについて説明します。 確認された脆弱性のほとんどは AEM 製品の既知の問題であり、緩和策は以前に確認されていることにご注意ください。新しい脆弱性には新しい Dispatcher バージョンが利用可能です。Adobe はまた、[AEM セキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/security-checklist.html) を完成させ、関連するガイドラインに従うことをお勧めします。

## アクションが必要です

* AEM デプロイメントは、最新バージョンの Dispatcher を使用して開始する必要があります。
* Dispatcher のセキュリティルールは、推奨される設定に従って適用する必要があります。
* この [AEM セキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/security-checklist.html) は、AEM のデプロイメント向けに完了する必要があります。

## 脆弱性と解決策

| 問題 | 解決策 | リンク |
|-------|------------|-------|
| AEM Dispatcher ルールをバイパス | 最新バージョンの Dispatcher（4.3.1）をインストールし、推奨される Dispatcher の設定に従います。 | [AEM Dispatcher リリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)および [Dispatcher の設定](https://helpx.adobe.com/jp/experience-manager/dispatcher/using/dispatcher-configuration.html)を参照してください。 |
| Dispatcher ルールを回避するために使用される可能性のある URL フィルターバイパスの脆弱性 - CVE-2016-0957 | これは古いバージョンの Dispatcher では修正されましたが、最新バージョンの Dispatcher（4.3.1）をインストールし、推奨される Dispatcher 設定に従うことをお勧めします。 | [AEM Dispatcher リリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)および [Dispatcher の設定](https://helpx.adobe.com/jp/experience-manager/dispatcher/using/dispatcher-configuration.html)を参照してください。 |
| 保存された SWF ファイルに関連する XSS の脆弱性 | これは、以前にリリースされたセキュリティ修正で対処されています。 | [AEM セキュリティ速報 APSB18-10](https://helpx.adobe.com/jp/security/products/experience-manager/apsb18-10.html) を参照してください。 |
| パスワード関連の攻撃 | より強力なパスワードについては、セキュリティチェックリストの推奨事項に従ってください。 | [AEM セキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/security-checklist.html) を参照してください。 |
| 匿名ユーザーに対するディスク使用量の漏洩 | この問題は AEM 6.1 以降で解決されました。AEM 6.0 の場合、標準提供の権限を変更して、より制限を厳しくすることができます。 | AEM 6.1 以降用の[リリースノート](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja#previous-updates)を参照してください。 |
| 匿名ユーザーに対するオープンソーシャルプロキシの漏洩 | これは 6.0 SP2 以降のバージョンで解決されました。 | AEM 6.1 以前の[リリースノート](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja#previous-updates)を参照してください。 |
| 実稼動インスタンスでの CRX Explorer アクセス | CRX Explorer アクセスの管理は、セキュリティチェックリストで既にカバーされています。CRX Explorer は実稼動オーサーおよびパブリッシュから削除する必要があり、削除されていない場合はセキュリティヘルスチェックが報告します。 | [AEM セキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-4/sites/administering/using/security-checklist.html)を参照してください。 |
| BGServlet が漏洩しました | これは AEM 6.2 以降で解決されています。 | [AEM 6.2 リリースノート](https://helpx.adobe.com/jp/experience-manager/6-2/release-notes.html)を参照してください。 |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher ユーザーガイド](https://helpx.adobe.com/jp/experience-manager/brand-portal/user-guide.html)
>* [AEM Dispatcher リリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM セキュリティ速報](https://helpx.adobe.com/jp/security.html#experience-manager)

