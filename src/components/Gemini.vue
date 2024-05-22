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

async function post(messages){
    const url = `${Common.config.apiUrl}/v1beta/models/${Common.config.model}:generateContent?key=${Common.config.apiKey}`
    const headers = {'Content-Type': 'application/json'}
    const data = {
        contents: messages,
        safetySettings: [
            {"category": "HARM_CATEGORY_HATE_SPEECH", "threshold": "BLOCK_NONE"},
            {"category": "HARM_CATEGORY_SEXUALLY_EXPLICIT", "threshold": "BLOCK_NONE"},
            {"category": "HARM_CATEGORY_HARASSMENT", "threshold": "BLOCK_NONE"},
            {"category": "HARM_CATEGORY_DANGEROUS_CONTENT", "threshold": "BLOCK_NONE"}
        ]
    }

    try {
        const res = await axios.post(url, data, {headers})
        if(res.status == 200){
            const candidate = res.data.candidates[0]
            if("content" in candidate)
                return {
                    msg: res.data.candidates[0].content,
                    tokenUsage: res.data.usageMetadata.totalTokenCount
                }
            else return {
                msg: `finishReason: ${candidate.finishReason}`,
                tokenUsage: res.data.usageMetadata.totalTokenCount
            }
        }
        else return {
            msg: {parts: [{text: `error! status ${res.status}`}], role: "model"},
            tokenUsage: 0
        }
    }catch(error){
        return {
            msg: {parts: [{text: error.toString()}], role: "model"},
            tokenUsage: 0
        }
    }
    

}

export default {
    getModels,
    post,
    name: "Gemini"
}
</script>

