<script setup>
import { computed, onMounted, ref } from "vue"
import Common from "./Common.vue";
import { marked } from "marked";

const props = defineProps(["msg", "onDelete"])

const text = ref("")

const displayContent = computed(() => {
    text.value = props.msg.parts.map((part)=>{ return part.text }).join("").trim()
    if(Common.config.markdown)
        return marked(text.value)
    else 
        return text.value
})


</script>

<template>
<div class="message">
    <img class="avatar" :src="msg.role=='user'?Common.config.avatar:'icons/gemini_sparkle.svg'"/>
    <p class="content foldContent" v-html="displayContent"></p>
    <div class="tools">
        <img src="/public/icons/copy.svg" />
        <img src="/public/icons/edit.svg" />
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
    color: #555861;
    font-family: "宋体";
    font-weight: 500;
    
}

.message:hover {
    background-color: rgb(252,198,188,0.4);
    
}

.message > .tools {
    display: none;
    position: absolute;
    flex-direction: column;
    top: 0;
    left: 100%;
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

.foldContent:hover {
    -webkit-line-clamp:999;
}
</style>