<script setup>
import Sidebar from './views/Sidebar.vue';
</script>

<template>
<Sidebar />
<!-- 内容区域 -->
<router-view v-slot="{ Component }">
<!-- keep-alive 保持每个页面的状态不会因为路由变化而丢失 -->
<keep-alive>
    <component :is="Component"/>
</keep-alive>
</router-view>

</template>


<style scoped>

</style>
