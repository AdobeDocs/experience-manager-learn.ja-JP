---
title: レスポンシブブレークポイント
description: AEM レスポンシブページエディター用に新しいレスポンシブブレークポイントを設定する方法を学びます。
version: Experience Manager as a Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# レスポンシブブレークポイント

AEM レスポンシブページエディター用に新しいレスポンシブブレークポイントを設定する方法を学びます。

## CSS ブレークポイントの作成

まず、レスポンシブ AEM サイトが準拠する AEM レスポンシブグリッド CSS に、メディアのブレークポイントを作成します。

`/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` ファイルに、モバイルエミュレーターと併せて使用するブレークポイントを作成します。 各ブレークポイントの `max-width` をメモします。これにより、CSS ブレークポイントが AEM レスポンシブページエディターのブレークポイントにマップされます。

![新しいレスポンシブブレークポイントの作成](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## テンプレートのブレークポイントのカスタマイズ

`ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` ファイルを開き、新しいブレークポイントノードの定義を使用して、`cq:responsive/breakpoints` を更新します。 それぞれの [CSS ブレークポイント](#create-new-css-breakpoints) には、`breakpoints` の下に対応するノードが必要で、その `width` プロパティが CSS ブレークポイントの `max-width` に設定されている必要があります。

![テンプレートのレスポンシブブレークポイントのカスタマイズ](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## エミュレーターの作成

作成者がレスポンシブビューを選択してページエディターで編集できるように、AEM エミュレーターを定義する必要があります。

`/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators` の下にエミュレーターノードを作成 

（例：`/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`）。CRXDE Lite の `/libs/wcm/mobile/components/emulators` から参照エミュレーターノードをコピーし、そのコピーを更新して、ノード定義を迅速化します。

![エミュレーターの新規作成](./assets/responsive-breakpoints/create-new-emulators.jpg)

## デバイスグループの作成

エミュレーターをグループ化して、[AEM ページエディターで使用できるようにします](#update-the-templates-device-group)。

`/ui.apps/src/main/content/jcr_root` の下に `/apps/settings/mobile/groups/<name of device group>` ノード構造を作成します。

![デバイスグループの新規作成](./assets/responsive-breakpoints/create-new-device-group.jpg)

`/apps/settings/mobile/groups/<device group name>` に `.content.xml` ファイルを作成し、
以下のようなコードを使用して新しいエミュレータを定義します。

![デバイスの新規作成](./assets/responsive-breakpoints/create-new-device.jpg)

## テンプレートのデバイスグループの更新

最後に、デバイスグループをページテンプレートにマッピングし戻し、このテンプレートから作成されたページのページエディターでエミュレーターを使用できるようにします。

`ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` ファイルをを開き、`cq:deviceGroups` プロパティを更新して、新しいモバイルグループ（例：`cq:deviceGroups="[mobile/groups/customdevices]"`）を参照します。
