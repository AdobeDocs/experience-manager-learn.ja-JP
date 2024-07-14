---
title: Edge Delivery Services 用のローカル開発環境を設定
description: Edge Delivery Services 用のローカル開発環境の設定方法。
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 100%

---

# ローカル開発環境を設定

Edge Delivery Services 開発用のローカル開発環境の設定方法。

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## ビデオで説明されている手順

1. AEM CLI をインストールします。

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. ディレクトリを、[AEM ボイラープレート](https://github.com/adobe/aem-boilerplate)テンプレートから作成した git リポジトリであるプロジェクトディレクトリに変更します。

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. AEM CLI を実行して、ローカルの AEM インスタンスを起動します。

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. Web ブラウザーで http://localhost:3000/ を開いて、AEM web サイトを表示します。
