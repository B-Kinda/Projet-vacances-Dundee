<script>
import { onMount } from 'svelte';
import PocketBase from 'pocketbase';

 // --- Variables ---

let pb = new PocketBase('http://127.0.0.1:8090');
let questions = [];
let currentQuestionIndex = 0;
let score = 0;
let quizFinished = false;
let answered = false;
let selectedAnswer = null;

 // --- Récupérer les questions depuis PocketBase ---

onMount(async () => {
    const result = await pb.collection('questions').getFullList();
    console.log('Questions récupérées :', result);
    questions = result;
});

// --- Vérifier la réponse et passer à la question suivante ---

function checkAnswer(answerKey) {
    selectedAnswer = answerKey;
    answered = true;
    if (answerKey === questions[currentQuestionIndex].bonne_reponse) {
        score++;
    }
}
function nextQuestion() {
    answered = false;
    selectedAnswer = null;
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
                <button class="reponse"
                class:reponse--answered={answered}
                class:reponse--correct={answered && questions[currentQuestionIndex].bonne_reponse === 'reponse1'}
                class:reponse--wrong={answered && selectedAnswer === 'reponse1' && selectedAnswer !== questions[currentQuestionIndex].bonne_reponse}
                on:click={() => checkAnswer('reponse1')}
                disabled={answered}>{questions[currentQuestionIndex].reponse1}</button>
                <button class="reponse"
                class:reponse--answered={answered}
                class:reponse--correct={answered && questions[currentQuestionIndex].bonne_reponse === 'reponse2'}
                class:reponse--wrong={answered && selectedAnswer === 'reponse2' && selectedAnswer !== questions[currentQuestionIndex].bonne_reponse}
                on:click={() => checkAnswer('reponse2')}
                disabled={answered}>{questions[currentQuestionIndex].reponse2}</button>
                <button class="reponse"
                class:reponse--answered={answered}
                class:reponse--correct={answered && questions[currentQuestionIndex].bonne_reponse === 'reponse3'}
                class:reponse--wrong={answered && selectedAnswer === 'reponse3' && selectedAnswer !== questions[currentQuestionIndex].bonne_reponse}
                on:click={() => checkAnswer('reponse3')}
                disabled={answered}>{questions[currentQuestionIndex].reponse3}</button>
                    {#if answered}
                        <p>{selectedAnswer === questions[currentQuestionIndex].bonne_reponse ? "Bonne réponse !"
                        : `Mauvaise réponse. La bonne réponse était : ${questions[currentQuestionIndex][questions[currentQuestionIndex].bonne_reponse]}`}</p>
                        <button class="next" on:click={nextQuestion}>{currentQuestionIndex < questions.length - 1 ? "Suivant" : "Voir le score"}</button>
                    {/if}
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
// reste à faire le CSS pour la partie 82-84 du main //
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
.reponse,
.next {
    border-radius: 0.8rem;
    padding: 2rem;
    border: none;
    font-weight: bold;
}
.title {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: xx-large;
    background: rgb(4, 109, 122);
    margin-top: 3rem;
}
.question {
    background: rgb(4, 79, 92);
    margin-top: 1rem;
    font-size: x-large;
}
.reponse,
.next {
    color: white;
    font-size: large;
}
.reponse {
    background: rgb(71, 157, 172);
}
.reponse--answered {
    opacity: 0.7;
    cursor: none;
}
.reponse--correct {
    background: #09a777;
    color: #fff;
}
.reponse--wrong {
    background: #e73c3c;
    color: #fff;
}
.next {
    background: rgb(4, 167, 179);
}
</style>