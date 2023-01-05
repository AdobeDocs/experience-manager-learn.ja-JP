---
title: レスポンシブブレークポイント
description: AEMレスポンシブページエディター用に新しいレスポンシブブレークポイントを設定する方法を説明します。
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# レスポンシブブレークポイント

AEMレスポンシブページエディター用に新しいレスポンシブブレークポイントを設定する方法を説明します。

## CSS ブレークポイントの作成

まず、レスポンシブAEMサイトが準拠するAEMレスポンシブグリッド CSS でメディアのブレークポイントを作成します。

In `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` ファイルに、モバイルエミュレーターと共に使用するブレークポイントを作成します。 をメモして `max-width` ブレークポイントごとに、CSS のブレークポイントをAEMレスポンシブページエディターのブレークポイントにマッピングします。

![新しいレスポンシブブレークポイントの作成](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## テンプレートのブレークポイントのカスタマイズ

を開きます。 `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` ファイルと更新 `cq:responsive/breakpoints` 新しいブレークポイントノードの定義を使用します。 各 [CSS ブレークポイント](#create-new-css-breakpoints) の下に対応するノードがある `breakpoints` その `width` プロパティを CSS ブレークポイントの `max-width`.

![テンプレートのレスポンシブブレークポイントのカスタマイズ](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## エミュレーターの作成

作成者がページエディターで編集するレスポンシブビューを選択できるように、AEMエミュレーターを定義する必要があります。

の下に emulators ノードを作成します。 `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

（例：`/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`）。次の参照エミュレーターノードをコピーします。 `/libs/wcm/mobile/components/emulators` ノードの定義を早めるために、にCRXDE Liteし、コピーを更新します。

![新しいエミュレーターの作成](./assets/responsive-breakpoints/create-new-emulators.jpg)

## デバイスグループを作成

エミュレーターを次のようにグループ化します。 [AEM Page Editor で使用できるようにします。](#update-the-templates-device-group).

作成 `/apps/settings/mobile/groups/<name of device group>` 下のノード構造 `/ui.apps/src/main/content/jcr_root`.

![新しいデバイスグループを作成](./assets/responsive-breakpoints/create-new-device-group.jpg)

の作成 `.content.xml` ～に入る `/apps/settings/mobile/groups/<device group name>` 次のようなコードを使用して、新しいエミュレーターを定義します。

![新しいデバイスを作成](./assets/responsive-breakpoints/create-new-device.jpg)

## テンプレートのデバイスグループを更新する

最後に、デバイスグループをページテンプレートにマッピングし、このテンプレートから作成されたページでエミュレーターをページエディターで使用できるようにします。

を開きます。 `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` ファイルを作成し、 `cq:deviceGroups` 新しいモバイルグループを参照するプロパティ ( 例： `cq:deviceGroups="[mobile/groups/customdevices]"`)
