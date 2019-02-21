<template>
    <div class="main" id="main">
        <my-header></my-header>
        <div class="input-area">
            <input type="text" v-model="translateTxt" @input="detectLanguage">
            <select name="" id="" v-model="selected">
                <option v-for="(item,index) in langs" :value="item.code" :key="index">{{item.lang}}</option>
            </select>
            <input type="button" value="TRANSLATE" @click="translateText">
        </div>
        <p></p>
        <show-result :result="translateResult"></show-result>
    </div>
</template>

<script>
import myHeader from './my-header.vue'
import showResult from './show-result.vue'
export default {
    name:"Main",
    data(){
        return{
            langs:[],
            translateTxt:"",
            translateTxtLang:"",
            selected:"en",
            translateResult:"",
            loading:true
        }
    },
    methods:{
        getLanguageList(){
            this.axios.get("/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20190125T022104Z.3235ec78c545d752.aab2cda069d6453dffb2b0c7fc6fd576956d5962&ui=en")
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
            // 不使用拦截器
            let instance=this.axios.create();
            this.axios.interceptors.request.eject(instance);
            instance.get("/api/v1.5/tr.json/detect?key=trnsl.1.1.20190125T022104Z.3235ec78c545d752.aab2cda069d6453dffb2b0c7fc6fd576956d5962&text="+this.translateTxt)
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
            // 不使用拦截器
            let instance=this.axios.create();
            this.axios.interceptors.request.eject(instance);
            instance.get("/api/v1.5/tr.json/translate?key=trnsl.1.1.20190125T022104Z.3235ec78c545d752.aab2cda069d6453dffb2b0c7fc6fd576956d5962&text="+this.translateTxt+"&lang="+this.translateTxtLang+"-"+this.selected)
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
    components:{
        myHeader,showResult
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
    font-size:18px;
    outline: none;
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
    font-size: 18px;
    outline:none;
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


