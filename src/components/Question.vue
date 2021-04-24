<template>
  <div class="question">
    <ion-grid>
      <ion-row>
        <ion-col>
        <ion-item-divider>
          <ion-label class="ion-text-wrap">{{question[0]}}. &nbsp; {{question[1]}}</ion-label>
        </ion-item-divider>
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>
          <ion-radio-group v-model="answer">
            <p><ion-radio type="radio" id="1" value="1"></ion-radio>
              <ion-label class="ion-text-wrap" >&nbsp;{{question[2]}}</ion-label>
            </p>
            <p><ion-radio type="radio" id="2" value="2"></ion-radio>
              <ion-label class="ion-text-wrap" >&nbsp;{{question[3]}}</ion-label>
            </p>
            <p><ion-radio type="radio" id="3" value="3"></ion-radio>
              <ion-label class="ion-text-wrap" >&nbsp;{{question[4]}}</ion-label>
            </p>
            <p v-if="question[5].length"><ion-radio id="4" value="4"></ion-radio>
              <ion-label class="ion-text-wrap" >&nbsp;{{question[5]}}</ion-label>
            </p>
          </ion-radio-group>
        </ion-col>
      </ion-row>
    </ion-grid>      
    <ion-button v-on:click="next()">Next</ion-button>
  </div>
</template>

<script>
import { IonButton, IonLabel, IonItemDivider, IonGrid, IonRow, IonCol, IonRadio, IonRadioGroup } from '@ionic/vue';
export default {
  name: 'Question',
  props: {
    question: {}    
  },
  emits: ['correct', 'incorrect'],
  data() {
    return {
      answer: ''
    }
  },
  methods: {
    next() {
      console.log(`Answer ${this.answer}.  Correct Answer: ${this.question[6]}`);
      if (this.answer == this.question[6]) {
        console.log('emitting correct');
        this.$emit('correct');
      } else {
        const correctAnswer = Number(this.question[6]) + 1; // first answer is element[2]
        const text = this.question[correctAnswer];
        console.log(`emitting incorrect with answer[${this.question[6]}] = ${correctAnswer}: ${text}`);
        this.$emit('incorrect', text);
      }

      this.answer = '';
    }
  },
  components: {
    IonButton,
    IonLabel,
    IonItemDivider,
    IonGrid,
    IonRow,
    IonCol,
    IonRadio,
    IonRadioGroup,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ion-text-wrap {
  font-size: 18px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  text-align: left;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
