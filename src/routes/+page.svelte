<script lang="ts">
    import { writable, derived, type Writable} from "svelte/store";
    import data from "../data/questions.json";
    
    type Question = {
        title: string,
        anserws: string[],
        correctAnserw: number
    }

    let points = 0;
    let allQuestions: Writable<Question[]> = writable(data);
    let currentQuestionIndex = writable(3);
    let currentQuestion = createDerivedStore();

    function createDerivedStore() {
        const { subscribe } = derived([allQuestions, currentQuestionIndex], ([$allQuestions, $currentQuestionIndex]) => $allQuestions.at($currentQuestionIndex));
        return {
            subscribe
        }
    }
    function handleAnserwClick( ) {
        currentQuestionIndex.update(n => ++n % $allQuestions.length)
    }
</script>

<main class="flex p-8 flex-col gap-8 items-center"> 
    <div class="w-1/3 min-w-[400px] px-8 py-6 bg-blue-100 text-2xl rounded-xl">
        <h1 class="font-bold mb-4">{$currentQuestion?.title}</h1>
        <div class="pl-4 flex flex-col items-start gap-1">
            {#each $currentQuestion?.anserws as anserw}
            <button class="mb-1 text-lg bg-blue-50 hover:bg-blue-200 duration-150 ease-in-out w-1/2 text-left p-2 px-3 rounded-lg" on:click={() => handleAnserwClick()}>{anserw}</button>
            {/each}
        </div>
    </div>
</main>