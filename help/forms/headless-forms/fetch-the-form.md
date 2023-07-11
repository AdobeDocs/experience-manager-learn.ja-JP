---
title: 埋め込むアダプティブフォームの JSON の取得
description: API を使用したアダプティブフォームの JSON の取得
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
exl-id: 5953a1ad-0eaf-43f0-b356-6d20c0b59fee
source-git-commit: 529e98269a08431152686202a8a2890712b9c835
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 100%

---

# フォームの JSON の取得

AEM Forms オーサーインスタンスにログインし、**コアコンポーネントを使用した空白**&#x200B;テンプレートを使用して新しいアダプティブを作成します。フォームをパブリッシュインスタンスに公開します。

フォームを埋め込むには、まずパブリッシュサーバーに対して GET 呼び出しを実行して、アダプティブフォームの JSON を取得します。

次のコードスニペットは、**contactus** という名前のアダプティブフォームの JSON を取得します

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

連絡先機能コンポーネントの完全なコードを以下に示します

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

上記のコードでは、アダプティブフォームで使用されるコンポーネントにマッピングされるネイティブ HTML コンポーネントを使用します。例えば、アダプティブフォームのテキスト入力コンポーネントを TextField コンポーネントにマッピングするとします。この記事で使用されているネイティブコンポーネントは、[こちらからダウンロードできます](./assets/native-components.zip)

## 次の手順

[ドロップダウンリストからのフォームの選択](./select-form-from-drop-down-list.md)
