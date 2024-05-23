<template>
<div id="sidebar">
    <button id="menuBtn"><img src="/public/icons/menu.svg"></button><br/>

    <b style="margin-left: 10px;">Historys</b>

    <div id="historys">
        <div class="button sidebarBtn fullwidth" v-for="history in historysList" @click="Common.triggerEventAsync('onLoadHistory', history.id)" :key="history.id" style="justify-content: space-between;">
            <p>{{ history.name }}</p>
            <img @click="deleteHistory(history.id)" src="/public/icons/delete.svg"/>
        </div>
    </div>

    <br/>

    <button class="sidebarBtn fullwidth" @click="newHistory(Common.triggerEvent('getMsgs'))">
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
import Common from '../components/Common.vue'
import { historysList, newHistory, deleteHistory} from "../components/History.vue"
</script>

<style scoped>
#sidebar {
    display: flex;
    flex-direction: column;
    width: 18%;
    min-width: 250px;
    height: 100%;
    padding: 15px;
    padding-bottom: 40px;
    box-sizing: border-box;
    background-color: rgb(252,233,218);
}

#menuBtn {
    width: 50px;
    height: 50px;
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
    padding: 5px 5px 5px 20px;
    overflow-y: auto;
    scroll-behavior: smooth;
}

</style>