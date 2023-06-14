---
title: フォーム送信時に「ありがとうございます」メッセージを表示
description: onSubmitSuccess ハンドラーを使用して、設定済みの「ありがとうございます」メッセージを React アプリに表示します
feature: Adaptive Forms
version: 6.5
kt: 13490
topic: Development
role: User
level: Intermediate
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# 設定済みの「ありがとうございます」メッセージを表示します

フォームの送信に関する「ありがとうございます」メッセージは、フォームの入力と送信に関するユーザーへの感謝の気持ちを込めた丁寧な方法です。 これは、彼らの投稿を受け取り、評価されたことを確認する役割を果たします。 「ありがとうございます」メッセージは、アダプティブフォームのガイドコンテナの「送信」タブを使用して設定します

![ありがとうございました](assets/thank-you-message.png)

設定された「ありがとうございます」メッセージは、AdaptiveForm スーパーコンポーネントの onSuccess イベントハンドラーでアクセスできます。
onSuccess イベントを関連付けるためのコードと onSuccess イベントハンドラのコードを以下に示します

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

連絡先機能コンポーネントの完全なコードを以下に示します

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

上記のコードでは、ネイティブの HTML コンポーネントが使用され、アダプティブフォームで使用されるコンポーネントにマッピングされます。 例えば、アダプティブフォームのテキスト入力コンポーネントを TextField コンポーネントにマッピングするとします。 記事で使用されるネイティブコンポーネント [ここからダウンロードできます](./assets/native-components.zip)

