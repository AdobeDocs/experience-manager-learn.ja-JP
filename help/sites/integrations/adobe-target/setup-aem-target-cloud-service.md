---
title: Adobe TargetCloud Serviceアカウントの作成
description: Cloud ServiceとAdobeのIMS認証を使用して、Adobe Experience ManagerをCloud ServiceとしてAdobe Targetに統合する方法に関する手順説明を順を追って説明します。
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---


# Adobe TargetCloud Serviceアカウントの作成 {#adobe-target-cloud-service}

Cloud ServiceとAdobeのIMS認証を使用して、Adobe Experience ManagerをCloud ServiceとしてAdobe Targetと統合する方法に関する手順説明を順を追って説明します。

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>このビデオに示すAdobe TargetCloud Servicesの設定に関する既知の問題があります。 このビデオと同じ手順に従う代わりに、 [レガシーAdobe TargetCloud Servicesの設定を使用することをお勧めします](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)。

## 一般的な問題

エクスペリエンスフラグメントをAdobeターゲットに書き出す際に、管理コンソールのターゲット統合に適切な権限がない場合、次に示すようなエラーが表示される場合があります。

**UIエラー**![ターゲットAPI UIエラー](assets/error-target-offer.png)

**ログエラー**![ターゲットAPIコンソールエラー](assets/target-console-error.png)


**解決策**

1. [Admin Consoleに移動](https://adminconsole.adobe.com/)
2. 製品を選択/Adobe Target/製品プロファイル
3. 「統合」タブで、統合(AdobeI/Oプロジェクト)を選択します。
4. エディタまたは承認者の役割を割り当てます。

![ターゲットAPIエラー](assets/target-permissions.png)

ターゲット統合に適切な権限を追加すると、上記の問題の解決に役立ちます。