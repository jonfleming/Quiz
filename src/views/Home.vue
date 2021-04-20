<template>
  <ion-page>
    <ion-header :translucent="true">
        <ion-title>Driver Knowledge Practice Test</ion-title>
    </ion-header>
    \<ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col>
            <ion-note color="secondary">(52 Questions)</ion-note><br/>
            <ion-button v-if="!started && !done" v-on:click="start()">Start</ion-button>
            <Question v-if="started && !done" v-bind:question="questionList[questionNumber]" @correct="increaseScore()" @incorrect="wrongAnswer"/>
            <ion-note color="red" v-if="wrong">Wrong. The correct answer is:<br/> {{answer}}<br/></ion-note>
            <ion-note v-if="started && !done">{{correct}}/{{questionNumber - 1}}</ion-note>
            <ion-note v-if="done">Your got {{correct}} out of {{questionList.length - 1}}</ion-note>
            <ion-button v-if="started && done" v-on:click="restart()">Try Again</ion-button>
          </ion-col>
        </ion-row>
      </ion-grid>    
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonButton, IonHeader, IonPage, IonTitle, IonNote, IonGrid, IonRow, IonCol } from '@ionic/vue';
import Question from '@/components/Question.vue';
import { defineComponent } from 'vue';
import { getQuestions } from '@/data/questions';

export default defineComponent({
  name: 'Home',
  data() {
    return {
      questionList: getQuestions(),
      started: false as boolean,
      correct: 0 as number,
      questionNumber: 0 as number,
      done: false as boolean,
      answer: '' as string,
    }
  },
  computed: {
    wrong(): boolean {
      return this.answer.length !== 0;
    }
  },
  methods: {
    refresh: (ev: CustomEvent) => {
      setTimeout(() => {
        ev.detail.complete();
      }, 3000);
    },
    start(): void {
      this.started = true;
      this.restart();
    },
    restart(): void {
      this.correct = 0;
      this.questionNumber = 1;
      this.done = false;
    },
    increaseScore(): void {
      console.log('increasing score');
      this.correct++;
      this.answer = '';
      this.nextQuestion();
    },
    wrongAnswer(text: string): void {
      console.log(`Received incorrect: ${text}`);
      this.answer = text;
      this.nextQuestion();
    },
    nextQuestion(): void {
      this.questionNumber++;

      if (this.questionNumber == this.questionList.length) {
        this.done = true;
      }
    }
  },
  components: {
    Question,
    IonContent,
    IonHeader,
    IonButton,
    IonPage,
    IonTitle,
    IonNote,
    IonGrid,
    IonRow,
    IonCol
  },
});
</script>