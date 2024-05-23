<script setup>
import {defineProps, ref, defineModel } from "vue"
const props = defineProps(["initContent", "rows", "onSave", "title", "onDismiss"])
const content = ref(props.initContent??"")

</script>

<template>
<div class="popupEditor">
    <b>{{ title }}</b>
    <textarea v-model="content" class="editor" :rows="rows??8"></textarea>
    <div>
        <button @click="onSave(content);onDismiss()" class="primary"><b>保存</b></button>
        <button @click="onDismiss()" style="margin-left: 20px;"><b>取消</b></button>
    </div>
</div>
</template>

<style>
.popupEditor {
    z-index: 9999;
    display: flex;
    position: fixed;
    padding: 10px;
    min-width: 25vw;
    flex-direction: column;
    align-items: center;
    caret-color: #39c5bccc;
    border-radius: 10px;
    background-color: white;
    box-shadow: 2px 1px 2px 4px rgb(252,198,188, 0.2);
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}

.popupEditor > * {
    margin: 5px 10px;
}

.popupEditor > .editor {
    width: 100%;
    padding: 10px;
    border-radius: 10px;
    caret-color: #ED9282;
    border: 2px dashed #ED9282;
    resize: none;
    outline: none;
}

@media screen and (max-width:500px){
    .popupEditor {min-width: 75vw;}
}
</style>