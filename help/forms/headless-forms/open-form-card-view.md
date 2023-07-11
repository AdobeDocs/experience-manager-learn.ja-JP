---
title: カードのクリックによるフォームの表示
description: カード表示からフォームにドリルダウンします
feature: Adaptive Forms
version: 6.5
kt: 13372
topic: Development
role: User
level: Intermediate
exl-id: e7cc7e22-9413-4820-9a52-d63ab29aff09
source-git-commit: d926bb35623e8d3bb8dd564e07d3cb394342e367
workflow-type: ht
source-wordcount: '61'
ht-degree: 100%

---

# カードクリック時のフォームの表示

ユーザーがカードをクリックしたときにフォームを表示するために、次のコードを使用しました。表示するフォームのパスは、useParams 関数を使用して URL から抽出されます。

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`/adobe/forms/af/${params.formID}`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```

## 次の手順

[フォーム送信時に「ありがとうございます」メッセージを表示](./display-thank-you-message.md)
