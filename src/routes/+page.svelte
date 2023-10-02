<!-- JS -->

<script>

    import { onMount } from 'svelte'
    
    let poem = "THis is the test zodat ik kan zien zo dit is een test en nog een desteigeere kshdflls";
    let voted_words = [];
    let timer = 10;
    
    setInterval(() => {
        timer -= 1; 
        if (timer < 0) {
            let mostVotes = {"word": "_","votes": 0};
            words.forEach(word => {
                if(word.votes > mostVotes.votes){
                    mostVotes = word;
                }
            });
            for (let i = 0; i < words.length; i++){
                words[i].votes = 0;
            }
            poem = poem + " " + mostVotes.word;
            timer = 10;
        }
    }, 1000);

    
    let words = [];

    onMount(async () => {
        try{
            const response = await fetch("/src/routes/output.json");
            words = await response.json();
        } catch (error) {
            console.log("Couldn't get words because error: ", error);
        }
    });
    
    function chooseWord(_word){
        if(words.includes(_word)){
            words[words.indexOf(_word)].votes++;
        }
    }
</script>

<!--HTML-->

<h6>{timer}</h6>

<p>{poem}</p>


<div id="voted_words">
    {#each words as word}
        {#if word.votes > 0}
            <div id="voted_word">
                <p>{word.votes}</p>
                <button on:click={() => chooseWord(word)}>{word.word}</button>
            </div>
        {/if}
    {/each}
</div>

<div id="not_voted_words">
    {#each words as word}
        {#if word.votes === 0}
            <button on:click={() => chooseWord(word)}>{word.word}</button>
        {/if}
    {/each}
</div>



<!--CSS-->

<style>
    *{
        font-family: 'Libre Franklin', sans-serif;
    }
    h6{
        right:2px;
        top:2px;
        position: absolute;
    }
    button {
        padding: 9px;
        margin: 5px;
    }

    p {
        text-align: center;
        margin: 160px;
    }

    #voted_word p{
        text-align: center;
        padding: 0;
        margin: 0;
    }
    
    #voted_word{
        display: inline-block;
        margin-top: 10px;
    }

    #voted_words{
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        margin: 60px 0px;
    }
    
    #not_voted_words{
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }
</style>