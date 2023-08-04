<template>
  <main>
    <span class="prompt">{{ prompts[activeTab] }}...</span>
    <div class="questions">
      <div
        class="group"
        v-show="activeTab === groupIndex"
        v-for="(questionGroup, groupIndex) in questions"
      >
        <p v-for="(question, questionIndex) in questionGroup">
          <span v-for="token in question">
            <span
              @click="unveal(token, question[0], questionIndex)"
              :class="{
                tag: isTag(token, question[0], questionIndex),
              }"
              >{{ token }}</span
            >&nbsp;
          </span>
        </p>
      </div>
    </div>
    <br />
    <button @click="previousTab()">Previous</button>&nbsp;&nbsp;&nbsp;
    <button @click="nextTab()">Next</button>
  </main>
</template>

<script setup>
import { ref } from 'vue';
const activeTab = ref(0);
const unvealed = ref([]);

const unveal = (token, question, questionIndex) => {
  unvealed.value.push(`${token} ${question} ${questionIndex}`);
  socket.emit('unveal', `${token} ${question} ${questionIndex}`);
};

const isTag = (token, question, questionIndex) => {
  return (
    ![question, 'jon sandman'].includes(token) &&
    !unvealed.value.includes(`${token} ${question} ${questionIndex}`)
  );
};

const previousTab = () => {
  activeTab.value = activeTab.value > 0 ? activeTab.value - 1 : 0;
};

const nextTab = () => {
  activeTab.value = activeTab.value < 6 ? activeTab.value + 1 : 6;
};

const prompts = [
  'is jon sandman',
  'how jon sandman',
  'who jon sandman',
  'why jon sandman',
  'where jon sandman',
  'what jon sandman',
  'when jon sandman',
];

const questions = ref([
  [
    ['is', 'jon sandman', 'in spacestation gaming'],
    ['is', 'jon sandman', 'married'],
    ['is', 'jon sandman', 'good at rocket league'],
  ],
  [
    ['how', 'is', 'jon sandman', 'grand champ'],
    ['how', 'old is', 'jon sandman'],
    ['how', 'tall is', 'jon sandman'],
    ['how', 'much does', 'jon sandman', 'make'],
  ],
  [
    ['who', 'is', 'jon sandman', 'married to'],
    ['who', 'is', 'jon sandman'],
  ],
  [['why', 'is', 'jon sandman', 'so bad']],
  [
    ['where', 'does', 'jon sandman', 'live'],
    ['where', 'is', 'jon sandman', 'from'],
  ],
  [
    ['what', 'is', 'jon sandman', 'real name'],
    ['what', 'is', 'jon sandman', 'rank'],
    ['what', 'is', 'jon sandman', 'net worth'],
    ['what', 'happened to', 'jon sandman'],
  ],
  [
    ['when', 'does', 'jon sandman', 'return'],
    ['when', 'does', 'jon sandman', 'die'],
  ],
]);
</script>

<style scoped>
main {
  background-image: url('./assets/bg.png');
  background-repeat: no-repeat;
  background-position: center 50px;
  background-size: 600px;
  min-height: 100vh;
  min-width: 600px;
  position: relative;
}

.questions {
  padding-left: 70px;
  padding-top: 230px;
}

p {
  font-size: 26px;
  text-align: left;
}

.prompt {
  position: absolute;
  top: 187px;
  font-size: 26px;
  left: 70px;
}

.tag {
  cursor: pointer;
  position: relative;

  &::after {
    content: '';
    border-radius: 5px;
    position: absolute;
    top: 0;
    left: 0;
    width: 105%;
    height: 42px;
    background-color: #666;
  }
}
</style>
