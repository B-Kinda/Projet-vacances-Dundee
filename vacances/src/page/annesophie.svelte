<script>
import { onMount } from 'svelte';
import PocketBase from 'pocketbase';

 // --- Variables ---

let pb = new PocketBase('http://127.0.0.1:8090');
let questions = [];
let currentQuestionIndex = 0;
let score = 0;
let quizFinished = false;

 // --- Récupérer les questions depuis PocketBase ---

onMount(async () => {
    const result = await pb.collection('questions').getFullList();
    console.log('Questions récupérées :', result);
    questions = result;
});

// --- Vérifier la réponse et passer à la question suivante ---

function checkAnswer(selectedAnswer) {
    if (selectedAnswer === questions[currentQuestionIndex].bonne_reponse) {
        score++;
    }
    if (currentQuestionIndex < questions.length -1) {
        currentQuestionIndex++;
    } else {
        quizFinished = true;
    }
}

// --- Recommencer le quiz ---

function restartQuiz() {
    currentQuestionIndex = 0;
    score = 0;
    quizFinished = false;
}
</script>

<!-- Interface du quiz -->

{#if questions.length > 0}
    {#if !quizFinished}
        <h1>Question {currentQuestionIndex +1}/{questions.length}</h1>
        <p>{questions[currentQuestionIndex].question}</p>
        <button on:click={() => checkAnswer('reponse1')}>{questions[currentQuestionIndex].reponse1}</button>
        <button on:click={() => checkAnswer('reponse2')}>{questions[currentQuestionIndex].reponse2}</button>
        <button on:click={() => checkAnswer('reponse3')}>{questions[currentQuestionIndex].reponse3}</button>
        {:else}
            <h1>Quiz terminé !</h1>
            <p>ton score : {score}/{questions.length}</p>
            <button on:click={restartQuiz}>Recommencer</button>
    {/if}
        {:else}
        <p>Chargement des questions...</p>
{/if}


<style>
</style>