<template>
<div id="sidebar">
    <button id="menuBtn"><img src="/public/icons/menu.svg"></button><br/>

    <b style="margin-left: 10px;">Historys</b>

    <div id="historys" style="flex:1;">

    </div>

    <router-link to="/">
        <button id="newChatBtn" class="primary fullwidth" @click="Common.triggerEvent('onNewChat')">
            <img src="/public/icons/add.svg" style="width: 24px;margin-right: 10px;">
            <b>New Chat</b>
        </button>
    </router-link>
    
    <br/>

    <router-link to="/settings">
        <button id="settingsBtn" class="fullwidth">
            <img src="/public/icons/settings.svg" style="width: 24px;margin-right: 10px;">
            <b>Settings</b>
        </button>
    </router-link>
</div>
</template>

<script setup>
import Common from '../components/Common.vue';
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

</style>