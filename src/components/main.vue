<template>
    <div class="main">
        <div class="header">
            <h1>Word Translator</h1>
            <p>Powered By Vue.js</p>
        </div>
        <div class="input-area">
            <input type="text" v-model="translateTxt" @input="detectLanguage">
            <select name="" id="" v-model="selected">
                <option v-for="(item,index) in langs" :value="item.code" :key="index">{{item.lang}}</option>
            </select>
            <input type="button" value="TRANSLATE" @click="translateText">
        </div>
        <p></p>
        <div class="result">
            <span>翻译结果</span>
            <p>{{translateResult}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name:"Main",
    data(){
        return{
            langs:[],
            translateTxt:"",
            translateTxtLang:"",
            selected:"en",
            translateResult:""
        }
    },
    methods:{
        getLanguageList(){
            this.axios.get("https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20190125T022104Z.3235ec78c545d752.aab2cda069d6453dffb2b0c7fc6fd576956d5962&ui=en")
            .then((res)=>{
                let result=res.data.langs;
                // console.log(result);
                for(let item in result)
                {
                    this.langs.push({code:item,lang:result[item]});
                }
            })
            .catch((err)=>{
        
            })
        },
        detectLanguage(){
            this.axios.get("https://translate.yandex.net/api/v1.5/tr.json/detect?key=trnsl.1.1.20190125T022104Z.3235ec78c545d752.aab2cda069d6453dffb2b0c7fc6fd576956d5962&text="+this.translateTxt)
            .then((res)=>{
                let result=res.data;
                if(result.code==200)
                {
                    this.translateTxtLang=result.lang;
                }
            })
            .catch((err)=>{
               
            })
        },
        translateText(){
            this.axios.get("https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20190125T022104Z.3235ec78c545d752.aab2cda069d6453dffb2b0c7fc6fd576956d5962&text="+this.translateTxt+"&lang="+this.translateTxtLang+"-"+this.selected)
            .then((res)=>{
                let result=res.data;
                if(result.code==200)
                {
                    this.translateResult=result.text[0];
                }
            })
            .catch((err)=>{
                
            })
        }
    },
    mounted(){
        this.getLanguageList();
    }
}
</script>

<style  scoped>
.main{
    width:100%;
    height:100%;
}
.header{
    width:100%;
    height:20%;
    border-bottom:1px solid #e5e5e5;
}
.header>h1{
    font-size:50px;
}
.header>p{
    color:#999;
    font-size:24px;
}
.input-area{
    width:600px;
    height:80px;
    line-height:80px;
    background:#F5F4F6;
    border:1px solid #E9E7E9;
    border-radius:5px;
    margin:20px auto;
}
.input-area>input[type="text"]{
    width:200px;
    border:none;
    border-bottom:1px solid #999;
    background:transparent;
    font-size:16px;
}
.input-area>select{
    width:160px;
    background: transparent;
    border:none;
    border-bottom: 1px solid #999;
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    background:url("../assets/三角.png") no-repeat right center;
    background-size: 30px 30px;
    font-family: 'Courier New', Courier, monospace;
    font-size: 16px;
}
.input-area>input[type="button"]{
    width:100px;
    height:30px;
    background:#2092F5;
    box-shadow: 0 0 2px #999;
    text-align: center;
    font-size:14px;
    color:#fff;
    border:none;
    border-radius:4px;
    outline: none;
}
.result{
    position: relative;
    width:600px;
    margin:30px auto;
}
.result>span{
    font-size: 20px;
    font-weight: bold;
}
.result p{
    width:600px;
    height:200px;
    box-sizing: border-box;
    display:inline-block;
    padding:10px;
    border:1px solid #666;
    vertical-align: top;
    text-align: left;
    font-size: 20px;
}
</style>


