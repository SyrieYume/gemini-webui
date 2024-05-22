<script setup>
import default_avatar from "../assets/icons/default_avatar.jpg"
import gemini_sparkle from "../assets/icons/gemini_sparkle.svg"
import { onMounted,reactive } from "vue"

const props = defineProps({
    sender: String,
    content: String
})

let avatarSrc = reactive(gemini_sparkle)

onMounted(()=>{
    if(props.sender == "user")
        avatarSrc = default_avatar
    else
        avatarSrc = gemini_sparkle
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