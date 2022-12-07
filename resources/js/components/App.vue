<template>
    <div style="width: 500px">
        <h1>The quiz</h1>

        <section class="quiz" v-if="!quizCompleted">
            <div class="quiz-info">
                <span class="question">{{ getCurrentQuestion.question }}</span>
                <span class="score">{{ score }}/{{ questions.length }}</span>
            </div>

            <div class="options">
                <label
                    v-for="(option, index) in getCurrentQuestion.options"
                    :key="index"
                    :class="`option ${
                        getCurrentQuestion.selected == index
                            ? index == getCurrentQuestion.answer
                                ? 'correct'
                                : 'wrong'
                            : ''
                    } ${
                        getCurrentQuestion.selected != null &&
                        index != getCurrentQuestion.selected
                            ? 'disabled'
                            : ''
                    }`"
                >
                    <input
                        type="radio"
                        :name="getCurrentQuestion.index"
                        id=""
                        :value="index"
                        v-model="getCurrentQuestion.selected"
                        :disabled="getCurrentQuestion.selected"
                        @change="SetAnswer"
                    />
                    <span>{{ option }}</span>
                </label>
            </div>

            <button
                @click="NextQuestion"
                :disabled="!getCurrentQuestion.selected"
            >
                {{
                    getCurrentQuestion.index == questions.length - 1
                        ? "Finish"
                        : getCurrentQuestion.selected == null
                        ? "Select an option"
                        : "Next question"
                }}
            </button>
        </section>

        <section v-else>
            <h2>You finished the quiz</h2>
            <p>Your score is {{ score }}/{{ questions.length }}</p>
        </section>
    </div>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { computed } from "@vue/runtime-core";

const questions = ref([
    {
        question: "What is Vue?",
        answer: 0,
        options: [
            "A frontend framework",
            "A cinema chain",
            "A conspiracy",
            "King of Cheese",
        ],
        selected: null,
    },
    {
        question: "What is VueX?",
        answer: 0,
        options: ["A state management tool"],
        selected: null,
    },
    {
        question: "What is Vue Router?",
        answer: 0,
        options: ["A state management tool"],
        selected: null,
    },
]);
const quizCompleted = ref(false);
const currentQuestion = ref(0);

/**
 * Calculate the score based on the correct answers selected by the user.
 * Where value is the selected value.
 */
const score = computed(() => {
    let value = 0;

    questions.value.map((quest) => {
        if (quest.selected == quest.answer) {
            value++;
        }
    });

    return value;
});

/**
 * Retrieve the current question to display.
 */
const getCurrentQuestion = computed(() => {
    let question = questions.value[currentQuestion.value];
    question.index = currentQuestion.value;

    return question;
});

/**
 * Set the answer for a question.
 */
const SetAnswer = (evt) => {
    questions.value[currentQuestion.value].selected = evt.target.value;
    evt.target.value = null;
};

/**
 * Figure out what the next question is.
 */
const NextQuestion = () => {
    if (currentQuestion.value < questions.value.length - 1) {
        currentQuestion.value++;
    } else {
        quizCompleted.value = true;
    }
};

// export default {};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Montserrat", sans-serif;
}

body {
    background-color: #271c36;
    color: #fff;
}

#app {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    min-height: 100vh;
}

h1 {
    font-size: 2rem;
    margin-bottom: 2rem;
}

.quiz {
    background: #382a4b;
    padding: 1rem;
    max-width: 500px;
}

.quiz-info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
}

.quiz-info .question {
    color: #fff;
    font-size: 1.25rem;
}

.quiz-info .score {
    color: #fff;
    font-size: 1.25rem;
}

.options {
    margin-bottom: 1rem;
}

.option:hover {
    background-color: blanchedalmond;
}

.option.correct {
    background-color: green;
}

.option.wrong {
    background-color: red;
}

.option input {
    display: none;
}

.option {
    display: block;
    padding: 1rem;
    cursor: pointer;
    margin-bottom: 0.5rem;
    border-radius: 0.5rem;
    background-color: deeppink;
}
</style>
