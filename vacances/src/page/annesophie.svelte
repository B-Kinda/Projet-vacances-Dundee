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
            <div class="reponse-container">
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
            </div>
                    {#if answered}
                        <p class="good-bad">{selectedAnswer === questions[currentQuestionIndex].bonne_reponse ? "Bonne réponse !"
                        : `Mauvaise réponse. La bonne réponse était : ${questions[currentQuestionIndex][questions[currentQuestionIndex].bonne_reponse]}`}</p>
                        <button class="next" on:click={nextQuestion}>{currentQuestionIndex < questions.length - 1 ? "Suivant" : "Voir le score"}</button>
                    {/if}
                {:else}
                <h1 class="end-quiz">Quiz terminé !</h1>
                <p class="result">ton score : {score}/{questions.length}</p>
                <button class="restart" on:click={restartQuiz}>Recommencer</button>
            {/if}
            {:else}
                <p class="loading">Chargement des questions...</p>
        {/if}
    </section>
</main>

<style>

.quiz-container {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 80vh;
    gap: 2rem;
    background-image: url(../../images/background-quiz.jpg);
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
    background-size: 85% auto;
    color: white;
}
.title,
.question,
.reponse,
.next,
.end-quiz,
.result,
.restart {
    border-radius: 0.8rem;
    padding: 2rem;
    border: none;
    font-weight: bold;
}
.title,
.reponse,
.result {
    background: rgb(27, 5, 93);
}
.title,
.end-quiz,
.result,
.restart {
    font-size: xx-large;
    margin-top: 2rem;
}
.question,
.end-quiz {
    background: rgb(63, 193, 231);
}
.next,
.restart {
    background: rgb(253, 146, 251);
}
.title,
.end-quiz {
    margin-top: 8rem;
}
.title,
.question {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.question {
    margin-top: 2rem;
    font-size: x-large;
}
.reponse-container {
    display: flex;
    flex-direction: row;
    gap: 1rem;
    justify-content: center;
}
.reponse,
.next {
    color: white;
    font-size: large;
}
.reponse {
    display: flex;
    flex-wrap: nowrap;
    margin-top: 2rem;
}
.good-bad {
    font-size: x-large;
}
.reponse--answered {
    opacity: 0.7;
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
    color: white;
}
.restart {
    color: white;
}
</style>