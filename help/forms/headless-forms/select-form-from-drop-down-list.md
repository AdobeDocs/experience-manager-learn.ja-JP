---
title: 使用可能なフォームのリストからフォームを選択
description: リストフォーム API を使用したドロップダウンリストの入力
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
exl-id: 31008bb3-316b-4035-89ea-e830b429b927
source-git-commit: 529e98269a08431152686202a8a2890712b9c835
workflow-type: tm+mt
source-wordcount: '286'
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

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
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
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
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

* フォームを取得 — GET 呼び出しが [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition)（ id は、ドロップダウンリストでユーザーが選択したアダプティブフォームの id） このGET呼び出しの結果は、 selectedForm に保存されます。

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* 選択したフォームを表示します。 次のコードは、選択したフォームの表示に使用されました。 AdaptiveForm 要素は、 aemforms/af-react-renderer npm パッケージで提供され、マッピングと formJson をプロパティとして想定します

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## 次の手順

[フォームをカードレイアウトで表示する](./display-forms-card-view.md)
