<template>
    <div class="result">
        <span>翻译结果</span>
        <p>{{result}}</p>
    </div>
</template>

<script>
export default {
    name:"showResult",
    props:["result"]
}
</script>

<style scoped>
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

