<template>
<div id="sidebar">
    <button id="menuBtn"><img src="../assets/icons/menu.svg"></button><br/>

    <b style="margin-left: 10px;">Historys</b>

    <div id="historys" style="flex:1;">

    </div>

    <router-link to="/">
        <button id="newChatBtn">
            <img src="../assets/icons/add.svg" style="width: 24px;margin-right: 10px;">
            <b>New Chat</b>
        </button>
    </router-link>
    
    <br/>

    <router-link to="/settings">
        <button id="settingsBtn">
            <img src="../assets/icons/settings.svg" style="width: 24px;margin-right: 10px;">
            <b>Settings</b>
        </button>
    </router-link>
</div>
</template>

<script setup>

</script>

<style scoped>
#sidebar {
    display: flex;
    flex-direction: column;
    flex-grow: 0;
    width: 18%;
    min-width: 50px;
    height: 100%;
    padding: 15px;
    box-sizing: border-box;
    background-color: rgb(252,233,218);
}

#menuBtn {
    width: 50px;
    height: 50px;
    justify-content: center;
}

#newChatBtn {
    background-color: rgb(252,198,188);
    width: 100%;
}

#settingsBtn {
    width: 100%;
}
</style>