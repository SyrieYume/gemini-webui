<script>
import { set,get,del } from 'idb-keyval';
import { reactive } from 'vue';

let historysList = reactive([])

async function loadHistorysList(){
    const listStr = await get("historysList")
    const list = JSON.parse(listStr??"[]")
    historysList.splice(0,historysList.length)
    list.forEach(history => { historysList.push(history) });
    console.log("loadHistorysList", historysList)

}

async function newHistory(historyData){
    const id = `his-${Date.now().toString(16)}`
    historysList.push({
        id,
        name: id
    })
    await set(id, JSON.stringify(historyData))
    await set("historysList", JSON.stringify(historysList))
    console.log("newHistory", id, historysList, historyData)
}

async function loadHistory(id){
    let data = await get(id)
    if(data)
        data = JSON.parse(data)
    console.log("loadHistory", id, data)
    return data
}

async function deleteHistory(id){
    historysList.splice(historysList.indexOf(id), 1)
    del(id)
    await set("historysList", JSON.stringify(historysList))
}

loadHistorysList().then()

export {
    historysList,
    newHistory,
    loadHistory,
    deleteHistory
}

</script>