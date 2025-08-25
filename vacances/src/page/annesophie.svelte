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
<main>
    <section class="quiz-container">
        {#if questions.length > 0}
            {#if !quizFinished}
                <h1 class="title">Question {currentQuestionIndex +1}/{questions.length}</h1>
                <p class="question">{questions[currentQuestionIndex].question}</p>
                <button class="reponse" on:click={() => checkAnswer('reponse1')}>{questions[currentQuestionIndex].reponse1}</button>
                <button class="reponse" on:click={() => checkAnswer('reponse2')}>{questions[currentQuestionIndex].reponse2}</button>
                <button class="reponse" on:click={() => checkAnswer('reponse3')}>{questions[currentQuestionIndex].reponse3}</button>
            {:else}
                <h1 class="end-quiz">Quiz terminé !</h1>
                <p class="result">ton score : {score}/{questions.length}</p>
                <button on:click={restartQuiz}>Recommencer</button>
            {/if}
            {:else}
                <p class="loading">Chargement des questions...</p>
        {/if}
    </section>
</main>

<style>

.quiz-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 80vh;
    gap: 2rem;
    background: rgb(35, 153, 173);
    color: white;
}
.title,
.question,
.reponse {
        border-radius: 0.8rem;
    padding: 2rem;

}
.title {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: xx-large;
    font-weight: bold;
    background: rgb(4, 109, 122);
    margin-top: 5rem;
}
.question {
    background: rgb(4, 79, 92);
    margin-top: 3rem;
    font-size: x-large;
}
.reponse {
    background: rgb(71, 157, 172);
    border: none;
    color: white;
    font-size: large;
}
</style>