<p>{translatedText}</p>

<script>
	import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let textToTranslate;
    var translatedText = ''
    // https://stackoverflow.com/questions/29775797/fetch-post-json-data
    async function translateText(textToTranslate){
        console.log('translating text ' + textToTranslate)
        fetch('https://translate.kaustubh.app/translate', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
        },
            body: JSON.stringify({TEXT : textToTranslate})
        })
        .then(res => res.json())
        .then(data => {
            const translated = data.translated
            console.log(translated)
            translatedText = translated
            dispatch('translated',{ translated })        
        })
        .catch(error => {
            console.log(error)
        })  
    }

    // Text with a ! prefix are test queries, not to be sent to API
    if(textToTranslate[0] === '!'){
        const translated = textToTranslate
        translatedText = translated
        setTimeout(() => dispatch('translated',{ translated }), 1000)
    }else{
        translateText(textToTranslate)
    }
</script>

<style>

</style>