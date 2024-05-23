<script>
import { set,get } from 'idb-keyval';
import { reactive } from 'vue';


const config = reactive({
    avatar: "",
    apiUrl: "https://generativelanguage.googleapis.com",
    apiKey: "",
    model: "gemini-1.5-flash-latest",
    supportModels: [],
    maxTokens: "1048576",
    temperature: 1,
    topP: 0.95,
    topK: 64,
    stream: false,
    markdown: false,
    foldContent: 0
})

async function loadConfig(){
    for(let key in config){
        let value = await get(key)
        if(value){
            if(typeof config[key] == "object")
                config[key] = JSON.parse(value)
            else
                config[key] = value
        }
    }

    if(config.avatar == "")
        config.avatar = "icons/default_avatar.jpg"

    console.log("loadConfig", config)
}

async function saveConfig(){
    for(let key in config){
        if(typeof config[key] == "object")
            await set(key, JSON.stringify(config[key]))
        else 
            await set(key, config[key])
    }

    console.log("saveConfig", config)
}

async function setConfig(newConfig){
    for(let key in newConfig){
        if((typeof config[key]) == (typeof newConfig[key]))
            config[key] = newConfig[key]
    }
    await saveConfig()
}

const events = {}
function bindEvent(eventName, callback){ events[eventName] = callback }
function triggerEvent(eventName, ...params){ return events[eventName](params) }
async function triggerEventAsync(eventName, ...params){ return await events[eventName](...params) }

loadConfig().then()

export default {
    config,
    loadConfig,
    saveConfig,
    setConfig,
    bindEvent,
    triggerEvent,
    triggerEventAsync,
    name: "Common"
}

</script>