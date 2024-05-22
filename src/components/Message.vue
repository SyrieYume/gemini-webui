<script setup>
import { computed, onMounted, ref } from "vue"
import Common from "./Common.vue";
import { marked } from "marked";

const props = defineProps({
    sender: String,
    content: Array,
})

const displayContent = computed(() => {
    const text = props.content.map((part)=>{ return part.text }).join("").trim()
    if(Common.config.markdown)
        return marked(text)
    else 
        return text
})


</script>

<template>
<div class="message">
    <img class="avatar" :src="sender=='user'?Common.config.avatar:'icons/gemini_sparkle.svg'"/>
    <p class="content foldContent" v-html="displayContent"></p>
</div>
</template>

<style scoped>
.message {
    display: flex;
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
    white-space: pre-wrap;
    box-sizing: border-box;
    color: #555861;
    font-family: "宋体";
    font-weight: 500;
    
}

.message:hover {
    background-color: rgb(252,198,188,0.4);
    
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