<script setup>
import { onMounted, watch, ref, reactive } from 'vue';
import Message from '../components/Message.vue';
import Gemini from '../components/Gemini.vue';
import Common from '../components/Common.vue';

const inputText = ref("")
let inputBox

const messages = reactive([])

async function sendMessage(){
    messages.push({
        role: "user",
        parts: [{text: inputText.value}]
    })
    inputText.value = ""
    setTimeout(()=>{ inputBox.style.height = 'auto';inputBox.style.height = inputBox.scrollHeight + 'px' },30)

    const res = await Gemini.post(messages)
    messages.push(res.msg)
}

// inputBox根据文本行数自动调整高度
watch(inputText, () => {
    inputBox.style.height = 'auto'
    inputBox.style.height = inputBox.scrollHeight + 'px'
})

onMounted(()=>{
    inputBox = document.getElementById("inputBox")
})

Common.bindEvent("onNewChat", ()=>{ messages.splice(0,messages.length) })
</script>

<template>

<div id="chatView">
    <div id="chatContents">
        <Message sender="model" :content="[{text: 'Hello, What can I do for you?'}]"></Message>
        <Message v-for="msg,i in messages" :sender="msg.role" :content="msg.parts" :key="i"></Message>
    </div>

    <div id="messageInput">
        <textarea id="inputBox" v-model="inputText" ref="inputBox" @keydown.enter.ctrl.prevent="sendMessage" placeholder="Enter Text" rows="1" cols="27"></textarea>
        <button @click="sendMessage"><img src="/public/icons/send.svg"></button>
    </div>

</div>
</template>

<style>
#chatView {
    display: flex;
    flex: 1;
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
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    width: 100%;
    overflow-y: auto;
    scroll-behavior: smooth;
}

</style>