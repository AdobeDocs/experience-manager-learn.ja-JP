---
title: AEMセキュリティ通知（2018年11月）
seo-title: AEMセキュリティ通知（2018年11月）
description: AEMExperience Managerセキュリティ通知ディスパッチャー
seo-description: AEMExperience Managerセキュリティ通知ディスパッチャー
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 17%

---


# AEMセキュリティ通知（2018年11月）

## 概要

この記事では、AEMで最近報告された、いくつかの古い脆弱性について説明します。 AEM製品に関しては、既に確認されている脆弱性のほとんどが既知の問題であり、脆弱性の緩和策が事前に特定されていることに注意してください。新しい脆弱性には、新しいディスパッチャーバージョンが使用できます。 また、Adobeは、[AEMセキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/security-checklist.html)を完成させ、関連ガイドラインに従うようお客様に促します。

## アクションが必要です

* AEMのデプロイメントでは、最新バージョンのDispatcherを使用して開始を行う必要があります。
* 推奨される設定に従って、ディスパッチャーのセキュリティルールを適用する必要があります。
* AEMの展開用に、[AEMセキュリティチェックリスト](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)を完了する必要があります。

## 脆弱性と解決

| OS クリップボードと内部 AEM クリップボードを使用した     | 解決方法 | リンク |
|-------|------------|-------|
| AEMディスパッチャールールのバイパス | 最新バージョンのDispatcher(4.3.1)をインストールし、推奨されるディスパッチャー設定に従います。 | 「[AEM Dispatcherリリースノート](https://helpx.adobe.com/jp/experience-manager/dispatcher/release-notes.html)」および「[Dispatcher](https://helpx.adobe.com/jp/experience-manager/dispatcher/using/dispatcher-configuration.html)の設定」を参照してください。 |
| ディスパッチャールールを回避するために使用される可能性のある、URLフィルターバイパス脆弱性 — CVE-2016-0957 | これは古いバージョンのDispatcherで修正されましたが、現在は、最新バージョンのDispatcher(4.3.1)をインストールし、推奨されるDispatcher設定に従うことをお勧めします。 | 「[AEM Dispatcherリリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)」および「[Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)の設定」を参照してください。 |
| 保存されたSWFファイルに関連するXSS脆弱性 | この問題は、以前にリリースしたセキュリティ修正で解決されました。 | [AEMセキュリティ速報APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)を参照してください。 |
| パスワードに関する悪用 | セキュリティチェックリストの推奨事項に従って、より強力なパスワードを入手してください。 | [AEMセキュリティチェックリスト](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)を参照 |
| 匿名ユーザーに対するディスク使用量の増加 | この問題はAEM 6.1以降で解決されました。AEM 6.0では、初期設定の権限を変更して、より厳しくすることができます。 | AEM 6.1以前のバージョンの場合は、[リリースノート](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja#previous-updates)を参照してください。 |
| 匿名ユーザーに対するオープンソーシャルプロキシの公開 | この問題は6.0 SP2以降のバージョンで解決されました。 | AEM 6.1以前のバージョンの場合は、[リリースノート](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)を参照してください。 |
| 実稼働インスタンスでのCRX Explorerアクセス | CRX Explorerのアクセスの管理は、セキュリティチェックリストに既に記載されています。CRX Explorerは、実稼働用の作成者および発行者から削除する必要があります。削除しない場合は、セキュリティ正常性チェックで報告されます。 | [AEMセキュリティチェックリスト](https://helpx.adobe.com/jp/experience-manager/6-4/sites/administering/using/security-checklist.html)を参照してください。 |
| BGServletsが公開される | この問題はAEM 6.2以降で解決されています。 | [AEM 6.2リリースノート](https://helpx.adobe.com/jp/experience-manager/6-2/release-notes.html)を参照 |

>[!MORELIKETHIS]
>
>* [AEM Dispatcherユーザーガイド](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher リリースノート](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEMセキュリティ速報](https://helpx.adobe.com/security.html#experience-manager)

