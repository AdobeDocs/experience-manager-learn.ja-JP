---
title: AEM Forms と Marketo（第 4 部）
description: AEM Forms フォームデータモデルを使用した AEM Forms と Marketo の統合に関するチュートリアル
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 68
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: ht
source-wordcount: '278'
ht-degree: 100%

---

# 統合のテスト

シンプルなフォーム取得を作成して統合をテストし、マーケットから Lead オブジェクトを表示します。
>[!NOTE]
>
>この機能は、基盤コンポーネントに基づくフォームでテストされました。

## アダプティブフォームの作成

1. 「空のフォームテンプレート」に基づいてアダプティブフォームを作成して、以前の手順で作成したフォームデータモデルに関連付けます。
1. フォームを編集モードで開きます。
1. テキストフィールドコンポーネントとパネルコンポーネントをアダプティブフォームにドラッグ＆ドロップします。テキストフィールドコンポーネントのタイトルを「リード ID を入力」に設定し、その名前を「LeadId」に設定します
1. 2 つのテキストフィールドコンポーネントをパネルコンポーネントにドラッグ＆ドロップします。
1. 2 つのテキストフィールドコンポーネントの名前とタイトルを「First Name」と「Last Name」として設定します。
1. 最小値を 1 に、最大値を -1 に設定して、パネルコンポーネントを繰り返し可能なコンポーネントとして設定します。これが必要なのは、Marketo サービスがリードオブジェクトの配列を返すので、結果を表示するために繰り返し可能なコンポーネントが必要になるからです。ただし、この場合、ID でリードオブジェクトを検索しているので、返されるリードオブジェクトは 1 つだけです。
1. 以下の画像に示すように、「LeadId」フィールドに関するルールを作成します。
1. フォームをプレビューし、有効なリード ID を「LeadId」フィールドに入力してタブアウトします。「First Name」フィールドと「Last Name」フィールドに、サービス呼び出しの結果が入力されます。

次のスクリーンショットでは、ルールエディターの設定を説明しています。

![ruleeditor](assets/ruleeditor.png)


## これで完了です

AEM Forms フォームデータモデルを使用して、AEM Forms を Marketo と正常に統合しました。