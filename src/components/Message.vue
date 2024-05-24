<script setup>
import { computed, onMounted, ref } from "vue"
import Common from "./Common.vue";
import {Marked} from 'marked';
import { markedHighlight } from "marked-highlight"
import hljs from 'highlight.js'
import "highlight.js/styles/github.css";

const props = defineProps(["msg", "onDelete", "onEdit"])

const text = ref("")

const marked = new Marked(
    markedHighlight({
        langPrefix: 'hljs language-',
        highlight(code, language) {
            return hljs.highlightAuto(code).value;
        }
    })
)

marked.setOptions({
    renderer: new marked.Renderer(),
    gfm: true,
    tables: true,
    breaks: false,
    pedantic: false,
    smartLists: true,
    sanitize: true,
    smartypants: false
})

const displayContent = computed(() => {
    text.value = props.msg.parts.map((part)=>{ return part.text }).join("").trim()
    if(Common.config.markdown && props.msg.role == "model")
        return marked.parse(text.value)
    else 
        return text.value
})

</script>

<template>
<div class="message">
    <img class="avatar" :src="msg.role=='user'?Common.config.avatar:'icons/gemini_sparkle.svg'"/>

    <div v-if="Common.config.markdown && msg.role=='model'" 
        class="content foldContent markdown-body" 
        :style="{'-webkit-line-clamp': Common.config.foldContent>0?Common.config.foldContent:999}" 
        v-html="displayContent">
    </div>

    <div v-else class="content foldContent" 
        :style="{'-webkit-line-clamp': Common.config.foldContent>0?Common.config.foldContent:999}" 
        v-text="displayContent">
    </div>

    <div class="tools">
        <img src="/public/icons/copy.svg" />
        <img src="/public/icons/edit.svg" @click="onEdit()" />
        <img src="/public/icons/delete.svg" @click="onDelete()" />
    </div>
</div>
</template>

<style scoped>
.message {
    display: flex;
    position: relative;
    width: 80%;
    align-items: flex-start;
    margin-bottom: 20px;
}

.message > .avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    margin: 5px;
}

.message > .content {
    flex: 1;
    margin: 12px 5px 12px 20px;
    padding: 2px;
    font-size: 1rem;
    white-space: pre-line;
    box-sizing: border-box;
    color: #313237;
    font-family: "宋体";
    font-weight: 550;
    background: none;
    
}

.message:hover {
    background-color: rgb(252,198,188,0.4);
    
}

.message > .tools {
    display: none;
    position: absolute;
    flex-direction: row;
    bottom: 0;
    right: 0;
    margin: 0px;
    padding: 4px;
}

.tools > img {
    width: 28px;
    height: 28px;
    margin: 2px;
    padding: 4px;
    border-radius: 50%;
    background-color: #ffffffA0;
    box-shadow: 2px 2px 2px 1px #00000020;
}

.tools > img:hover {
    background-color: rgb(252,198,188,0.5);
}

.message:hover > .tools, .tools:hover {
    display: flex;
}

.foldContent {
    display:-webkit-box;
    -webkit-line-clamp:10;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
}

.message:hover > .foldContent {
    -webkit-line-clamp:999 !important;
}

.markdown-body {
    white-space: normal !important;
}

@media screen and (max-width:500px){
    .message{ width: 100%; }
    .message > .content { margin: 5px; }
}
</style>