<template>
    <div>
        <main>
            <h1>The Quiz</h1>
            <section class="quiz">
                <div class="quiz-info">
                    <span class="question">
                        {{ getCurrentQuestion.question }}</span
                    >
                    <div class="score">
                        Score {{ score }} / {{ questions.length }}
                    </div>
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
                            :value="index"
                            v-model="getCurrentQuestion.selected"
                            :disabled="getCurrentQuestion.selected"
                            @change="setAnswer"
                        />
                        <span>{{ option }}</span>
                    </label>
                </div>
                <button
                    @click="nextQuestion"
                    :disabled="!getCurrentQuestion.selected"
                >
                    {{
                        getCurrentQuestion.index == questions.length - 1
                            ? "Finish"
                            : getCurrentQuestion.selected == null
                            ? "Select an option"
                            : "Next Question"
                    }}
                </button>
            </section>
        </main>
        {{ quizCompleted }}
    </div>
</template>

<script setup>
import { ref, computed } from "vue";

const questions = ref([
    {
        question: "What is VueJS",
        answer: 0,
        options: ["A front end framework", "A library", "An ice cream maker"],
        selected: null
    },
    {
        question: "What is Vuex",
        answer: 2,
        options: ["Vue with an x", "A library", "State management library"],
        selected: null
    },
    {
        question: "What is Vue Router used for",
        answer: 1,
        options: [
            "Walking in space",
            "A routing library for VueJS",
            "Burger sauce",
            "Quiz"
        ],
        selected: null
    }
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

// Tính điểm
const score = computed(() => {
    let value = 0;
    questions.value.map((q) => {
        // So sánh lựa chọn đáp án với câu trả lời
        if (q.selected == q.answer && q.selected != null) {
            value++;
        }
    });
    return value;
});

// Câu hỏi hiện tại

// Lấy ra câu hỏi hiện tại
const getCurrentQuestion = computed(() => {
    // currentQuestion.value -> truy cập câu hỏi tương ứng trong mảng questions.value
    let question = questions.value[currentQuestion.value];

    question.index = currentQuestion.value;
    return question;
});

const setAnswer = (event) => {
    questions.value[currentQuestion.value].selected = event.target.value;
    event.target.value = null;

    console.log(questions.value);
};

const nextQuestion = () => {
    if (currentQuestion.value < questions.value.length - 1) {
        currentQuestion.value++;
    } else {
        quizCompleted.value = true;
    }
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: #271c36;
    color: #fff;
}
</style>
