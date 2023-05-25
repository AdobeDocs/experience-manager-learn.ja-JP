---
title: 使用可能なフォームのリストからフォームを選択
description: リストフォーム API を使用したドロップダウンリストの入力
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# 入力するフォームをドロップダウンリストから選択します

ドロップダウンリストを使用すると、ユーザーにオプションのリストを表示する、コンパクトで整理された方法を提供できます。 ドロップダウンリスト内の項目には、 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![カード表示](./assets/forms-drop-down.png)

## ドロップダウンリスト

次のコードは、リストフォーム API 呼び出しの結果をドロップダウンリストに入力するために使用されました。 ユーザーの選択に基づいて、アダプティブフォームが表示され、ユーザーが入力して送信できます。 [マテリアル UI コンポーネント](https://mui.com/) は、このインターフェイスの作成に使用されています

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formPath, setFormPath] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The path is "+event.target.value) 
    
        setFormPath(event.target.value)
        console.log("The formPath"+ formPath);
     };
const getForm = async () =>
     {
        const resp = await fetch(`${formPath}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formPath]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formPath}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.path}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

このユーザーインターフェイスの作成では、次の 2 つの API 呼び出しが使用されました

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). フォームを取得する呼び出しは、コンポーネントがレンダリングされる際に 1 回だけおこなわれます。 API 呼び出しの結果は、 afForms 変数に格納されます。
上記のコードでは、 map 関数を使用して afForms を繰り返し処理し、 afForms 配列内の各アイテムに対して、 MenuItem コンポーネントを作成して Select コンポーネントに追加します。

* フォームを取得 — 次のエンドポイントに対して get 呼び出しがおこなわれます。 formPath は、ドロップダウンリストでユーザーが選択したアダプティブフォームへのパスです。 このGET呼び出しの結果は、 selectedForm に保存されます。

```
${formPath}/jcr:content/guideContainer.model.json`
```

* 選択したフォームを表示します。 次のコードは、選択したフォームの表示に使用されました。 AdaptiveForm 要素は、 aemforms/af-react-renderer npm パッケージで提供され、マッピングと formJson をプロパティとして想定します

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## 次の手順

[フォームをカードレイアウトで表示する](./display-forms-card-view.md)



