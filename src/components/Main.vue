<script setup>
import { onMounted, ref } from 'vue';
import data from '../utils/data.json';
import { getRandomElements, shuffle } from "../utils/index.js";

const info = data.data;

const concepts = ref([]);
const explanation = ref([]);
concepts.value = Object.keys(info);
explanation.value = Object.values(info);

const previousConcept = ref("");
const activeConcept = ref("");
const explanations = ref([]);

const trueExp = ref("");
const falseExp = ref([]);
const finalObj = ref({});

const answers = ref([]);

const getConcept = (latestConcept) => {
  const newConcept = concepts.value[Math.ceil(Math.random()*(concepts.value.length-1))];
  
  if(newConcept === latestConcept) {
    getConcept(newConcept);
  } else {
    activeConcept.value = newConcept;
  }
  trueExp.value = info[activeConcept.value]
}

const getExplanations = () => {
  explanations.value = shuffle([...getRandomElements(explanation.value, 4), info[activeConcept.value]]);

  finalObj.value = {};
  for(let i=0; i < explanations.value.length; i++) {
    finalObj.value[explanations.value[i]] = info[activeConcept.value] == explanations.value[i];
  }
}

function chceckAnswer(exp) {
  if(answers.value.includes(trueExp.value)) {
    return;
  }
  answers.value.push(exp);
}

onMounted(() => { getConcept(previousConcept.value); getExplanations(); });
</script>
<template>
  <div class="w-1/5 whole-dev">
    <div class="w-full h-fit bg-slate-800 px-4 py-6 rounded-lg text-slate-200 justify-center flex text-lg">
      {{ activeConcept }}
    </div>
    <div class="w-full text-slate-200 mt-8 exps">
      <div
        v-for="exp in explanations"
        :key="exp"
        class="px-4 py-2 my-3 rounded-md cursor-pointer exp"
        :class="answers.includes(exp) ? finalObj[exp] == true ? 'bg-green-600 cursor-default' : 'bg-red-700 cursor-default' : 'bg-slate-600 cursor-pointer'"
        @click="chceckAnswer(exp)"
        >
        {{ exp }}
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@media only screen and (max-width: 600px) {
  .whole-dev {
    width: 80%;
  }
}
</style>