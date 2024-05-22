<script setup>
import { onMounted, watch, ref } from 'vue';
import Message from '../components/Message.vue';

const inputText = ref("")
let inputBox

// inputBox根据文本行数自动调整高度
watch(inputText, () => {
    inputBox.style.height = 'auto'
    inputBox.style.height = inputBox.scrollHeight + 'px'
})

onMounted(()=>{
    inputBox = document.getElementById("inputBox")
})

</script>

<template>

<div id="chatView">
    <div id="chatContents">
        <Message sender="model" content="Hello, What can I do for you?"></Message>
        <Message sender="user" content="test content"></Message>
    </div>

    <div id="messageInput">
        <textarea id="inputBox" v-model="inputText" ref="inputBox" placeholder="Enter Text" rows="1" cols="27"></textarea>
        <button><img src="/public/icons/send.svg"></button>
    </div>

</div>
</template>

<style>
#chatView {
    display: flex;
    flex-grow: 1;
    flex-shrink: 0;
    flex-direction: column;
    height: 100%;
    padding: 20px;
    box-sizing: border-box;
    align-items: center;
    background-color: rgb(254,244,238);
}

#messageInput {
    flex-shrink: 1;
    width:80%;
    box-sizing: border-box;
    text-align: right;
    border-radius: 20px;
    padding:15px 15px 10px 15px;
    background-color: rgb(252,233,218);
}

#inputBox {
    width:100%;
    height: auto;
    max-lines: 5;
    box-sizing: border-box;
    word-wrap: break-word;
    font-size: 1.1rem;
    background: none;
    border: none;
    outline: none;
    resize: none;
}

#chatContents {
    flex: 1;
    width: 80%;
    overflow-y: auto;
    scroll-behavior: smooth;
}

</style>