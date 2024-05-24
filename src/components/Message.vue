<script setup>
import { computed, onMounted, ref } from "vue"
import useClipboard from 'vue-clipboard3'
import Common from "./Common.vue";
import {Marked} from 'marked';
import { markedHighlight } from "marked-highlight"
import hljs from 'highlight.js'
import "highlight.js/styles/github.css";

const { config } = Common
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
    breaks: true,
    pedantic: false,
    smartLists: true,
    sanitize: true,
    smartypants: true
})

const displayContent = computed(() => {
    text.value = props.msg.parts.map((part)=>{ return part.text }).join("").trim()
    if(config.markdown && props.msg.role == "model")
        return marked.parse(text.value)
    else 
        return text.value
})

const contentRef = ref(null)
const shouldFold = ref(false)
const isFold = ref(false)

function foldContent(){
    isFold.value = shouldFold.value && !isFold.value
}

function copyToClipboard(){
    useClipboard().toClipboard(text.value)
}

onMounted(()=>{
    if(config.foldHeight > 0 && contentRef.value.getBoundingClientRect().height > config.foldHeight)
        shouldFold.value = true
    foldContent()
})

</script>

<template>
<div class="message">
    <img class="fold-more-icon button" @click="foldContent()" src="/public/icons/more.svg" v-if="isFold"/>
    <img class="fold-more-icon button" @click="foldContent()" src="/public/icons/more.svg" v-else-if="shouldFold" style="transform: rotate(180deg);"/>

    <img class="avatar" @click="foldContent()" :src="msg.role=='user'?config.avatar:'icons/gemini_sparkle.svg'"/>

    <div v-if="config.markdown && msg.role=='model'" 
        ref="contentRef"
        :class="{'content':true,'markdown-body':true }" 
        :style="{'max-height': isFold?`${config.foldHeight}px`:''}" 
        v-html="displayContent">
    </div>

    <div v-else 
        ref="contentRef"
        :class="{ 'content':true }"
        :style="{'max-height': isFold?`${config.foldHeight}px`:''}" 
        v-text="displayContent">
    </div>

    <div class="tools">
        <img src="/public/icons/copy.svg" @click.stop="copyToClipboard()" />
        <img src="/public/icons/edit.svg" @click.stop="onEdit()" />
        <img src="/public/icons/delete.svg" @click.stop="onDelete()" />
    </div>
</div>
</template>

<style scoped>
.message {
    position: relative;
    width: 100%;
    align-items: flex-start;
    padding: 5px 10px 20px 37px;
    box-sizing: border-box;
}

.message > .avatar {
    position: absolute;
    width: 32px;
    height: 32px;
    left: 5px;
    top: 5px;
    border-radius: 50%;
    margin: 5px;
}

.message > .content {
    /* display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow-x: auto;
    text-overflow: ellipsis;
    flex: 1; */
    display: block;
    overflow-y: auto;
    scroll-behavior: smooth;
    width: 100%;
    margin: 12px 5px 12px 20px;
    padding: 2px;
    padding-right: 20px;
    font-size: 1rem;
    white-space: pre-line;
    box-sizing: border-box;
    color: #313237;
    font-family: "宋体";
    font-weight: 550;
    background: none;
    
}

.message:hover {
    background-color: rgb(252,198,188,0.25);
    
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


.markdown-body {
    white-space: normal !important;
}

.fold-more-icon {
    z-index: 100;
    display: none;
    position: absolute;
    bottom: 0;
    left: 0;
}

.message:hover > .fold-more-icon {
    display: block;
}


@media screen and (max-width:500px){
    .message { width: 100%; }
    .message > .content { margin: 5px 10px; padding-right: 5px; }
    .message > .avatar { left: 0; top: 0;}
}
</style>