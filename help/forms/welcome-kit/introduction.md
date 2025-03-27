---
title: ウェルカムキットの作成
description: 送信されたフォームデータに基づいてアセットをダウンロードするためのリンクを含む、AEM Sites のページを作成します。
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 7aba25d1-0d4d-4c49-8132-f844a288e8f3
duration: 19
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '101'
ht-degree: 100%

---

# ウェルカムキット

このチュートリアルでは、送信されたフォームデータに基づいて様々なアセットをダウンロードするためのリンクを使用して、AEM ページを組み立てる方のに役立ちます。サンプルコードを使用して、新規顧客が関連ドキュメントをダウンロードできるようにするウェルカムキットを生成したり、リクエストされたドキュメントをダウンロードするためのリンクをを使用して AEM ページを生成したりできます。

## 前提条件

以下が必要です。

フォームアドオンパッケージがインストールされた AEM の実用的なインスタンス
インストール済みのコアコンポーネント

[このドキュメントに従って設定された開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=ja)
