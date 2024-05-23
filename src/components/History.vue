<script>
import { set,get,del } from 'idb-keyval';
import { reactive,ref,watch } from 'vue';

const historysList = reactive([])
const currentHistory = ref("")

async function loadHistorysList(){
    const listStr = await get("historysList")
    const list = JSON.parse(listStr??"[]")
    historysList.splice(0,historysList.length)
    list.forEach(history => { historysList.push(history) });
    console.log("loadHistorysList", historysList)

}

async function saveHistory(historyData){
    if(currentHistory.value == ""){
        currentHistory.value  = `his-${Date.now().toString(16)}`
        historysList.push({
            id: currentHistory.value,
            name: currentHistory.value
        })
    }
    
    await set(currentHistory.value, JSON.stringify(historyData))
    await set("historysList", JSON.stringify(historysList))
    console.log("saveHistory", currentHistory.value, historysList, historyData)
}

async function renameHistory(id, newName){
    const index = historysList.findIndex((history) => history.id == id)
    if(index < 0) return
    historysList[index].name = newName
    await set("historysList", JSON.stringify(historysList))

}

async function loadHistory(id){
    let data = await get(id)
    if(data)
        data = JSON.parse(data)
    console.log("loadHistory", id, data)
    return data
}

async function deleteHistory(id){
    historysList.splice(historysList.findIndex((history) => history.id == id), 1)
    del(id)
    await set("historysList", JSON.stringify(historysList))
    if(currentHistory.value == id)
        currentHistory.value = ""
    console.log("delHistory", id)
}

loadHistorysList().then()

export {
    historysList,
    currentHistory,
    saveHistory,
    loadHistory,
    deleteHistory,
    renameHistory
}

</script>