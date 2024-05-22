<script>
import axios from 'axios';
import Common from './Common.vue';

async function getModels(){
    const url = `${Common.config.apiUrl}/v1beta/models?key=${Common.config.apiKey}`
    const headers = {'Content-Type': 'application/json'}

    const res = await axios.get(url, {headers})
    if(res.status == 200){
        return res.data.models.map(model => { return {
            "name": model.displayName,
            "modelName": model.name.split("/")[1],
            "description": model.description,
            "maxTokens": model.inputTokenLimit,
            "temperature": model.temperature,
            "topP": model.topP,
            "topK": model.topK
        }})
    }
}

export default {
    getModels,
    name: "Gemini"
}
</script>

