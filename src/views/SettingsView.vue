<script setup>
import ImageCropper from '../components/ImageCropper.vue';
import Common from '../components/Common.vue';
import { reactive } from 'vue';
import Gemini from '../components/Gemini.vue';

const settings = reactive(Common.config)

function sortModels(models){
    return models.sort((m1,m2) => {
            if(m1.modelName == Common.config.model) return -1
            if(m2.modelName == Common.config.model) return 1
            return m2.maxTokens - m1.maxTokens
        })
}

settings.supportModels = sortModels(settings.supportModels)

async function refreshModels(){
    const models = await Gemini.getModels()
    if(models)
        settings.supportModels = sortModels(models)
    else alert("获取模型失败！")
}

function selectModel(i){
    settings.model = settings.supportModels[i].modelName
    settings.maxTokens = settings.supportModels[i].maxTokens
    settings.temperature = settings.supportModels[i].temperature
    settings.topP = settings.supportModels[i].topP
    settings.topK = settings.supportModels[i].topK
}

async function saveNewConfig(){
    await Common.setConfig(settings)
    alert("保存成功！")
}

</script>

<template>
<div id="settingsView">
    <div class="column">
        <b>聊天头像: </b><br/>
        <p class="tip">显示在聊天界面的头像。</p>
        <ImageCropper :src="settings.avatar" :callback="(imgData)=>{ settings.avatar = imgData }"></ImageCropper>
    </div>

    <div class="column">
      <b>Gemini API 地址:</b><br/>
      <p class="tip">输入Gemini的API地址，或者第三方的Gemini API地址。</p>
      <input class="input" type="text" placeholder="https://generativelanguage.googleapis.com" v-model="settings.apiUrl"/>
    </div>

    <div class="column">
      <b>API Key: </b><br/>
      <p class="tip">输入你的api密钥，可以在 https://aistudio.google.com/app/apikey 申请</p>
      <input class="input" type="text" v-model="settings.apiKey"/>
    </div>

    <div class="column">
        <div style="display: flex;align-items: center;">
            <b>选择模型: </b> 
            <img @click="refreshModels" class="button" src="/public/icons/refresh.svg"/><br/>
        </div>
      <div class="modelsContainer">
        <div v-for="model,i in settings.supportModels" @click="selectModel(i)" :class="['model', model.modelName == Common.config.model?'select':'unselect']" :key="model.modelName">
            <b class="model-name" >{{ model.name }}</b>
            <p class="model-desc">{{ model.description }}</p>
            <p class="model-maxTokens"><b>{{ model.maxTokens.toLocaleString() }}</b> tokens</p>
        </div>
      </div>
      
    </div>

    <div class="column">
      <b>创造性: </b><br/>
      <p class="tip">0.0到1.0，值越高，生成的回答的多样性和创造性越高，但也会更不准确，更不可预测，通常选择0.5到1.0之间。</p>
      <input class="input" type="number" step="0.1" min="0" max="1" v-model="settings.temperature"/>
    </div>

    <div class="column">
      <b>流式传输: &nbsp;</b><input type="checkbox" v-model="settings.stream">
      <br/>
      <p class="tip">关闭后，需要等整个回答生成完毕才能收到回复。</p>
    </div>

    <div class="column">
      <b>Markdown: &nbsp;</b><input type="checkbox" v-model="settings.markdown">
      <br/>
      <p class="tip">使用markdown渲染输出结果。</p>
    </div>

    <div class="column">
      <b>内容折叠: </b><br/>
      <p class="tip">当对话内容的行数超过该值时，折叠内容，设置为0则不折叠。</p>
      <input class="input" type="number" step="1" min="0" max="999" v-model="settings.foldContent"/>
    </div>

    <button class="primary" @click="saveNewConfig" style="margin: 10px; padding: 10px 20px;"><b>保存</b></button>
</div>

</template>

<style scoped>
#settingsView {
    display: flex;
    flex-grow: 1;
    box-sizing: border-box;
    flex-direction: column;
    height: 100%;
    padding: 20px;
    align-items: flex-start;
    background-color: rgb(254,244,238);
    overflow-x: hidden;
    overflow-y: auto;
    scroll-behavior: smooth;
}

.column {
    margin: 2.5vh 1vw;
    width: 100%;
}

.column> * {
    margin:1vh 0;
    font-size: 1.1rem;
}

.tip {
    color:rgb(120,120,120);
    font-size: 0.9rem;
}

.input {
  font-size: 0.9rem;
  width: 90%;
  padding: 7px 8px;
  border-radius: 5px 5px;
  background-color: #ffffffa0;
  border:2px solid rgba(220, 220, 220, 0.453);
  /* caret-color: gainsboro; */
}

.input:focus,.input:hover {
  border-color:rgb(252,198,188);
  outline:none;
}

.modelsContainer {
    display: flex;
    width: 100%;
    padding-bottom: 8px;
    overflow-x: auto;
    scroll-behavior: smooth;
}

.model {
    display: flex;
    flex-direction: column;
    min-width: 200px;
    padding: 16px;
    margin-right: 8px;
    border: 2px solid rgb(252,198,188);
    border-radius: 10px;
    cursor: url("/public/icons/cursor/Link.cur"),auto;
}

.model.select {
    background-color: rgb(252,198,188);
}

.model > * {
    cursor: url("/public/icons/cursor/Link.cur"),auto;
}

.model:hover {
    background-color: rgb(252,198,188,0.5);
}

.model-name {
    display:-webkit-box;
    -webkit-line-clamp:2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
}

.model-name:hover {
    -webkit-line-clamp:5;
}

.model-desc {
    flex: 1;
    color:rgb(120,120,120);
    font-size: .8rem;
    margin: 8px 0;
    hyphens: auto;
}

.model-maxTokens {
    color: rgb(120,120,120);
    font-size: 1rem;
}

.model-maxTokens > b {
    color: rgb(208, 140, 128);
}

@media screen and (max-width:500px){
    #settingsView{ padding-top: 65px;}
}

</style>