<script setup>
import { onMounted, ref } from "vue"
import Common from "./Common.vue";

const props = defineProps({
    sender: String,
    content: String,
    tokenUsage: Number,
    maxTokens: Number
})

let avatarSrc = ref("")

onMounted(()=>{
    if(props.sender == "user")
        avatarSrc.value = Common.config.avatar
    else
        avatarSrc.value = "icons/gemini_sparkle.svg"
})

</script>

<template>
<div class="message">
    <img class="avatar" :src="avatarSrc"/>
    <p class="content">{{ content }}</p>
</div>
</template>

<style scoped>
.message {
    display: flex;
    align-items: flex-start;
    margin-bottom: 30px;
}

.message > .avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    margin: 5px;
}

.message > .content {
    margin: 12px 5px 12px 20px;
    font-size: 1.2rem;
    white-space: pre;
    box-sizing: border-box;
    color: #555861;
}

</style>