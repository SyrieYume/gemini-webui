<template>
<PopupEditor v-if="editedHistory>=0" 
    title="编辑名称" 
    :initContent="historysList[editedHistory].name" 
    :onSave="(newName) => renameHistory(historysList[editedHistory].id,newName)" 
    :onDismiss="()=>{editedHistory = -1}" />

<button id="menuBtn" @click="switchSidebarShow()">
    <img src="/public/icons/menu.svg">
</button>

<div id="sidebar">
    
    <b style="margin-left: 10px;">Historys</b>

    <br/>

    <div id="historys">
        <div v-for="history,i in historysList" 
            @click="Common.triggerEventAsync('onLoadHistory', history.id)" 
            :class="['button','fullwidth', currentHistory == history.id?'primary':'']" :key="history.id" style="justify-content: space-between;">
            
            <p>{{ history.name }}</p>
            <div>
                <img @click.stop="editedHistory = i" src="/public/icons/edit.svg" style="padding-right: 5px;"/>
                <img @click.stop="deleteHistory(history.id)" src="/public/icons/delete.svg"/>
            </div>
            
        </div>
    </div>

    <br/>

    <button class="sidebarBtn fullwidth" @click="saveHistory(Common.triggerEvent('getMsgs'))">
        <img src="/public/icons/save.svg">
        <b>Save Hstory</b>
    </button>

    <br/>

    <router-link to="/">
        <button class="sidebarBtn primary fullwidth" @click="Common.triggerEvent('onNewChat')">
            <img src="/public/icons/add.svg">
            <b>New Chat</b>
        </button>
    </router-link>
    
    <br/>

    <router-link to="/settings">
        <button class="sidebarBtn fullwidth">
            <img src="/public/icons/settings.svg">
            <b>Settings</b>
        </button>
    </router-link>
</div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Common from '../components/Common.vue'
import PopupEditor from '../components/popupEditor.vue';
import { historysList, saveHistory, deleteHistory, currentHistory, renameHistory} from "../components/History.vue"

const editedHistory = ref(-1)
let sidebar

function switchSidebarShow(){
    const display = sidebar.style.display
    sidebar.style.display = (display == "flex")?"none":"flex"
}

onMounted(()=>{
    sidebar = document.getElementById("sidebar")
})

</script>

<style scoped>
#sidebar {
    z-index: 9997;
    display: flex;
    flex-direction: column;
    width: 20vw;
    min-width: 250px;
    height: 100%;
    padding: 15px;
    padding-top: 65px;
    padding-bottom: 40px;
    box-sizing: border-box;
    background-color: rgb(252,233,218);
}

#menuBtn {
    z-index: 9998;
    position: fixed;
    width: 50px;
    height: 50px;
    left: 0;
    top: 0;
    margin: 5px;
    box-sizing: border-box;
    justify-content: center;
}

.sidebarBtn > img {
    width: 24px;
    margin-right: 10px;
}

#historys {
    display: flex;
    flex-direction: column;
    flex: 1;
    width: 100%;
    padding: 5px 5px 5px 10px;
    overflow-y: auto;
    scroll-behavior: smooth;
}

#historys > div {
    margin-bottom: 4px;
}

#historys > div > p {
    color: #404040;
    padding-left: 10px;
}

@media screen and (max-width:500px){
    #sidebar {
        display: none;
        position: absolute;
    }
}

</style>