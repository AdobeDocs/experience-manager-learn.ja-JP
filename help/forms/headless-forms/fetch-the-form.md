---
title: 埋め込むアダプティブフォームの JSON を取得
description: API を使用してアダプティブフォームの JSON を取得する
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
ht-degree: 1%

---

# フォームの JSON を取得

AEM Formsオーサーインスタンスにログインし、 **コアコンポーネントで空白** テンプレート。 フォームをパブリッシュインスタンスに発行します。

フォームを埋め込むには、まずパブリッシュサーバーに対して get 呼び出しを実行して、アダプティブフォームの json を取得します。

次のコードスニペットは、という名前のアダプティブフォームの json を取得します。 **接触**

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

上記のコードでは、ネイティブの HTML コンポーネントが使用され、アダプティブフォームで使用されるコンポーネントにマッピングされます。 例えば、アダプティブフォームのテキスト入力コンポーネントを TextField コンポーネントにマッピングするとします。 記事で使用されるネイティブコンポーネント [ここからダウンロードできます](./assets/native-components.zip)

## 次の手順

[ドロップダウンリストからフォームを選択](./select-form-from-drop-down-list.md)
