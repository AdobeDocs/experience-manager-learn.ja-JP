---
title: AEMセキュリティ通知（2018年11月）
seo-title: AEMセキュリティ通知（2018年11月）
description: AEMExperience Managerセキュリティ通知Dispatcher
seo-description: AEMExperience Managerセキュリティ通知Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: セキュリティ
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 17%

---


# AEMセキュリティ通知（2018年11月）

## 概要

この記事では、AEMで最近報告された、いくつかの古い脆弱性について説明します。 特定された脆弱性のほとんどは、AEM製品の既知の問題であり、緩和策は既に特定されています。新しい脆弱性には、新しいDispatcherバージョンを使用できます。 Adobeは、お客様に[AEMセキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/security-checklist.html)を完了し、関連するガイドラインに従うよう求めます。

## アクションが必要です

* AEMデプロイメントは、最新バージョンのDispatcherを使用し始める必要があります。
* 推奨される設定に従って、Dispatcherのセキュリティルールを適用する必要があります。
* AEMデプロイメントの場合は、[AEMセキュリティチェックリスト](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)を完了する必要があります。

## 脆弱性と解決策

| OS クリップボードと内部 AEM クリップボードを使用した     | 解決方法 | リンク |
|-------|------------|-------|
| AEM Dispatcherルールのバイパス | 最新バージョンのDispatcher(4.3.1)をインストールし、推奨されるDispatcher設定に従います。 | [AEM Dispatcherリリースノート](https://helpx.adobe.com/jp/experience-manager/dispatcher/release-notes.html)および[Dispatcherの設定](https://helpx.adobe.com/jp/experience-manager/dispatcher/using/dispatcher-configuration.html)を参照してください。 |
| Dispatcherルールを回避するために使用される可能性があるURLフィルターバイパスの脆弱性 — CVE-2016-0957 | これは古いバージョンのDispatcherで修正されましたが、現在は、最新バージョンのDispatcher(4.3.1)をインストールし、推奨されるDispatcher設定に従うことをお勧めします。 | [AEM Dispatcherリリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)および[Dispatcherの設定](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)を参照してください。 |
| 保存されたSWFファイルに関するXSSの脆弱性 | これは、以前にリリースされたセキュリティ修正で対処されています。 | [AEMセキュリティ情報APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)を参照してください。 |
| パスワード関連の弱点 | より強力なパスワードについては、セキュリティチェックリストの推奨事項に従ってください。 | [AEMセキュリティチェックリスト](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)を参照 |
| 匿名ユーザーに対するディスク使用量の露出 | この問題はAEM 6.1以降で解決されました。AEM 6.0の場合は、標準の権限を変更して、より制限を厳格にすることができます。 | AEM 6.1以前のバージョンについては、[リリースノート](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja#previous-updates)を参照してください。 |
| 匿名ユーザーに対するオープンソーシャルプロキシの公開 | これは6.0 SP2以降のバージョンで解決されています。 | AEM 6.1以前のバージョンについては、[リリースノート](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)を参照してください。 |
| 実稼動インスタンスでのCRX Explorerアクセス | CRX Explorerのアクセスの管理は、セキュリティチェックリストで既に説明されています。CRX Explorerは、実稼動オーサーとパブリッシュから削除する必要があり、削除しない場合はセキュリティヘルスチェックが報告します。 | [AEMセキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-4/sites/administering/using/security-checklist.html)を参照してください。 |
| BGServletsが公開される | これはAEM 6.2以降で解決されています。 | [AEM 6.2リリースノート](https://helpx.adobe.com/jp/experience-manager/6-2/release-notes.html)を参照 |

>[!MORELIKETHIS]
>
>* [AEM Dispatcherユーザーガイド](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher リリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEMセキュリティ速報](https://helpx.adobe.com/security.html#experience-manager)

